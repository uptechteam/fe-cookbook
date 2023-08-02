# Redux Toolkit

1. [General information](#general-info)
2. [Setup](#setup)
3. [Usage](#usage)
4. [Examples](#examples)

## General information

 Redux Toolkit is a popular opinionated package that simplifies the development process when using Redux in a React application. It was created by the Redux team to address common issues and boilerplate code that developers encountered when using Redux.

 Here is a [link](https://redux-toolkit.js.org/introduction/getting-started) to the ofiicial documentation.

 ![Redux toolkit](https://github.com/uptechteam/fe-vitejs-template/assets/13544983/3afabbba-3fa4-4079-8cf4-9d0524eea239)
 This screenshot was taken from [this](https://www.udemy.com/course/react-redux/) course.

## Setup

The recommended way to start new apps with React and Redux is by using [official Redux+TS template for Vite](https://github.com/reduxjs/redux-templates), or by creating a new Next.js project using [Next's  `with-redux`  template](https://github.com/vercel/next.js/tree/canary/examples/with-redux).

Both of these already have Redux Toolkit and React-Redux configured appropriately for that build tool, and come with a small example app that demonstrates how to use several of Redux Toolkit's features.

```bash
# Vite with our Redux+TS template
# (using the `degit` tool to clone and extract the template)
npx degit reduxjs/redux-templates/packages/vite-template-redux my-app

# Next.js using the `with-redux` template
npx create-next-app --example with-redux my-app
```

Redux Toolkit is available as a package on NPM for use with a module bundler or in a Node application:

```bash
pnpm install @reduxjs/toolkit react-redux
```

## Usage

Here is a [link](https://redux-toolkit.js.org/tutorials/quick-start) of how you can configure this library on your project.

## Examples

These examples were taken from [this](https://www.udemy.com/course/react-redux/) course which was previously mentioned.

1. Playlists app.

In the example below we build a very simple playlist application whenever a user clicks on add movie. We are going to randomly generate a movie name and add it into a list. Whenever a movie is added, we're going to see a red X button to the very right hand side here. If we click on that button, we delete that movie.

Here is a mockup of this app:

 ![Application mockup](https://github.com/uptechteam/fe-vitejs-template/assets/13544983/36057427-da59-41f4-a259-d1f4e7582c11)

And [demo](https://codesandbox.io/s/completed-media-project-zyz2mx?file=/src/App.js) with the source code.

2. Car cost app.

Here we have an application which allows a user to track a couple of cars they own.
The user can type in the name of the car and the value of the car and this is intended to be the dollar value or whatever currency you are using.

When we click on Submit, we're going to take the car name and the car value and add it in to the list of cars displayed down here towards the bottom. The user can also delete any cars that they add. And the total down here should automatically be updated.

We're going to make sure that the list of cars can be searchable.

We are going to allow vehicles with identical names to be added into this application, but as a nice little kind of extra thing for our users, we're just going to highlight to them if they are adding in a car with a name that already exists.

Here is a mockup of this app:

 ![Application mockup](https://github.com/uptechteam/fe-vitejs-template/assets/13544983/1154eaf9-b023-4491-8d97-d2c5811d9645)

And [demo](https://codesandbox.io/s/vigorous-cartwright-p33tkx) with the source code.
