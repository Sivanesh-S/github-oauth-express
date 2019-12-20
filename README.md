# github-oauth-express

- A simple package to get the `access_token` of a Github account for a particular client. So that we can call Github's user specific APIs.
- Follow these steps to create a Github App and get `client_id`, `client_secret`.
  - [Login Github](https://github.com).
  - Click your profile icon -> `Settings` -> `Developer Settings` -> `OAuth Apps` -> `New OAuth App`.
  - Enter your app details and specifically `callback URL`.
- In your App's Github Login button the redirection link must be

```js
`https://github.com/login/oauth/authorize?client_id=${YOUR_CLIENT_ID}`;
```

![](https://sivanesh-s.github.io/images/OAuth-github.jpg)

## Implementation

Here you can obtain authToken using both callback way as well as promise way.

### Promise

```js
const express = require('express');
const app = express();

const githubAPI = require('../');

// YOUR EXPRESS APPLICATION

githubAPI(
  app, // Send your app instance to get OAuth Access
  {
    clientId: 'YOUR_CLIENT_ID',
    clientSecret: 'YOUR_CLIENT_SECRET',
    redirectURL: '/oauth-call'
  }
)
  .then(authToken => {
    console.log('authToken:', authToken);
  })
  .catch(err => console.log(err));
```

### Callback

```js
const express = require('express');
const app = express();

const githubAPI = require('../');

// YOUR EXPRESS APPLICATION

githubAPI(
  app, // Send your app instance to get OAuth Access
  {
    clientId: 'YOUR_CLIENT_ID',
    clientSecret: 'YOUR_CLIENT_SECRET',
    redirectURL: '/oauth-call'
  },
  (err, authToken) => {
    if (err) {
      console.log('err:', err);
      return;
    }

    console.log('authToken:', authToken);
  }
);
```

Once Auth Token obtained you can call [Githubs Developer APIs](https://developer.github.com/v3/) by just adding a header in each request as

```json
{
  "headers": {
    "Accept": "application/json"
  }
}
```
