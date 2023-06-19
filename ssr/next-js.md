# Next JS

*   [Installation](#installation)
    *   [Automatic Installation](#automatic-installation)
    *   [Manual Installation](#manual-installation)
*   [Routing](#routing)
    *   [Dynamic routing](#dynamic-routing)
*   [Components](#components)
    *   [Client Components](#client-components)
    *   [Server Components](#server-components)
*   [Optimizations](#optimizations)
    *   [Images](#images)
    *   [Dynamic import](#dynamic-import)

## Installation

There are two ways to set up the Next.js project.

### Automatic Installation:

Starting from **Next.js version 12,** Next.js provides an automatic installation feature that allows you to set up a new project with just one command. Here's how you can use the automatic installation feature:

1\. Install the **create-next-app** package globally: Run the following command to install the **create-next-app** package globally on your system:

```plaintext
npm install -g create-next-app
```

2\. Create a new Next.js project: Navigate to the directory where you want to create your new Next.js project, and run the following command:

```plaintext
npx create-next-app
```

3\. Select a template (optional): After running the above command, you'll be prompted to choose a template for your project. You can select one of the available templates or press Enter to skip this step and use the default template.

```plaintext
What is your project named? my-app
Would you like to use TypeScript with this project? No / Yes
Would you like to use ESLint with this project? No / Yes
Would you like to use Tailwind CSS with this project? No / Yes
Would you like to use `src/` directory with this project? No / Yes
Use App Router (recommended)? No / Yes
Would you like to customize the default import alias? No / Yes
```

4\. Navigate into the project directory: Once the project is created, navigate into the project directory by running the following command:

```plaintext
cd my-app
```

5\. Start the development server: Run the following command to start the Next.js development server:

```plaintext
npm run dev 
```

### Manual Installation:

1\. Create a new project folder: Create a new directory for your Next.js project. You can do this through your operating system's command line or terminal.  
2\. Initialize a new project: Navigate to the project folder using the command line or terminal and run the following command to initialize a new Node.js project:

```plaintext
npm init -y
```

This command creates a new **package.json** file for your project with default settings.

3\. Install Next.js. Run the following command to install Next.js and React:

```plaintext
npm install next react react-dom
```

4\. Open `package.json` and add the following `scripts`:

```plaintext
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  }
}
```

5\. Create Next.js pages: In Next.js, pages are created as individual files inside a folder called **pages**. Create a new **pages** folder in your project directory and create your first page. For example, you can create a file named **index.js** inside the **pages** folder with the following content:

```plaintext
function HomePage() {
  return <div>Welcome to Next.js!</div>;
}

export default HomePage;
```

6\. Start the development server: Run the following command to start the Next.js development server:

```plaintext
npm run dev
```

## Routing

In Next.js, routing is handled through the built-in file-based routing system. Here's how you can configure routing in Next.js:

1\. Create pages:

*   Inside the **pages** folder in your Next.js project, create individual files for each page you want to create. For example, you can have files like **index.js**, **about.js**, and **contact.js**.
*   Each of these files represents a specific route in your application.

2\. Define the content for each page:

*   Inside each page file, export a React component as the default export.
*   This component will define the content and structure of the respective page.

Example page component (**pages/index.js**):

```plaintext
function HomePage() {
  return <div>Welcome to the homepage!</div>;
}

export default HomePage;
```

3\. Linking between pages:

*   Next.js provides the **Link** component from the **next/link** module, which allows you to create links between pages in your application.
*   Import the **Link** component and use it to wrap the anchor (**\<a>**) tag.
*   Set the **href** prop of the **Link** component to the destination page path.

Example usage of **Link** component:

```plaintext
import Link from 'next/link';

function HomePage() {
  return (
    <div>
      <h1>Welcome to the homepage!</h1>
      <Link href="/about"><a>About</a></Link>
      <Link href="/contact"><a>Contact</a></Link>
    </div>
  );
}

export default HomePage;
```

### Dynamic routing:

*   Next.js supports dynamic routing where parts of the URL can be dynamic parameters.
*   To define a dynamic route, create a page file with square brackets (**\[\]**) in the filename. For example, **\[id\].js**.
*   Inside the page component, you can access the dynamic parameter using **useRouter** hook or **getServerSideProps**/**getStaticProps** functions.

Example dynamic routing (**pages/post/\[id\].js**):

```plaintext
import { useRouter } from 'next/router';

function PostPage() {
  const router = useRouter();
  const { id } = router.query;

  return <div>Post ID: {id}</div>;
}

export default PostPage;
```

These are the basic steps to configure routing in Next.js using the file-based routing system. Next.js takes care of mapping the URLs to the respective page components based on the file structure within the **pages** folder.

For more advanced routing scenarios, Next.js provides additional features like nested routes, catch-all routes, and custom routing configurations. You can refer to the Next.js documentation ([https://nextjs.org/docs/routing/introduction](https://nextjs.org/docs/routing/introduction)) for more information and examples on routing in Next.js.

## Components

Server and Client Components allow developers to build applications that span the server and client, combining the rich interactivity of client-side apps with the improved performance of traditional server rendering.

### Client Components:

*   Client components in Next.js are traditional React components that run on the client-side.
*   They are responsible for rendering UI elements and handling user interactions in the browser.
*   Client components are bundled and sent to the client as part of the initial page load.
*   They can utilize client-side rendering (CSR) or static generation (SSG) for data fetching
*   Examples of client components include form components, interactive widgets, and UI components that rely on user events and browser capabilities.

```javascript
// pages/client-component.js

import { useState } from 'react';

export default function ClientComponent() {
  const [count, setCount] = useState(0);

  const incrementCount = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <h1>Client Component</h1>
      <p>Count: {count}</p>
      <button onClick={incrementCount}>Increment</button>
    </div>
  );
}
```

### Server Components:

*   They allow you to perform server-side operations during rendering, such as data fetching, accessing databases, and performing server-side logic.
*   Server components execute on the server at request time, providing improved performance and flexibility.
*   Server components are written using a specialized syntax provided by Next.js.
*   They have access to a subset of React features and provide server-side rendering (SSR) capabilities.
*   Server components can be used to handle parts of your application that require server-side functionality, while other parts can be handled by client components.
*   They allow you to offload work from the client and execute it on the server, resulting in faster rendering and reduced client-side processing.

```javascript
// pages/server-component.js

export default function ServerComponent({ data }) {
  return (
    <>
      <h1>Server Component</h1>
      <ul>
        {data.map((item) => (
          <li key={item.id}>{item.title}</li>
        ))}
      </ul>
    </>
  );
}

export async function getServerSideProps() {
  // Fetch data and return it as props for the server component
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();

  return {
    props: {
      data,
    },
  };
}
```

### When to use Server and Client Components?

**👉 Use Server Components:**

*   Server-side Operations: If your component requires server-side operations such as data fetching, accessing databases, or performing server-side logic, server components are a good choice. They allow you to perform these operations on the server at the request time, reducing the load on the client and improving performance.
*   Enhanced Security: If you have sensitive logic or data that should not be exposed to the client-side, server components provide a secure way to execute code on the server. This can be useful when dealing with authentication, authorization, or other server-specific operations.
*   Dynamic Content Generation: If you need to generate dynamic content on the server based on user requests or custom parameters, server components offer the ability to execute the necessary server-side logic and return the generated content to the client.

**👉 Use Client Components**:

*   Interactive UI: If your component relies heavily on client-side interactivity, such as handling user events, DOM manipulation, or real-time updates, client components are a suitable choice. They run on the client-side and can leverage the full range of client-side capabilities and libraries.
*   Static Content: If your component displays static content that doesn't require server-side operations or frequent updates, using client components with static generation (SSG) or client-side rendering (CSR) can be more efficient. This allows the content to be generated and served from a CDN, reducing the load on the server.
*   Code Reusability: If you have components that can be reused across multiple pages and don't require server-side operations, using client components allows you to maintain a consistent user experience and improve code reusability.

## Optimizations

Optimizations in Next.js refer to various techniques and features that are designed to improve the performance and user experience of your Next.js applications. These optimizations help reduce the page load times, enhance rendering efficiency, and optimize resource usage. 

### Images

👉 Next.js provides built-in image optimization capabilities through the **next/image** package, which simplifies the process of optimizing and rendering images.

*   Basic Image Component:

```javascript
import Image from 'next/image';

export default function MyImage() {
  return (
    <div>
      <h1>Image Optimization Example</h1>
      <Image
        src="/path/to/image.jpg"
        alt="Description of the image"
        width={500}
        height={300}
      />
    </div>
  );
}
```

In the above example, the **next/image** package is imported, and the **Image** component is used to render an optimized image. The **src** prop specifies the path to the image, while the **width** and **height** props define the desired dimensions of the image. Next.js will automatically optimize the image during the build process.

---

*   Responsive Images with Multiple Sources:

```javascript
import Image from 'next/image';

export default function ResponsiveImage() {
  return (
    <div>
      <h1>Responsive Image Example</h1>
      <Image
        src="/path/to/image.jpg"
        alt="Description of the image"
        layout="responsive"
        width={1200}
        height={800}
        sizes="(max-width: 600px) 100vw, (max-width: 1200px) 50vw, 33vw"
      />
    </div>
  );
} 
```

In this example, the layout prop is set to "responsive" to enable responsive image loading. The sizes prop specifies the sizes of the image at different viewport widths. Next.js will automatically generate and deliver the appropriate version of the image based on the screen size.

---

*   Blurred Placeholder Image:

```javascript
import Image from 'next/image';

export default function PlaceholderImage() {
  return (
    <div>
      <h1>Placeholder Image Example</h1>
      <Image
        src="/path/to/image.jpg"
        alt="Description of the image"
        width={800}
        height={500}
        placeholder="blur"
        blurDataURL="/path/to/blur-image.jpg"
      />
    </div>
  );
}
```

In this example, the **placeholder** prop is set to "blur" to generate a blurred placeholder image. The **blurDataURL** prop specifies the path to the low-resolution blurred version of the image. Next.js will display the blurred placeholder image during the initial load, and replace it with the high-resolution image when it finishes loading.

---

### Dynamic import

The **next/dynamic** module in Next.js allows you to dynamically import components or modules in your application. It is particularly useful when you have components or modules that are not required during the initial page load or are conditionally rendered based on user interactions or other runtime conditions. **next/dynamic** enables code splitting and lazy loading of these components, resulting in improved performance and reduced initial bundle size.

```javascript
import dynamic from 'next/dynamic';

const DynamicComponent = dynamic(() => import('../components/DynamicComponent'));

export default function MyPage() {
  return (
    <div>
      <h1>Dynamic Component Example</h1>
      <DynamicComponent />
    </div>
  );
}
```

You can also provide additional options to the **dynamic** function to customize its behavior.

```javascript
const DynamicComponent = dynamic(() => import('../components/DynamicComponent'), {
  loading: () => <div>Loading...</div>,
  ssr: false,
});
```

*   The **loading** option is set to a function that returns a loading placeholder component to be rendered while the dynamic component is being loaded.
*   The **ssr** option is set to **false**, which disables server-side rendering (SSR) for this dynamic component. This can be useful when the component relies on browser-specific APIs or

When using **next/dynamic** with named exports, you need to slightly modify the syntax to ensure the correct import. Here's an example:

```javascript
const DynamicComponent = dynamic(() =>
  import('../components/DynamicComponent').then((module) => module.MyComponent)
);
```