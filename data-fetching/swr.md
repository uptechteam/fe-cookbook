
# SWR

* [General information](#general-information)
* [Pros and cons](#pros-and-cons)
* [Getting started](#getting-started)
* [Global configuration](#global-configuration)
* [Cache](#cache)

## General information

The name “SWR” is derived from stale-while-revalidate. It is a caching strategy and a data fetching library that helps optimize network requests and improve the performance of web applications.

The official documentation you can find [here](https://swr.vercel.app/).

SWR is particularly useful when working with data that frequently changes, such as real-time data or data that is updated by multiple users. The main goal of SWR is to provide a great user experience by minimizing the time it takes for data to be updated and displayed on the user's screen.

Here's how SWR typically works:

1. Stale Data: When a request is made for data, SWR first returns the data that is already available in its cache, even if it may be slightly outdated. This allows the application to display something immediately to the user while the request is in progress.
2. Revalidation: SWR then sends a request to the server to fetch the latest data. If the data has changed since the last request, SWR updates the cache with the new data.
3. Updated Data: Once the response is received, SWR updates the cache with the fresh data and notifies any components that are subscribed to that data. This triggers a re-render, allowing the application to display the most up-to-date information to the user.

SWR also provides additional features like automatic caching, interval-based polling for data updates, and intelligent deduplication of requests to optimize performance and reduce unnecessary network traffic.

SWR is often used in conjunction with React, but it can be used with other JavaScript frameworks or libraries as well. It simplifies data fetching and caching, making it easier to build responsive and performant web applications.

 ---

## Pros and cons

Pros:

1. Improved Performance: SWR optimizes network requests by providing caching and automatic data revalidation. It reduces the number of unnecessary network requests, improves response times, and enhances overall application performance.
2. Real-Time Data: SWR is well-suited for working with real-time data or frequently changing data. It supports automatic polling, allowing you to keep the data up-to-date without manual intervention. This is especially useful for applications that require real-time updates, such as chat applications, live dashboards, or collaborative tools.
3. Simplified Data Fetching: SWR simplifies the process of fetching and managing data in web applications. It provides a simple and intuitive API, making it easier to handle data fetching, caching, and error handling. The use of the useSWR hook and the built-in caching mechanism reduces the amount of boilerplate code needed for data fetching.
4. Better User Experience: By leveraging caching and background revalidation, SWR provides a smoother user experience. It allows you to display data immediately from the cache while fetching the latest updates in the background. This reduces perceived latency and provides a more responsive interface for users.
5. Integration with React Ecosystem: SWR integrates well with React and other JavaScript frameworks, making it a convenient choice for developers working with these technologies. It provides hooks-based API, allowing seamless integration with React components and state management libraries like Redux or MobX.

Cons:

1. Learning Curve: While SWR simplifies data fetching and caching, there is still a learning curve associated with understanding its concepts and API. Developers need to spend some time familiarizing themselves with SWR and its options to make the most of its features.
2. Limited to Client-Side Caching: SWR primarily focuses on client-side caching, which means the cached data is stored in the browser's memory. This can be a limitation if you require server-side caching or need to share cached data across different users or sessions.
3. Overhead for Complex Scenarios: While SWR is great for many use cases, it may introduce some complexity for more advanced scenarios or when dealing with complex data dependencies. Customizing cache behavior or handling complex data relationships may require additional effort and careful consideration.

 ---

## Getting Started

Inside your React project directory, run the following:

```plaintext
pnpm add swr
```

For normal RESTful APIs with JSON data, first you need to create a `fetcher` function, which is just a wrapper of the native `fetch`:

```javascript
const fetcher = (...args) => fetch(...args).then(res => res.json())
```

If you want to use GraphQL API or libs like Axios, you can create your own fetcher function. Check [here](https://swr.vercel.app/docs/data-fetching) for more examples.

Here is an example of using the `fetcher` function with Axios:

```javascript
import axios from 'axios'
 
const fetcher = url => axios.get(url).then(res => res.data)
 
```

Then you can import `useSWR` and start using it inside any function components:

```javascript
import useSWR from 'swr'
 
function Profile () {
  const { data, error, isLoading } = useSWR('/api/user/123', fetcher)
 
  if (error) return <div>failed to load</div>
  if (isLoading) return <div>loading...</div>
 
  // render data
  return <div>hello {data.name}!</div>
}
```

When building a web app, you might need to reuse the data in many places of the UI. It is incredibly easy to create reusable data hooks on top of SWR:

```javascript
function useUser (id) {
  const { data, error, isLoading } = useSWR(`/api/user/${id}`, fetcher)
 
  return {
    user: data,
    isLoading,
    isError: error
  }
}
```

And use it in your components:

```javascript
function Avatar ({ id }) {
  const { user, isLoading, isError } = useUser(id)
 
  if (isLoading) return <Spinner />
  if (isError) return <Error />
  return <img src={user.avatar} />
}
```

If you call your custom hooks in multiple components on current page, it will be only **1 request** sent to the API, because they use the same SWR key and the request is **deduped**, **cached** and **shared** automatically.

 ---

## Global Configuration

```javascript
import useSWR, { SWRConfig } from 'swr'
 
function Dashboard () {
  const { data: events } = useSWR('/api/events')
  const { data: projects } = useSWR('/api/projects')
  const { data: user } = useSWR('/api/user', { refreshInterval: 0 }) // override
 
  // ...
}
 
function App () {
  return (
    <SWRConfig 
      value={{
        fetcher, // function we described above
        revalidateOnFocus: false, // automatically revalidate when window gets focused
        shouldRetryOnError: false, // retry when fetcher has an error
        provider: () => new Map(), // it's used for the work with cache
      }}
    >
      <Dashboard />
    </SWRConfig>
  )
}
```

 ---

## Cache

[Here](https://swr.vercel.app/docs/advanced/cache) is an official documentation about how to work with cache.

SWR provides built-in caching capabilities to optimize data fetching and minimize unnecessary network requests. It automatically handles caching for you, but you can also customize the cache behavior based on your specific requirements. Here's how you can work with cache in SWR:

1. Automatic Caching: By default, SWR automatically caches the response data based on the key you provide to the `useSWR` hook. When you make a request using SWR, it checks if the data for that key is already present in the cache. If it exists and is not stale, SWR returns the cached data immediately, without making a network request. This behavior helps improve performance by minimizing redundant requests.
2. Manual Caching: You can also manually update the cache using the `mutate` function provided by the `useSWR` hook. The `mutate` function allows you to update the cached data and trigger re-renders in components that are subscribed to that data. Here's an example of how to use the `mutate` function:

```javascript
const MyComponent = () => {
  const { data, mutate } = useSWR('/api/data', fetcher);

  const handleUpdateData = () => {
    // Perform data update logic
    const updatedData = ...

    // Update the cache with the new data
    mutate(updatedData);
  };

  return (
    <div>
      <button onClick={handleUpdateData}>Update Data</button>
      {data && <div>{data}</div>}
    </div>
  );
};
```

Here is another example of using cache updating when we update information about some user:

```javascript

import merge from "lodash.merge";

const updateUser = (resultData, userId) =>
  Axios.patch(`users/${userId}`, resultData);

const UserPage = () => {
  const { userId } = useParams();
  const { mutate } =  useSWRConfig();
  // Here we fetch a list of users and redefine the `revalidateIfStale` option to the `false` value. It means that the library will not automatically revalidate even if there is stale data.
  const { data } = useSWR(`users/${userId}`, {
      revalidateIfStale: false,
  });

  const handleUpdateData = () => {
    // Perform data update logic
    const updatedData = ...

    // Update the cache with the new data
     mutate(`users/${userId}`, () => updateUser(updatedData, userId), {
      populateCache: (updatedData, currentData) => {
        const updatedUser = updatedData?.data?.data;
        return merge(currentData, updatedUser || {});
      },
      revalidate: false,
    });
  };

  return (
    <div>
      <button onClick={handleUpdateData}>Update Data</button>
      {data && <div>{data}</div>}
    </div>
  );
};
```

In the example above we don't need to refetch the user data from the server. We can just update cache and use cached user data in this component.
