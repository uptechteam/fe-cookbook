# AWS-Cognito-auth

- [AWS-Cognito-auth](#aws-cognito-auth)
  - [Configure User Pool in AWS Console](#configure-user-pool-in-aws-console)
    - [Create User Pool in Cognito](#create-user-pool-in-cognito)
    - [Create identity provider](#create-identity-provider)
    - [Add credentials to the project](#add-credentials-to-the-project)
    - [Adding env variables to the project in GitHub (if you use GitHub Actions)](#adding-env-variables-to-the-project-in-github-if-you-use-github-actions)
  - [Configure Amplify locally](#configure-amplify-locally)
    - [Install Amplify CLI](#install-amplify-cli)
    - [Init Amplify in the project](#init-amplify-in-the-project)
    - [Add authentication to the project(automaticaly creates User Pool)](#add-authentication-to-the-projectautomaticaly-creates-user-pool)
  - [Create simple authorization](#create-simple-authorization)
    - [Add Amplify to the project](#add-amplify-to-the-project)
    - [Importing existing user pools](#importing-existing-user-pools)
    - [Add authorization with predefined components](#add-authorization-with-predefined-components)
    - [Add custom authorization](#add-custom-authorization)
  - [Add social Sign-in(Google, Facebook ...)](#add-social-sign-ingoogle-facebook-)
    - [Setup your auth provider](#setup-your-auth-provider)
    - [Configure providers in Cognito](#configure-providers-in-cognito)
    - [Configure providers in your app](#configure-providers-in-your-app)
    - [Inform your auth provider of URL](#inform-your-auth-provider-of-url)
    - [Add social sign-in to your app](#add-social-sign-in-to-your-app)
  - [Caveats](#caveats)
    - [Using social sign-in with localhost](#using-social-sign-in-with-localhost)
    - [Using with Next.js](#using-with-nextjs)
  - [Handling logic for using access and refresh tokens](#handling-logic-for-using-access-and-refresh-tokens)

There are 2 ways to add Amplify to your project. First, you can do it manually through AWS Console. Second, do it using Amplify CLI locally.
Let's look at the first way.

## Configure User Pool in AWS Console 

### Create User Pool in Cognito

1. Go to the AWS Cosole -> Cognito.

2. Click "Create user pool".
<img width="1291" alt="Screenshot 2023-04-24 at 18 54 23" src="https://user-images.githubusercontent.com/26439649/234050831-220a4be7-1af8-4c7c-bdaa-4c08de8b8d3f.png">

3. Choose sign-in options and if you want any social provider to be used.
<img width="780" alt="Screenshot 2023-04-24 at 19 08 56" src="https://user-images.githubusercontent.com/26439649/234054198-ebf278eb-4390-49a4-a8a7-a338f02664f4.png">
<img width="776" alt="Screenshot 2023-04-24 at 19 09 25" src="https://user-images.githubusercontent.com/26439649/234054317-c43a3bcb-9900-4b37-8141-6acd26be4ea1.png">
<img width="776" alt="Screenshot 2023-04-24 at 19 09 38" src="https://user-images.githubusercontent.com/26439649/234054377-78010b16-1238-4b80-9078-d44d65e601f2.png">

4. Configure passwords policy and other app security.
<img width="791" alt="Screenshot 2023-04-24 at 19 11 37" src="https://user-images.githubusercontent.com/26439649/234055168-5f8f50be-e78c-435a-8294-21efd7fd3c45.png">

5. Configure sign-up options
<img width="818" alt="Screenshot 2023-04-25 at 19 07 29" src="https://user-images.githubusercontent.com/26439649/234337183-e2b70a99-50c2-4f7d-b813-b914f0982e92.png">

Choose required attributes for sign-up.
<img width="819" alt="Screenshot 2023-04-25 at 19 15 44" src="https://user-images.githubusercontent.com/26439649/234339161-854ff09f-c3e8-4ff4-9340-147befcdf408.png">

6. Configure messages delivery.
<img width="825" alt="Screenshot 2023-04-25 at 19 18 57" src="https://user-images.githubusercontent.com/26439649/234339896-53606462-773c-49af-aba6-9f402bf5dbd9.png">

7. Configure federated identity providers if needed(Google, Facebook etc.).

8. Configure user pool.

Choose user pool name.

<img width="818" alt="Screenshot 2023-04-25 at 19 34 52" src="https://user-images.githubusercontent.com/26439649/234343768-57658a0a-f0ed-43cf-98a2-6158098b6723.png">

Choose domain name.

<img width="814" alt="Screenshot 2023-04-25 at 19 37 01" src="https://user-images.githubusercontent.com/26439649/234344304-ab2ec9ad-3898-4cf4-b6cc-dda16f2bf1b7.png">

Choose client name.

<img width="787" alt="Screenshot 2023-04-25 at 19 47 59" src="https://user-images.githubusercontent.com/26439649/234346890-8b69f199-191b-4dac-b9c9-c5b714c8c87d.png">

Add callback url.

<img width="758" alt="Screenshot 2023-04-25 at 19 50 42" src="https://user-images.githubusercontent.com/26439649/234347532-c67367fc-247b-4819-ae75-6573bea405d8.png">

Choose authentication flow.

<img width="784" alt="Screenshot 2023-04-25 at 19 48 31" src="https://user-images.githubusercontent.com/26439649/234346998-e88d8986-5433-4ae2-8075-26bfd3a74f29.png">

9. Check all entered data.

10. Get User Pool Id

<img width="1321" alt="234875662-be9d19f1-192a-43c9-8ca6-a48e5c433860" src="https://user-images.githubusercontent.com/26439649/235461511-d0554e1d-9f41-4415-9eb6-4fd47b7d8df2.png">

11. Get ClientId (User Pool Web Client Id) on *App Integration* tab

<img width="1309" alt="234876199-25aade18-33de-468b-9b24-4e94dea8c7b1" src="https://user-images.githubusercontent.com/26439649/235461672-191d3db0-951f-4456-9cc3-24aa985ce779.png">

### Create identity provider

1. Choose identity pool name

<img width="1398" alt="Screenshot 2023-04-27 at 16 01 37" src="https://user-images.githubusercontent.com/26439649/234869795-f6249337-2aec-4b68-a046-57e616a4e125.png">

2. Enter *User Pool Id* and *App client id* from created User Pool.

<img width="1381" alt="234870068-a7b9ce5e-9291-43f8-aeb4-d664305c9907" src="https://user-images.githubusercontent.com/26439649/235461925-2d361325-4219-48c3-98f7-0aa3a267278b.png">

3. Allow creating identity roles

<img width="1649" alt="Screenshot 2023-04-27 at 16 05 42" src="https://user-images.githubusercontent.com/26439649/234870879-a144a394-e97b-4d45-8ffa-ea2e48e7239d.png">

4. Get Identity Pool id

<img width="880" alt="234871055-bab30045-ac4e-4929-8a96-0aad6c80f137" src="https://user-images.githubusercontent.com/26439649/235461261-f7b3ea04-b3c2-424b-b149-71c4bffe7356.png">


### Add credentials to the project
Create *.env* file
```js
// .env
// For create-react-app:
REACT_APP_AWS_REGION=XX-XXXX-X
REACT_APP_AWS_POOL_ID=XX-XXXX-X_abcd1234
REACT_APP_AWS_WEB_CLIENT_ID=a1b2c3d4e5f6g7h8i9j0k1l2m3
REACT_APP_AWS_IDENTITY=XX-XXXX-X:XXXXXXXX-XXXX-1234-abcd-1234567890ab

// For vite.js:
VITE_AWS_REGION=XX-XXXX-X
VITE_AWS_POOL_ID=XX-XXXX-X_abcd1234
VITE_AWS_WEB_CLIENT_ID=a1b2c3d4e5f6g7h8i9j0k1l2m3
VITE_AWS_IDENTITY=XX-XXXX-X:XXXXXXXX-XXXX-1234-abcd-1234567890ab
```

Create file *aws-exports.js*
```js
// aws-exports.js

const config = {
    Auth: {
        region: process.env.REACT_APP_AWS_REGION,
        userPoolId: process.env.REACT_APP_AWS_POOL_ID,
        userPoolWebClientId: process.env.REACT_APP_AWS_WEB_CLIENT_ID,
        identityPoolId: process.env.REACT_APP_AWS_IDENTITY
    }
}

export default config;
```

### Adding env variables to the project in GitHub (if you use GitHub Actions)

  ![Existing env variables](https://github.com/uptechteam/prism-athlete-fe/assets/13544983/91eefbe8-6a47-490b-b949-0f618215172f)
  
  ![Env variables adding](https://github.com/uptechteam/prism-athlete-fe/assets/13544983/727c4a11-11fe-450f-bf75-dd0f120b10d5)

## Configure Amplify locally

### Install Amplify CLI

Run `npm install -g @aws-amplify/cli`.

### Init Amplify in the project

> Note: if you need to create new IAM user run `amplify configure` at first, sign in as administrator and create a user follow the instructions in command line. 

Run `amplify init`.

> Note: if you don't have Amplify profile locally, you need to find your ***accessKeyId*** and ***secretAccessKey*** in AWS Console.

### Add authentication to the project(automaticaly creates User Pool)

1. Run `amplify add auth`.
2. Choose preferred authentication method(you can change it afterwards by CLI or through AWS Console).
3. Run `amplify push`.
4. You have file *aws-exports.js*

## Create simple authorization

### Add Amplify to the project

1. Run `npm install aws-amplify` or `yarn add aws-amplify`.
2. Configure Amplify credentials.
```js
// App.js

import { Amplify } from "aws-amplify"; 
import awsConfig from "./aws-exports";

Amplify.configure(awsConfig);
```

### Importing existing user pools

Import existing Amazon Cognito resources into your Amplify project. Get started by running amplify import auth command to search for & import an existing Cognito User Pool & Identity Pool in your account.

```js
amplify import auth
```

The amplify import auth command will:

- automatically populate your Amplify Library configuration files (aws-exports.js, amplifyconfiguration.json) with your chosen Amazon Cognito resource information;
- provide your designated existing Cognito resource as the authentication & authorization mechanism for all auth-dependent categories (API, Storage and more);
- enable Lambda functions to access the chosen Cognito resource if you permit it
Make sure to run `amplify push` to complete the import process and deploy this backend change to the cloud.

This feature is particularly useful if you're trying to:

- enable Amplify categories (such as API, Storage, and function) for your existing user base;
- incrementally adopt Amplify for your application stack;
- independently manage Cognito resources while working with Amplify.

*!!!Important!!!*
Pay attention to the autogenerated `amplify` folder. You don't need to store it in your project or just add it to the .gitignore file as it has sensitive data.

More info you can find [here](https://docs.amplify.aws/cli/auth/import/).

### Add authorization with predefined components

1. Run `npm i @aws-amplify/ui-react` or `yarn add @aws-amplify/ui-react`.
2. Add *withAuthenticator* to your App.

```js
import './App.css';
import { withAuthenticator } from "@aws-amplify/ui-react";
import { Amplify } from "aws-amplify";
import awsConfig from "./aws-exports-cognito";

Amplify.configure(awsConfig);

function App({ user, signOut }) {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default withAuthenticator(App);
```

> Note: how to customize Authenticator component you can read [here](https://ui.docs.amplify.aws/react/connected-components/authenticator/customization).

> Note: also you're receiving `user` and `signOut` props for further actions.

### Add custom authorization

Below there is the code generated by ChatGPT to demonstrate you how to use Amplify API to create custom authorization.

```js
import React, { useState, useEffect } from 'react';
import { Auth } from 'aws-amplify';
import { Route, Redirect } from 'react-router-dom';

function CustomAuthenticator() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    Auth.currentAuthenticatedUser()
      .then((user) => {
        setUser(user);
      })
      .catch((error) => {
        setUser(null);
      });
  }, []);

  const handleSignIn = async (username, password) => {
    try {
      const user = await Auth.signIn(username, password);
      setUser(user);
    } catch (error) {
      console.log(error);
    }
  };

  const handleSignUp = async (username, password, email) => {
    try {
      await Auth.signUp({
        username,
        password,
        attributes: {
          email,
        },
      });
    } catch (error) {
      console.log(error);
    }
  };

  const handleSignOut = async () => {
    try {
      await Auth.signOut();
      setUser(null);
    } catch (error) {
      console.log(error);
    }
  };

  return (
    <div>
      {user ? (
        <Redirect to="/" />
      ) : (
        <>
          <Route path="/signin">
            <SignIn handleSignIn={handleSignIn} />
          </Route>
          <Route path="/signup">
            <SignUp handleSignUp={handleSignUp} />
          </Route>
        </>
      )}
      <Route path="/signout">
        <SignOut handleSignOut={handleSignOut} />
      </Route>
      <Route exact path="/">
        <HomePage user={user} />
      </Route>
    </div>
  );
}

function SignIn(props) {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');

  const handleUsernameChange = (event) => {
    setUsername(event.target.value);
  };

  const handlePasswordChange = (event) => {
    setPassword(event.target.value);
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    props.handleSignIn(username, password);
  };

  return (
    <div>
      <h2>Sign In</h2>
      <form onSubmit={handleSubmit}>
        <div>
          <label htmlFor="username">Username:</label>
          <input type="text" id="username" value={username} onChange={handleUsernameChange} />
        </div>
        <div>
          <label htmlFor="password">Password:</label>
          <input type="password" id="password" value={password} onChange={handlePasswordChange} />
        </div>
        <button type="submit">Sign In</button>
      </form>
      <p>
        Don't have an account? <a href="/signup">Sign Up</a>
      </p>
    </div>
  );
}

function SignUp(props) {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [email, setEmail] = useState('');

  const handleUsernameChange = (event) => {
    setUsername(event.target.value);
  };

  const handlePasswordChange = (event) => {
    setPassword(event.target.value);
  };

  const handleEmailChange = (event) => {
    setEmail(event.target.value);
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    props.handleSignUp(username, password, email);
  };

  return (
    <div>
      <h2>Sign Up</h2>
      <form onSubmit={handleSubmit}>
        <div>
          <label htmlFor="username">Username:</label>
          <input type="text" id="username" value={username} onChange={handleUsernameChange} />
        </div>
        <div>
          <label htmlFor="password">Password:</label>
          <input type="password" id="password" value={password} onChange={handlePasswordChange} />
        </div>
        <div>
          <label htmlFor="email">Email:</label>
          <input type="email" id="email" value={email} onChange={handleEmailChange} />
        </div>
        <button type="submit">Sign Up</button>
       </form>
       <p>
        Already have an account? <a href="/signin">Sign In</a>
       </p>
    </div>
  );
}

function SignOut(props) {
    const handleSignOut = (event) => {
        event.preventDefault();
        props.handleSignOut();
    };

    return (
        <div>
            <h2>Sign Out</h2>
            <form onSubmit={handleSignOut}>
                <button type="submit">Sign Out</button>
            </form>
        </div>
    );
}

function HomePage(props) {
    return (
        <div>
            <h2>Home Page</h2>
            {props.user && (
                <>
                    <p>Welcome, {props.user.username}!</p>
                    <p>
                        Click here to <a href="/signout">Sign Out</a>
                    </p>
                </>
            )}
            {!props.user && (
                <>
                    <p>You are not signed in.</p>
                    <p>
                        Click here to <a href="/signin">Sign In</a> or <a href="/signup">Sign Up</a>
                    </p>
                </>
            )}
        </div>
    );
}

export default CustomAuthenticator;
```

## Add social Sign-in(Google, Facebook ...)

### Setup your auth provider

Detailed instruction for every provider you can find [here](https://docs.amplify.aws/lib/auth/social/q/platform/js/#setup-your-auth-provider).

### Configure providers in Cognito

1. In the User Pool dashboard, click on "Federated identities" on the left sidebar.
2. Choose your identity pool or create a new one.
3. Find "Authentication providers" section (if you have identity pool click on "Edit identity pool")
4. Enter appropriate credentials.

<img width="1018" alt="Screenshot 2023-05-10 at 14 37 07" src="https://github.com/uptechteam/fe-cookbook/assets/26439649/a6958e9c-9b25-4816-a095-3cce63d59bce">

5. Go to User Pools -> Your User Pool -> App Integration and choose App Client.
6. Click on Edit in Hosted UI section.
7. Add *"Allowed callback URLs"* and *"Allowed sign-out URLs"*

> When configuring social identity providers in your AWS Cognito User Pool, you will need to provide the Redirect Sign-in URI and Redirect Sign-out URI for each social provider. These URIs should match the corresponding routes in your application.
>   
> For example, if your application's sign-in route is **/signin** and your sign-out route is **/signout**, the Redirect Sign-in URI would be **https://your-app-domain.com/signin** and the Redirect Sign-out URI would be **https://your-app-domain.com/signout**.

8. Expose your *aws-exports.js* file

```js
// aws-exports.js

const config = {
    Auth: {
        region: process.env.REACT_APP_AWS_REGION,
        userPoolId: process.env.REACT_APP_AWS_POOL_ID,
        userPoolWebClientId: process.env.REACT_APP_AWS_WEB_CLIENT_ID,
        identityPoolId: process.env.REACT_APP_AWS_IDENTITY
        oauth: {
            domain: process.env.DOMAIN
            redirectSignIn: process.env.REACT_APP_BASE_URL,
            redirectSignOut: process.env.REACT_APP_BASE_URL,
            responseType: "token",
        }
    }
}

export default config;
```

### Configure providers in your app

1. Run `amplify update auth`.
2. Select the social identity providers you want to enable and enter the necessary credentials.
3. Enter redirect sign-in URI and redirect sign-out URI.
4. Run `amplify push`.

### Inform your auth provider of URL

You can find the instruction for every provider [here](https://docs.amplify.aws/lib/auth/social/q/platform/js/#configure-auth-category:~:text=You%20need%20to%20now%20inform%20your%20auth%20provider%20of%20this%20URL%3A)

### Add social sign-in to your app

There is example of using social provider in your app

```js 
import React from 'react';
import { Auth } from 'aws-amplify';
import { withAuthenticator } from 'aws-amplify-react';

const SocialSignIn = () => {
  const handleGoogleSignIn = () => {
    Auth.federatedSignIn({ provider: 'Google' });
  };

  const handleFacebookSignIn = () => {
    Auth.federatedSignIn({ provider: 'Facebook' });
  };

  return (
    <div>
      <h2>Social Sign-In</h2>
      <button onClick={handleGoogleSignIn}>Sign In with Google</button>
      <button onClick={handleFacebookSignIn}>Sign In with Facebook</button>
    </div>
  );
};

export default withAuthenticator(SocialSignIn);
```

## Caveats

### Using social sign-in with localhost

Transform your config in *aws-exports.js* file into function

```js
// aws-exports.js

const getConfig = (isLocalhost) => ({
    Auth: {
        region: process.env.REACT_APP_AWS_REGION,
        userPoolId: process.env.REACT_APP_AWS_POOL_ID,
        userPoolWebClientId: process.env.REACT_APP_AWS_WEB_CLIENT_ID,
        identityPoolId: process.env.REACT_APP_AWS_IDENTITY
        oauth: {
            domain: process.env.DOMAIN
            redirectSignIn: isLocalhost
                ? "http://localhost:3000/signin" 
                : process.env.REACT_APP_BASE_URL,
            redirectSignOut: isLocalhost
                ? "http://localhost:3000/signout" 
                : process.env.REACT_APP_BASE_URL,
            responseType: "token",
        }
    }
})

export default config;
```

Also modify *App.js* file
```js
// App.js
...
if (typeof window !== "undefined") {
  isLocalhost = !!(window.location.hostname === "localhost");
}

Amplify.configure(getAwsConfig(isLocalhost));
...
```

### Using with Next.js

Move Amplify configuration to *_app.js*

Example of using:

```js
// _app.js

...
let isLocalhost = false;

if (typeof window !== "undefined") {
  isLocalhost = !!(window.location.hostname === "localhost");
}

Amplify.configure(getAwsConfig(isLocalhost));

const App: FC<EnhancedAppProps> = (props) => {
  const { Component, pageProps } = props;
  const getLayout = Component.getLayout || ((page) => page);
  const { pathname } = useRouter();

  return (
    <>
      <Head>
        <meta name="viewport" content="initial-scale=1, width=device-width" />
      </Head>
      <AuthStatusProvider> // returns is user authorized
          {Object.values(PUBLIC_ROUTES).includes(pathname) ? (
             <PublicRoute> // if user authorized redirects to internal page
                 {getLayout(<Component {...pageProps} />)}
             </PublicRoute>
             ) : (
             <ProtectedRoute> // if user unauthorized redirects to login page
                 {getLayout(<Component {...pageProps} />)}
             </ProtectedRoute>   
           )}
      </AuthStatusProvider>
    </>
  );
};

export default App;
```

## Handling logic for using access and refresh tokens
Here is an example of the `axiosInstance.ts` file with the default configuration for axios.

```typescript
// parseServerException.ts
import axios, { AxiosError } from 'axios';

export const parseServerException = (e: Error | AxiosError): string => {
  let errorMessage = '';

  if (!axios.isAxiosError(e)) {
    errorMessage = e.message;
  } else {
    const responseData = e.response?.data;
    errorMessage =
      !e.response || !responseData ? e.message : responseData.error?.message;
  }

  return errorMessage;
};

// displayToast.tsx

import { toast } from 'react-toastify';

export const displayToastError = (title: string) => {
  toast.error(title);
};


// axiosInstance.ts

import { Auth } from 'aws-amplify';
import axios, { AxiosRequestHeaders } from 'axios';

import { displayToastError } from './displayToast';
import { parseServerException } from './parseServerException';

export const Axios = axios.create({
  baseURL: `${import.meta.env.VITE_API_URL}`,
});

Axios.defaults.headers.common['Content-Type'] = 'application/json';

Axios.interceptors.request.use(
  (config) =>
    new Promise((resolve) => {
      Auth.currentSession()
        .then((session) => {
          const idTokenExpire = session.getIdToken().getExpiration();
          const refreshToken = session.getRefreshToken();
          const currentTimeSeconds = Math.round(+new Date() / 1000);
          const accessToken = `Bearer ${session
            .getAccessToken()
            .getJwtToken()}`;
          if (idTokenExpire < currentTimeSeconds) {
            Auth.currentAuthenticatedUser().then((res) => {
              res.refreshSession(refreshToken, (err: Error) => {
                if (err) {
                  Auth.signOut();
                } else {
                  if (!config.headers) {
                    config.headers = {} as AxiosRequestHeaders;
                  }
                  config.headers.Authorization = accessToken;
                  resolve(config);
                }
              });
            });
          } else {
            if (!config.headers) {
              config.headers = {} as AxiosRequestHeaders;
            }
            config.headers.Authorization = accessToken;
            resolve(config);
          }
        })
        .catch(() => {
          // No logged-in user: don't set auth header
          resolve(config);
        });
    })
);

Axios.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response.status === 401) {
      Auth.signOut();
    }

    const errorMessage = parseServerException(error);

    displayToastError(errorMessage);

    return Promise.reject(error);
  }
);

```
