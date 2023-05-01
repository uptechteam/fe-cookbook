# AWS-Cognito-auth

1. Create User Pool in Cognito
2. Configure Amplify locally
3. Create simple authorization
4. Create Protected route
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

<img width="1321" alt="Screenshot 2023-04-27 at 16 24 06" src="https://user-images.githubusercontent.com/26439649/234875662-be9d19f1-192a-43c9-8ca6-a48e5c433860.png">

11. Get ClientId (User Pool Web Client Id) on *App Integration* tab

<img width="1309" alt="Screenshot 2023-04-27 at 16 26 07" src="https://user-images.githubusercontent.com/26439649/234876199-25aade18-33de-468b-9b24-4e94dea8c7b1.png">

### Create identity provider

1. Choose identity pool name

<img width="1398" alt="Screenshot 2023-04-27 at 16 01 37" src="https://user-images.githubusercontent.com/26439649/234869795-f6249337-2aec-4b68-a046-57e616a4e125.png">

2. Enter *User Pool Id* and *App client id* from created User Pool.

<img width="1381" alt="Screenshot 2023-04-27 at 16 03 09" src="https://user-images.githubusercontent.com/26439649/234870068-a7b9ce5e-9291-43f8-aeb4-d664305c9907.png">

3. Allow creating identity roles

<img width="1649" alt="Screenshot 2023-04-27 at 16 05 42" src="https://user-images.githubusercontent.com/26439649/234870879-a144a394-e97b-4d45-8ffa-ea2e48e7239d.png">

4. Get Identity Pool id

<img width="880" alt="Screenshot 2023-04-27 at 16 07 19" src="https://user-images.githubusercontent.com/26439649/234871055-bab30045-ac4e-4929-8a96-0aad6c80f137.png">

### Add credentials to the project
Create *.env* file
```js
// .env
REACT_APP_AWS_REGION=us-east-1
REACT_APP_AWS_POOL_ID=us-east-1_F2Yb8FCtz
REACT_APP_AWS_WEB_CLIENT_ID=62si3qgd7tpsj9i4tcdsncqfp2
REACT_APP_AWS_IDENTITY=us-east-1:940ad2d5-f5db-4db3-909e-657d66f1d4a4
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

!Note: if you need to create new IAM user run `amplify configure` at first, sign in as administrator and create a user follow the instructions in command line. 

Run `amplify init`.

!Note: if you don't have Amplify profile locally, you need to find your ***accessKeyId*** and ***secretAccessKey*** in AWS Console.(How to find?)

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
