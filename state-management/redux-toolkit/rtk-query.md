# Redux Toolkit Query

1. [General information](#general-info)
2. [Setup](#setup)
3. [Usage](#usage)
4. [Example](#example)

## General information

RTK Query is a powerful data fetching and caching tool. It is designed to simplify common cases for loading data in a web application, eliminating the need to hand-write data fetching & caching logic yourself.
RTK Query is an optional addon included in the Redux Toolkit package, and its functionality is built on top of the other APIs in Redux Toolkit.

Here is a [link](https://redux-toolkit.js.org/rtk-query/overview) to the ofiicial documentation.

Web applications normally need to fetch data from a server in order to display it. They also usually need to make updates to that data, send those updates to the server, and keep the cached data on the client in sync with the data on the server. This is made more complicated by the need to implement other behaviors used in today's applications:

- Tracking loading state in order to show UI spinners
- Avoiding duplicate requests for the same data
- Optimistic updates to make the UI feel faster
- Managing cache lifetimes as the user interacts with the UI

## Setup

To install RTK Query (Redux Toolkit Query), you need to follow these steps:

1. Install Redux Toolkit: RTK Query relies on Redux Toolkit, so make sure you have Redux Toolkit installed in your project. If you haven't installed Redux Toolkit yet, you can do so with the following command:

```bash
pnpm install @reduxjs/toolkit
```

2. Install RTK Query: After installing Redux Toolkit, you can install RTK Query by running the following command:

```bash
pnpm install @reduxjs/toolkit-query
```

## Usage

Here is a [link](https://redux-toolkit.js.org/tutorials/rtk-query/) of how you can configure this library on your project.

Also, you can follow these guides here:
[RTK Query Basics](https://redux.js.org/tutorials/essentials/part-7-rtk-query-basics)
In the example above pay attention to the section about the refreshing caching data topics (both manual and automatic refreshing with cache invalidation using tags). More info is [here](https://redux.js.org/tutorials/essentials/part-7-rtk-query-basics#refreshing-cached-data).

Here you can find more complex topics of using RTK Query to simplify your codebase and improve user experience (especially Invalidating Specific Items and Advanced cache updates):
[RTK Query Advanced Patterns](https://redux.js.org/tutorials/essentials/part-8-rtk-query-advanced)

## Example

This example was taken from [this](https://www.udemy.com/course/react-redux/) course.

### User photo albums app

Here is a mockup of this app:

 ![Application mockup](https://github.com/uptechteam/fe-vitejs-template/assets/13544983/83c806ed-4d59-4ccd-b3f3-4d1cd42366d1)

In the example below we build the app where we will make a network request to an outside API and fetch a list of users and show them on the screen. We're going to have a button that we can click on to add in a new user. Whenever that button is clicked, we're going to make a request to create a new user, and we're going to give that user a randomly generated name. Next to every user, we're going to have a little button to delete the user.

As we show these users, you notice there are these little kind of expander arrows, arrows over here to the right hand side. So if we expand this first section right here, we're going to display a list of photo albums that have been created by this user.
So an album is going to be a collection of different photos. For every album this user has created, we're going to show a very similar little expander panel right here. We can delete an album that has been created by clicking on that button. We can expand the album by clicking over here and we can add in brand new albums that are tied to this user by clicking on the `Add album` button.
And then finally, if the user clicks on an album to expand it, we're going to show a collection of photos that have been created inside of that album. And likewise, very similar to adding new users and new albums, we can add in new photos as well.

So all the data is essentially randomly generated, but we are going to save it to an outside API so we can fetch these randomly generated records at some point in the future.

All of our data will be stored on an outside server. For this purpose we use JSON server library. This is a very simple API server that we can use to store any kind of data. So on the server, we're going to have a list of users albums and photos. And we're going to make requests to fetch those lists and store them inside of our Redux store.

And then once they're inside of our store, of course, we're going to access that data inside of React Component and render the list of users albums or photos out.

[Demo](https://codesandbox.io/p/sandbox/confident-farrell-93xx5j) with the source code.
