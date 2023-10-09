# Module federation

1. [General information](#general-information)
2. [When do we need use?](#when-do-we-need-use)
3. [Get started](#get-started)

## General information

Module Federation is a feature introduced in Webpack 5 that allows you to share JavaScript modules (chunks) across multiple applications at runtime. It enables developers to create a federated architecture, where different applications can dynamically consume and use each other's code as if they were part of the same application.

The main advantages of Module Federation include:

1. **Code Sharing**: Module Federation allows you to share code between applications, reducing duplication and leading to smaller bundle sizes for each application.

2. **Micro Frontends**: With Module Federation, you can build micro frontends, where different teams can work independently on separate applications and still collaborate and share code efficiently.

3. **Decoupled Architecture**: Each application can be decoupled and developed independently, providing more flexibility and scalability in the overall architecture.

4. **Dynamic Loading**: Applications can dynamically load modules from other applications at runtime, enabling a more efficient and optimized user experience.

5. **Shared State**: Module Federation allows shared state management between applications, making it easier to share data and communicate between different parts of the application.

The basic workflow of using Module Federation involves:

1. **Creating the Host Application**: This is the main application that will consume modules from other applications. It defines the remotes (external applications) it wants to consume.

2. **Creating the Remote Applications**: These are the applications that will provide modules to the host application. Each remote application exposes certain modules for consumption by the host.

3. **Configuration**: The host application and remote applications need to be configured properly to allow sharing of modules. This involves specifying which modules to share and how to load them.

4. **Runtime Sharing**: At runtime, the host application dynamically loads and consumes modules from the remote applications.

Module Federation is a powerful feature that opens up new possibilities for building complex, modular, and scalable web applications. It allows developers to create a more efficient development workflow by breaking down monolithic applications into smaller, more manageable parts that can be independently developed and deployed.

## When do we need use?

Module Federation is particularly useful in the following cases:

1. **Micro Frontends**: When building large-scale applications with multiple teams and features, Module Federation enables you to adopt a micro frontend architecture. Different teams can develop and maintain separate applications (micro frontends) independently, and these micro frontends can then be combined to create the complete application.

2. **Decentralized Development**: If you have multiple development teams working on different parts of a single application, Module Federation allows each team to maintain their own codebase and independently deploy changes. This can lead to faster development cycles and better team autonomy.

3. **Shared Components and Libraries**: Module Federation is beneficial when you have shared components, libraries, or utility modules that are used across multiple applications. Instead of duplicating these modules in each application, you can centralize them in one location and share them using Module Federation.

4. **Dynamic Loading**: When you want to optimize loading times and improve performance by dynamically loading chunks of code only when needed, Module Federation is a powerful tool. It allows applications to fetch and load modules on-demand, reducing initial load times and keeping the main bundle size smaller.

5. **Scaling Applications**: Module Federation can help you scale your application by enabling the decomposition of a monolithic codebase into smaller, more manageable modules. This can lead to better maintainability, faster build times, and improved development velocity.

6. **Legacy Code Integration**: If you have an existing application that you want to modernize, Module Federation allows you to gradually introduce new features and technologies without rewriting the entire application. You can build new functionalities as separate micro frontends and integrate them into the existing application.

7. **Shared State Management**: Module Federation can facilitate shared state management across applications. If you have multiple applications that need to share data or communicate with each other, Module Federation provides a way to achieve that without resorting to complex communication mechanisms.

In summary, Module Federation is valuable in scenarios where you want to create a modular, decoupled, and scalable architecture, with the ability to dynamically share and load modules across different parts of your application ecosystem. It allows for efficient collaboration among development teams, improves code sharing and reuse, and promotes a more flexible and maintainable codebase.

## Get started

To get started with Module Federation, you'll need to follow these general steps:

1. **Update Webpack to Version 5**: Module Federation is a feature introduced in Webpack 5, so make sure you have Webpack 5 or a later version installed in your project.

2. **Create the Host Application**: Decide which application will be the host that consumes modules from other applications. This will be the main application that imports and uses remote modules.

3. **Create the Remote Applications**: Identify the applications that will provide modules (remote modules) to the host application. These are the applications whose code you want to share and consume dynamically.

4. **Set up Webpack Configurations**:

a. In the host application, configure the webpack.config.js file to use the Module Federation plugin. Define the remotes (external applications) from which you want to consume modules.

b. In the remote applications, also configure the webpack.config.js file to use the Module Federation plugin. Define the exposed modules that the host application can consume.

6. **Build the Applications**: Build each application using their respective webpack configurations.

7. **Host Application Integration**: In the host application, you can dynamically load remote modules using SystemJS or Webpack 5's native dynamic import() function. Use the import statement to load the remote modules as needed in your host application components.

8. **Run the Applications**: Deploy or serve your applications. Ensure that all applications are accessible and running.

9. **Verify Functionality**: Test your applications to ensure that modules are being shared and consumed correctly. Check that the host application can load and use remote modules from the remote applications.

Here's a high-level code example to illustrate how you can integrate Module Federation into your host application:

```javascript
// webpack.config.js (Host Application)
const { ModuleFederationPlugin } = require("webpack");

module.exports = {
  // ...other configurations...

  plugins: [
    new ModuleFederationPlugin({
      name: "host_app",
      remotes: {
        remote_app: "remote_app@http://localhost:3001/remoteEntry.js", // URL of the remote application's entry file
      },
    }),
  ],
};

// app.js (Host Application)
import("remote_app/Module").then((remoteModule) => {
  // Use the remote module in your host application
});
```

With this setup, the host application can consume the `Module` exported by the `remote_app`. The `remote_app` is another application (a remote) that exposes the `Module` for consumption.

The steps may vary based on your specific project setup and requirements, but these general guidelines should help you get started with Module Federation. Be sure to refer to the official Webpack documentation and relevant tutorials for more in-depth guidance.
