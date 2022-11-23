# solid-auth-form
A Solid Start Auth strategy for working with forms. 

# How to use
First, install the strategy and Remix Auth. 
```sh
$ npm install solidjs-auth solidjs-auth-form
```

Then, create an Authenticator instance.
```ts
import { Authenticator } from "solidjs-auth";

// check out solidjs-auth documentation on how to create a sessionStorage
export let authenticator = new Authenticator<User>(sessionStorage);
```

Finally tell the Authenticator to use the form strategy
```ts
authenticator.use(
  new FormStrategy(async ({ form }) => {

    // Note that you can use fields other than email and password to validate!
    
    // get the data from the form...
    let email = form.get('email') as string;
    let password = form.get('password') as string;

    // initialize the user here
    let user = null;

    // do some validation, errors are in the sessionErrorKey
    if (!email || email?.length === 0) throw new AuthorizationError('Bad Credentials: Email is required')
    if (typeof email !== 'string')
      throw new AuthorizationError('Bad Credentials: Email must be a string')

    if (!password || password?.length === 0) throw new AuthorizationError('Bad Credentials: Password is required')
    if (typeof password !== 'string')
      throw new AuthorizationError('Bad Credentials: Password must be a string')

    // login the user, this could be whatever process you want. For example, accessing a database and validating data.
    if (email === 'example@mail.com' && password === 'password') {
      user = {
        name: email,
        token: `${password}-${new Date().getTime()}`,
      };

      // the type of this user must match the type you pass to the Authenticator
      // the strategy will automatically inherit the type if you instantiate
      // directly inside the `use` method
      return await Promise.resolve({ ...user });

    } else {
      // if problem with user throw error AuthorizationError
      throw new AuthorizationError("Bad Credentials")
    }

  }),
);

export default authenticator
```

This package is to be used with [solid-auth](https://github.com/orjdev/solid-auth). It adds the functionallity of being able to authenticate using a form, based on the equivalent package for remix called [remix-auth-form](https://github.com/sergiodxa/remix-auth-form).
