# Redux Toolkit Query

1. [General information](#general-info)
2. [Setup](#setup)
3. [Usage](#usage)

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



