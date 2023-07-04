# What's New in Next.js 13?

*   [Turbopack](#turbopack)
*   [/app Directory](#/app-directory)
    *   [layout.js](#layouts)
    *   [loading.js](#loading-convention)
    *   [error.js](#layouts)
    *   [not-found.js](#layouts)
*   [Data Fetching](#data-fetching)
*   [Improvements](#improvements)
    *   [next/image](#next/image)
    *   [@next/font](#@next/font)
    *   [next/link](#next/link)
*   [Caching with Fetch](#caching-with-fetch)

## Turbopack

[Turbopack](https://turbo.build/pack) is a new general-purpose JavaScript bundler and a major feature in Next.js 13. It is intended as a Webpack replacement, and although it’s released as alpha, you can use Turbopack now as the **dev-mode** bundler from Next.js 13 forward.

**Using the Turbopack in Next.js 13**

It’s easy to create a new Next.js app via `create-next-app` and use Turbopack. You use the `--example` switch and give it the `with-turbopack` argument, as shown in step 1.

1\. Start a new turbopack-built Next App

```plaintext
npx create-next-app --example with-turbopack
```

If you look at `package.json`, you’ll see this is reflected in a small change in how the `dev` script works:

```plaintext
"dev": "next dev --turbo"
```

Try running `dev` with `--turbo` enabled versus without, as shown below. As you can see, even for the humble starter app, the start time drops from over 1000 milliseconds to around 8.

```plaintext
// with --turbo
ready - started server on 0.0.0.0:3000, url: http://localhost:3000
event - initial compilation 7.216ms

// without --turbo
ready - started server on 0.0.0.0:3000, url: http://localhost:3000
event - compiled client and server successfully in 1118 ms (198 modules)
```

## /app Directory

**Enabling the /app directory**

The `/app` directory is still a **beta** feature, so to use it you have to enable experimental features in `next.config.js`, like so:

```plaintext
// next.config.js
 
const nextConfig = {
  // ...
  experimental: { appDir: true },
};
 
// ...
```

☝️ Note that this was done for us when we scaffolded the new project with `create-next-app` .

The `/app` directory lives next to the familiar `/pages` directory and supports more advanced routing and layout capabilities. Routes that match in both `/pages` and `/app` will go to `/app`, so you can gradually supersede existing routes.

The basic routing in `/app` is similar to `/pages` in that the nested folders describe the URL path, so `/app/foo/bar/page.js` becomes `localhost:3000/foo/bar` in our `dev` setup. 

The app directory contains folders and files, where the folders are used to define the routes for the application relative to the root folder (app directory). Files on the other hand handle the UI which displays to the user contents for consumption or interaction.

```plaintext
// app folder expamle 

/app
    page.js  --- *root page navigate to  "/"
    layout.js --- *root layout*
    not-found.js 
        /dashboard  
            layout.js
            loading.js
            error.js
            page.js  --- *dashboard page navigate to  "/dashboard"
            /settings
                page.js --- *settings page navigate to  "/dashboard/settings"
        
```

### layout.js

*   **layout.js** contains the UI for elements and contents that are common to a route segment and its children. This file eliminates the redundancy of having to repeat certain components or contents on every page of a segment and its descendants.

```javascript
// app/dashboard/layout.js directory

export default function DashboardLayout({
  children, // will be a page or nested layout
}) {
  return (
    <section>
      {/* header */}
      <header>
        <nav></nav>
      </header>
      {children}
      {/* footer */}
      <footer></footer>
    </section>
  );
}
```

Layout in Next.js is a UI shared between several pages or children files contained within its route segment. 

☝️ _A parent layout wraps child layouts (or nested layouts) below it using the React_ `_children_` _prop. **Root layout** must contain_ `_<html>_` _and_ `_<body>_` _tags since all the pages for the application will be rendered inside this layout, and Next.js does not automatically create these tags._

```javascript
// app/layout.js directory

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

### loading.js

*   **loading.js** encompass the UI for the loading state when data is being fetched from the server. React 18 Suspense feature makes this possible to implement to boost your user’s experience. It wraps a route segment and its descendants in the [React 18 Suspense Boundary.](https://react.dev/reference/react/Suspense#)

```javascript
// app/dashboard/loading.js directory

export default function DashboardLoading() {
  // You can add any UI inside Loading, including a Skeleton Loader
  return <SkeletonLoader />
}
```

### error.js

*   **error.js** handles the UI content for scenarios where errors are caught may be due to a wrong request or bug in code. This file will contain the design for the content to display to users with necessary contextual information. It wraps a route segment and its descendants in the React Error Boundary feature.

```javascript
// app/dashboard/error.js directory

"use client"; // Error components must be Client components

import { useEffect } from "react";

export default function DashboardError({ error, reset }) {
  useEffect(() => {
    // Log the error to an error reporting service
    console.error(error);
  }, [error]);

  return (
    <div>
      <h2>Something went wrong!</h2>
      <button
        onClick={
          // Attempt to recover by trying to re-render the segment
          () => reset()
        }
      >
        Try again
      </button>
    </div>
  );
}
```

### not-found.js

*   **not-found.js** handles the display of a 404 error page, this file invokes the notFound function which injects a `<meta name="robots" content="noindex" />` tag and throws a `NEXT_NOT_FOUND` error and also terminates rendering the UI of the route segment it is contained.

```javascript
// app/not-found.js

export default function NotFound() {
  return (
    <>
      <h2>Not Found</h2>
      <p>Could not find requested resource</p>
    </>
  );
}
```

## Data Fetching

The API to fetch data has been simplified, and the old APIs `getServerSideProps`, `getStaticProps`, and `getInitialProps` are not supported in the new `**/app**` directory.

You should fetch data directly in the components that use it, even if you need to request the data in multiple components. Behind the scenes, React and Next.js will [cache and dedupe](https://beta.nextjs.org/docs/data-fetching/fundamentals#automatic-fetch-request-deduping) requests to avoid the same data being fetched more than once.

**Fetching data in Server Components:**

```javascript
async function getData() {
 const res = await fetch("https://api.github.com/");
 return res.json();
}

export default async function Page() {
  const name = await getData();
 
  return <p>🤩 Hello {name}!</p>;
}
```

### **Fetching data in Client Components:**

```javascript
"use client";
 
import { use } from "react";
 
async function getData() {
     const res = await fetch("https://api.github.com/");
    return res.json();
}
 
export default function Page() {
  const name = use(getData());
 
  return <p>🤩 Hello {name}!</p>;
}
```

☝️ We need `**use**` because it handles the promise returned by a function in a way compatible with components, hooks, and Suspense. The `**use**` hook is a part of the [React RFC](https://github.com/acdlite/rfcs/blob/first-class-promises/text/0000-first-class-support-for-promises.md#usepromise).

## Improvements

### next/image

In terms of code changes, the **next/image** import has been renamed to **next/legacy/image**, and the **next/future/image** import renamed to **next/image**. There's [a codemod](https://nextjs.org/docs/app/building-your-application/upgrading/codemods) to help you migrate quickly. 

If you were not using **width** and **height** on non-static images (or images without the **fill** property), you will now have to set them. The same goes with the **alt** prop, though it is not required and can be left as an empty string.

```javascript
import Image from "next/image";

export default function ImageComp() {
  return (
    <Image
      src="../public/avatar.png"
      alt="Picture of the author"
      // width={500} automatically provided
      // height={500} automatically provided
      // blurDataURL="../public/avatar-blured.png" automatically provided
      // placeholder="blur" // Optional blur-up while loading
    />
  );
}
```

### @next/font

The new `@next/font/google` lets you use Google Fonts without sending any requests from the browser. CSS and font files are downloaded at build time with the rest of the static assets.

```javascript
import { Montserrat } from "@next/font/google";
 
const montserrat = Montserrat();
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={montserrat.className}>
      <body>{children}</body>
    </html>
  )
}
```

☝️ The `@next/font/google` package only works with **Google Fonts**. It does not work with other font providers. If you use any other font, you can download it locally and config with the `@next/font/local` package.

```javascript
import localFont from '@next/font/local'
 
// Font files can be colocated inside of `app`
const myFont = localFont({
  src: './my-font.woff2',
  display: 'swap',
})
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={myFont.className}>
      <body>{children}</body>
    </html>
  )
}
```

### next/link

You no longer have to include the **\<a>** tag when using **next/link**.

```javascript
import Link from 'next/link'
 
// Next.js 12: `<a>` has to be nested otherwise, it's excluded
<Link href="/dashboard">
  <a>Dashboard</a>
</Link>
 
// Next.js 13: `<Link>` always renders `<a>`
<Link href="/dashboard">
  Dashboard
</Link>
```

## Caching with Fetch

Now in Next.js 13, you can specify how you want requests to be cached (or if you don't want them to be cached at all):

```javascript
// This request should be cached until manually invalidated.
// Similar to `getStaticProps` from Next.js 12.
// `force-cache` is the default and can be omitted for brevity.
// This is called static data in Next.js world
fetch(URL, { cache: "force-cache" });
 
// This request should be refetched on every request.
// Similar to `getServerSideProps`.
// Here we have loading of dynamic data in Next.js world.
fetch(URL, { cache: "no-store" });
 
// This request should be cached with a lifetime of 10 seconds.
// Similar to `getStaticProps` with the `revalidate` option.
fetch(URL, { next: { revalidate: 10 } });
```