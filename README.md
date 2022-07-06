# gapi-cjs
A [gapi-script](https://github.com/partnerhero/gapi-script) alternative which uses commonjs module. Has some changes in the gapiScript so that it eliminates errors like:
- `this` is set to undefined
- jest error - import can't be used outside module
- using of eval directly

## Usage
```
npm i gapi-cjs
```

For gapi instance,
```javascript
import { gapi } from 'gapi-cjs'
```

You can create your own `useGoogleLogin` hook,
```javascript
import { gapi } from 'gapi-cjs';
import { useState, useEffect } from 'react';

export const useGoogleLogin = () => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    gapi.load('client:auth2', () => {
      gapi.client
        .init({
          clientId:
            '<client-id>',
          scope: 'openid',
        })
        .then(() => {
          const auth = gapi.auth2.getAuthInstance();
          setUser(auth.currentUser.get().getBasicProfile());
        });
    });
  }, []);

  const signIn = async () => {
    const auth = await gapi.auth2.getAuthInstance();
    await auth.signIn();
    const { access_token, id_token } = await auth.currentUser
      .get()
      .getAuthResponse();
    return { access_token, id_token };
  };

  const signOut = async () => {
    const auth = await gapi.auth2.getAuthInstance();
    await auth.signOut();
  };

  return { user, signIn, signOut };
};
```
