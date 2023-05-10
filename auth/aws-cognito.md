# AWS-Cognito-auth

1. Configure User Pool in AWS Console 
    a. Create User Pool in Cognito
    b. Create identity provider
    c. Add credentials to the project
2. Configure Amplify locally
    a. Install Amplify CLI
    b. Init Amplify in the project
    c. Add authentication to the project(automaticaly creates User Pool)   
3. Create simple authorization
    a. Add Amplify to the project
    b. Add authorization with predefined components
    c. Add custom authorization
4. Add social Sign-in(Google, Facebook ...)
    a. Configure providers in Cognito
    b. Use providers in your app
5. Issues with Next.js

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
REACT_APP_AWS_REGION=XX-XXXX-X
REACT_APP_AWS_POOL_ID=XX-XXXX-X_abcd1234
REACT_APP_AWS_WEB_CLIENT_ID=a1b2c3d4e5f6g7h8i9j0k1l2m3
REACT_APP_AWS_IDENTITY=XX-XXXX-X:XXXXXXXX-XXXX-1234-abcd-1234567890ab
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

5. Choose the social identity provider you want to enable (e.g., Google, Facebook, or Amazon).
Follow the instructions provided by the chosen social identity provider to set up the integration and obtain the necessary credentials.
Enter the required information and save the configuration.

### Use providers in your app
