# Axios

* [General information](#general-information)
* [Pros and cons](#pros-and-cons)
* [Configuration](#configuration)

## General information

Axios is a promise-based HTTP Client for node.js and the browser. It is isomorphic (= it can run in the browser and nodejs with the same codebase). On the server-side it uses the native node.js http module, while on the client (browser) it uses XMLHttpRequests.

The official documentation you can find [here](https://axios-http.com/docs/intro).

 ---

## Pros and cons

Pros:

1. **Simplicity**: Axios provides a simple and straightforward API, making it easy to use and understand. It abstracts away the complexities of making HTTP requests, handling responses, and managing errors.

2. **Flexibility**: Axios can be used in various environments, including web browsers, React Native, and Node.js. It supports different platforms and provides consistent functionality across them.

3. **Promise-based**: Axios uses promises, which allows you to handle asynchronous operations in a more elegant and readable way. Promises enable chaining of operations, error handling, and better control flow.

4. **Request and response interception**: Axios allows you to intercept and modify requests and responses through the use of interceptors. This feature is helpful for adding custom headers, handling authentication, and performing other tasks before the request is sent or after the response is received.

5. **Automatic request and response transformation**: Axios can automatically transform request and response data. For example, it can parse JSON responses into JavaScript objects or convert request payloads into different formats like JSON, FormData, or URL-encoded data.

6. **Testing**: It's easy to mock, there are a lot of libraries to mock and use mocks with the tests

Cons:

1. **Size**: Axios is a relatively small library, but it does add some overhead to your project's size. If you're working on a performance-sensitive application where minimizing bundle size is crucial, you may consider using a smaller HTTP library or writing custom code instead.

2. **Additional dependency**: Axios is an external dependency that needs to be included in your project. If you're already using another HTTP library or have a minimalistic approach, adding Axios may not be necessary and can increase the complexity of your project.

3. **Limited browser support for server-side features**: Some advanced features, such as canceling requests or setting custom headers in cross-origin requests, may have limitations or require server-side support. It's important to be aware of these constraints when using Axios in a browser environment.

 ---

## Configuration

Inside your React project directory, run the following:

```plaintext
pnpm add axios
```

### Creating an instance

You can create a new instance of axios with a custom config.

```typescript
import axios from 'axios';

export const Axios = axios.create({
  baseURL: `${import.meta.env.VITE_API_URL}`,
});

Axios.defaults.headers.common['Content-Type'] = 'application/json';

Axios.interceptors.request.use(
  (config) =>
    new Promise((resolve) => {
      // handling access/refresh tokens logic
      resolve(config);
    })
);

Axios.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response.status === 401) {
      // sign out current user
    }

    // Error parsing logic

    return Promise.reject(error);
  }
);

```
