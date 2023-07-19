# React Query (Tanstack Query)

* [General information](#general-information)
* [Pros and cons](#pros-and-cons)
* [Getting started](#getting-started)
* [Cache](#cache)

## General information

React Query is a popular JavaScript library used for managing, caching, and synchronizing remote data in React applications. It provides a simple and declarative way to fetch, cache, and update data from various data sources, such as APIs, GraphQL endpoints, or local storage.

The official documentation you can find [here](https://tanstack.com/query/latest/docs/react/overview).

Here are some key features and concepts of React Query:

1. Query: A query represents a request for data from a remote data source. It can fetch data from an API endpoint and handle caching, background refetching, and invalidation. React Query provides hooks like useQuery to initiate and manage queries.

2. Mutation: A mutation is used for modifying data on the server. It allows you to perform operations like creating, updating, or deleting data. React Query provides hooks like useMutation to handle mutations and manage the result states.

3. Query Invalidation and Refetching: React Query automatically manages the caching of data and provides mechanisms for invalidating and refetching queries. You can manually trigger a refetch or let React Query handle it based on various options like stale time, polling intervals, or events.

4. Query States and Data Management: React Query maintains various states for a query, such as loading, error, data, and more. These states help you handle different scenarios and render UI accordingly. React Query also provides helper functions and utilities to transform, filter, or combine data.

5. Optimistic Updates: React Query supports optimistic updates, allowing you to immediately update the UI with the expected result of a mutation before the actual response from the server. This enhances the perceived performance of your application.

6. Integration with React Components: React Query is designed to seamlessly integrate with React components. It provides a QueryClientProvider component to wrap your application and a set of hooks to fetch and manage data within your components.

7. Devtools and Debugging: React Query offers a browser extension called React Query Devtools, which provides a UI for inspecting and debugging your queries, mutations, and cache. It helps you visualize and understand the state of your data fetching processes.

8. React Query is known for its simplicity, powerful features, and excellent developer experience. It simplifies complex data fetching scenarios, improves performance, and reduces boilerplate code in React applications.

 ---

## Pros and cons

Pros:

1. Simplified Data Fetching: React Query provides a simple and declarative approach to fetching and managing data. It abstracts away the complexity of handling asynchronous data fetching, caching, and synchronization, making it easier to work with remote data sources.

2. Caching and State Management: React Query includes a built-in caching mechanism that eliminates the need for manual data caching. It intelligently manages data updates, background refetching, and cache invalidation, resulting in improved performance and reduced network requests.

3. Optimistic Updates: The library supports optimistic updates, allowing you to update the UI immediately with the expected result of a mutation, providing a more responsive user experience. It handles the synchronization with the server in the background.

4. Seamless Integration with React: React Query seamlessly integrates with React components and follows the React Hooks pattern. It provides a set of hooks that can be easily used within functional components, making it intuitive and familiar to React developers.

5. Developer-Friendly Features: React Query offers various developer-friendly features, such as query retries, query deduplication, query invalidation, and result transformations. It also provides a devtools extension that aids in debugging and inspecting queries and cache states.

6. Extensibility: React Query is designed to be extensible, allowing you to customize its behavior and integrate with other libraries or data sources. You can create custom hooks, modify query configurations, or add plugins to extend its capabilities.

Cons:

1. Learning Curve: While React Query simplifies many aspects of data fetching, it still requires some initial learning and understanding of its concepts and API. It may take some time for developers who are new to the library to become proficient in using it effectively.

2. Overhead for Simple Use Cases: React Query provides a robust set of features, but for simple use cases where basic data fetching is required, it might introduce unnecessary complexity. In such scenarios, using a lighter solution like fetch or axios directly could be more straightforward.

3. GraphQL Limitations: Although React Query can work with GraphQL APIs, it doesn't provide specific optimizations for GraphQL queries. For advanced GraphQL use cases, specialized GraphQL libraries like Apollo Client might be a better fit.

## Getting Started

Inside your React project directory, run the following:

```plaintext
pnpm add @tanstack/react-query
```

This code snippet very briefly illustrates the 3 core concepts of React Query:

```typescript
import {
  useQuery,
  useMutation,
  useQueryClient,
  QueryClient,
  QueryClientProvider,
} from '@tanstack/react-query'
import { getTodos, postTodo } from '../my-api'

// Create a client
const queryClient = new QueryClient()

function App() {
  return (
    // Provide the client to your App
    <QueryClientProvider client={queryClient}>
      <Todos />
    </QueryClientProvider>
  )
}

function Todos() {
  // Access the client
  const queryClient = useQueryClient()

  // Queries
  const query = useQuery({ queryKey: ['todos'], queryFn: getTodos })

  // Mutations
  const mutation = useMutation({
    mutationFn: postTodo,
    onSuccess: () => {
      // Invalidate and refetch
      queryClient.invalidateQueries({ queryKey: ['todos'] })
    },
  })

  return (
    <div>
      <ul>
        {query.data?.map((todo) => (
          <li key={todo.id}>{todo.title}</li>
        ))}
      </ul>

      <button
        onClick={() => {
          mutation.mutate({
            id: Date.now(),
            title: 'Do Laundry',
          })
        }}
      >
        Add Todo
      </button>
    </div>
  )
}

render(<App />, document.getElementById('root'))
```

 ---

## Cache

Working with cache in React Query is straightforward and intuitive. React Query automatically manages the cache for you, but also provides flexibility to customize and interact with the cache when needed. Here's how you can work with the cache in React Query:

1. Configuring the Query Client:
Start by creating a Query Client using the `QueryClient` class from React Query. You can configure the cache options when creating the client instance:

```javascript
import { QueryClient, QueryClientProvider } from 'react-query';

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      cacheTime: 30000, // Time in milliseconds to keep data in the cache
      staleTime: 10000, // Time in milliseconds before considering data stale
    },
  },
});

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      {/* Your app components */}
    </QueryClientProvider>
  );
}

```

2. Fetching Data with Caching:
Use the `useQuery` hook to fetch data and automatically cache the results. React Query will handle caching, background refetching, and cache invalidation based on the configured options. Here's an example:

```javascript
import { useQuery } from 'react-query';

function ExampleComponent() {
  const { data, isLoading, isError } = useQuery('todos', fetchTodos);

  if (isLoading) {
    return <div>Loading...</div>;
  }

  if (isError) {
    return <div>Error fetching data</div>;
  }

  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  );
}

```

3. Manually Invalidating and Refetching:
React Query provides methods to manually invalidate and refetch data. You can use the `invalidateQueries` function to invalidate one or more queries in the cache and trigger a refetch:

```javascript
import { useMutation, queryClient } from 'react-query';

function ExampleComponent() {
  const deleteTodo = useMutation(deleteTodoAPI, {
    onSuccess: () => {
      queryClient.invalidateQueries('todos');
    },
  });

  return (
    <button onClick={() => deleteTodo.mutate(todoId)}>
      Delete Todo
    </button>
  );
}
```
