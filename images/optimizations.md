# Images optimization

1. [General](#general)
2. [Lazy-Loading Libraries](#lazy-loading-liblraries)
    - [react-lazyload](#react-lazyload)
    - [react-progressive-graceful-image](#react-progressive-graceful-image)
    - [react-lazy-load-image-component](#react-lazy-load-image-component)
    - [react-lazy-images](#react-lazy-images)
    - [react-lazy-blur-image](#react-lazy-blur-image)
3. [Using different images for different viweports](#using-different-images-for-different-viweports)

## General

Image optimization is a crucial aspect of web development, especially in the context of React apps or Next.js applications, where performance and user experience are paramount. Optimizing images helps reduce loading times, improve page load speed, and enhance overall user satisfaction. Both React and Next.js provide various techniques and tools for optimizing images.

Here are some key concepts and strategies for image optimization in React apps and Next.js:

1. **Image Formats**: Choose the right image format for the type of image you're using. Common formats include JPEG, PNG, and GIF. JPEG is best for photographs, while PNG is suitable for images with transparency. Consider using modern formats like WebP, which offers better compression and quality.

2. **Image Compression**: Compress images to reduce their file size without significantly sacrificing quality. Use tools like ImageMagick, TinyPNG, or online services to compress images before integrating them into your app.

3. **Lazy Loading**: Implement lazy loading for images. Lazy loading delays the loading of images that are not initially visible on the screen, improving initial page load times. In React, you can use the `loading="lazy" `attribute in the `img` tag.

4. **Responsive Images**: Provide multiple versions of an image with different resolutions and sizes to cater to different devices and screen sizes. Use CSS media queries or React's conditional rendering to load the appropriate image version.

5. **CSS Sprites**: Combine multiple small images into a single sprite sheet to reduce the number of HTTP requests. This is particularly useful for icons and small UI elements.

6. **CDNs (Content Delivery Networks)**: Use a CDN to deliver images from servers located closer to the user's geographical location. This can significantly improve image loading speed.

7. **Next.js Image Optimization**: Next.js offers an optimized way to handle images through its built-in [< Image />](https://nextjs.org/docs/pages/api-reference/components/image "Image") component. It automatically provides features like lazy loading, responsive images, and automatic format selection (WebP or PNG/JPEG). It generates multiple image sizes during build time and serves the most appropriate image size based on the user's device.

8. **Image Optimization Libraries**: Use third-party libraries specifically designed for image optimization, such as next-optimized-images. These libraries provide additional features and configuration options for fine-tuning image optimization.

9. **Webpack Plugins**: If you're using React with a custom build setup, you can utilize webpack plugins like [image-webpack-loader ](https://www.npmjs.com/package/image-webpack-loader "image-webpack-loader ")to optimize and compress images as part of your build process.

10. **Server-Side Rendering (SSR) and Static Site Generation (SSG)**: In Next.js, by using SSR or SSG, you can optimize image loading by rendering images on the server side, which can improve initial load times and SEO.

11. **Caching**: Implement caching mechanisms for images to reduce server load and improve subsequent page loads. Browser caching and CDNs can help with this.

## Lazy-Loading Libraries

Lazy loading is one of the most popular methods to optimize images in React applications. It allows users to render content only when they need it, resulting in faster initial load time, reduced bandwidth consumption, and less data traffic.

### react-lazyload

[react-lazyload](https://www.npmjs.com/package/react-lazyload "react-lazyload") can be used to lazy load any type of component in a React application. It is one of the most popular lazy-loading libraries in the React community, supporting decorators and server-side rendering.

**Key Features:**

- Has a set of properties to enable users to customize the functionalities of the component.
- Provides utilities such as `forceCheck` to display hidden content that becomes visible without a resize or scroll event.
- Supports horizontal lazy load out of the box.
- Implements only two event listeners for all lazily loaded components, optimizing the performance further.

Usage:

```bash
pnpm install --save react-lazyload
```

```ts
import { FC } from "react";
import LazyLoad from "react-lazyload";
import LazyComponent from "./LazyComponent"; //Component to be lazyloaded.
import Placeholder from "./Placeholder"; //Placeholder component.

const App: FC = () => {
  return (
    <div className="container">
      {/* Lazy loading images is supported by default without extra configurations, set height for better experience */}
      <LazyLoad height={200}>
        <img src={"https://picsum.photos/id/235/500/500"} />
      </LazyLoad>
      {/* Once this component is loaded, LazyLoad will not care about it anymore, improving performance */}
      <LazyLoad height={200} once>
        <img src={"https://picsum.photos/id/235/500/500"} />
      </LazyLoad>
      {/*  Can also load components as below */}
      <LazyLoad height={200} once>
        <LazyComponent />
      </LazyLoad>
      {/* This component will be loaded when its top edge is 100px from viewport. Offset prop is useful to make user ignorant about lazy load  effect.*/}
      <LazyLoad height={200} offset={100}>
        <img src={"https://picsum.photos/id/235/500/500"} />
      </LazyLoad>
      {/* Can use a placeholder component as bellow  */}
      <LazyLoad placeholder={<Placeholder />}>
        <img src={"https://picsum.photos/id/235/500/500"} />
      </LazyLoad>
    </div>
  );
};

export default App;
```

---

### react-progressive-graceful-image

[react-progressive-graceful-image](https://www.npmjs.com/package/react-progressive-graceful-image "react-progressive-graceful-image") is designed to provide a seamless way to implement lazy loading and progressive image loading in React applications. It aims to improve performance and user experience by loading images progressively and gracefully.

**Key Features:**

- The library loads images in a progressive manner, which means that a lower-resolution version of the image is first loaded, allowing users to see a blurry or pixelated version of the image quickly.
- Supports lazy loading
- While the higher-resolution image is loading, a placeholder (usually a low-resolution version of the image) is displayed
- If the image fails to load for any reason, the library gracefully handles the situation by displaying an error state or placeholder.
- Supports responsive image loading

Usage:

```bash
pnpm i react-progressive-graceful-image
```

```ts
import ProgressiveImage from "react-progressive-graceful-image";

const MyComponent = () => {
  return (
    <div>
      <h1>My Component</h1>
      <ProgressiveImage
        src="high-res-image.jpg"
        placeholder="low-res-image.jpg"
        onError={(event) => {
          // Handle error, e.g., display a placeholder image
        }}
        alt="Example Image"
      />
    </div>
  );
};
```

---

### react-lazy-load-image-component

[react-lazy-load-image-component](https://www.npmjs.com/package/react-lazy-load-image-component "react-lazy-load-image-component") is an easy-to-use library for lazy loading any type of component. It supports the IntersectionObserver, and you can determine when an element leaves and enters the viewport.

**Key Features:**

- The most significant feature of this library is its HOC, trackWindowScroll, which allows components to track window scroll positions without using scroll or resize event listeners for every element.
- The lazy-loaded components will have an offset of 100 pixels by default.
- Built-in, on-visible effects such as blur, black and white, and opacity transitions help to improve the user experience.
- Server-side rendering compatible.
- Support for TypeScript declarations.
- A placeholder is provided by default with the same size as the image or component, though it can be customized.

Usage:

```bash
pnpm install --save react-lazy-load-image-component
```

```ts
import { LazyLoadImage } from "react-lazy-load-image-component";
import Placeholder from "./Placeholder"; // Placeholder component.

//Import the css file if using blur, black and white, or opacity effects.
import "react-lazy-load-image-component/src/effects/blur.css";

const App = () => {
  <div className="App">
    <div className="container">
      {/* Can use a placeholder for the component */}
      <LazyLoadImage
        alt="demonstration1"
        placeholder={<Placeholder />}
        width={100}
        height={100}
        src={"https://picsum.photos/id/235/500/500"}
      />
      {/* Using the opacity effect */}
      <LazyLoadImage
        alt="demonstration2"
        effect="opacity"
        src={"https://picsum.photos/id/235/500/500"}
      />
    </div>
  </div>;
};

export default App;
```

---

### react-lazy-images

[react-lazy-images](https://www.npmjs.com/package/react-lazy-images "react-lazy-images") is a flexible library that provides components and utilities to lazy load images in React. It gives full presentational control for the caller using render props.

**Key Features:**

- Uses IntersectionObserver with polyfill to improve performance.
- Provides fallback strategies for SEO- and JavaScript-disabled environments.
- Supports server-side rendering.
- Supports background images and works with horizontal scrolling.

Usage:

```bash
pnpm install --save react-lazy-images
```

```ts
import { LazyImage } from "react-lazy-images";

const App = () => {
  <div className="App">
    <div className="container">
      {/*  Basic implementation. It is important that you pass on the ref in placeholder, 
      otherwise the detection of the element intersecting is impossible */}
      <LazyImage
        src={"https://picsum.photos/id/235/500/500"}
        alt={"demonstration"}
        debounceDurationMs={800}
        placeholder={({ imageProps, ref }) => (
          <img
            ref={ref}
            src={"https://picsum.photos/id/235/500/500?blur=10"}
            alt={imageProps.alt}
            style={{ width: "100%" }}
          />
        )}
        actual={({ imageProps }) => (
          <img {...imageProps} style={{ width: "100%" }} />
        )}
      />
    </div>
  </div>;
};

export default App;
```

---

### react-lazy-blur-image

[react-lazy-blur-image](https://www.npmjs.com/package/react-lazy-blur-image "react-lazy-blur-image") is the ideal library to lazy load images into a low-resolution placeholder. By default, this component displays a lightweight, gray placeholder and is replaced with an actual placeholder when the component is about to reach the viewport.

**Key Features:**

- It provides a minimalistic approach to lazy load images, providing the perfect UX and performance balance.
- The component accepts only two props for customization: src and style.
- Component can use styled-components to transition an image from the placeholder.

Usage:

```bash
pnpm install react-lazy-blur-image
```

```ts
import LazyImage from "react-lazy-blur-image";

const App = () => {
  <div className="App">
    <div className="container">
      {/*Basic implementation */}
      <LazyImage
        placeholder={"https://picsum.photos/id/235/500/500?blur=10"}
        uri={"https://picsum.photos/id/235/500/500"}
        render={(src, style) => (
          <img src={src} style={style} alt="demonstration" />
        )}
      />
    </div>
  </div>;
};

export default App;
```

## Using different images for different viweports

Using different images for different viewports is a key aspect of responsive web design. The `srcset` attribute is used in HTML to provide a list of image sources along with their respective sizes and descriptors. This allows the browser to choose the most appropriate image based on the user's device and screen size.

[Here you can find documentation](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images "Here you can find documentation")

**Basic implementation:**

```ts
import { FC } from "react";

const ResponsiveImage: FC = () => {
  return (
    <div>
      <h1>Responsive Image Example</h1>
      <img
        srcSet="
          image-small.jpg 320w,
          image-medium.jpg 640w,
          image-large.jpg 1024w"
        sizes="(max-width: 320px) 280px,
               (max-width: 640px) 600px,
               1000px"
        src="image-medium.jpg"
        alt="Responsive Example Image"
      />
    </div>
  );
};

export default ResponsiveImage;
```

- `srcSet` specifies different image sources with their corresponding widths.
- `sizes` defines the image sizes based on different viewport sizes.
- `src` provides a default image source to be used if none of the conditions in sizes matches.

> NOTE: Make sure you have the appropriate image files (image-small.jpg, image-medium.jpg, image-large.jpg) in the same directory as your component or update the paths accordingly.

**Implementation with` react-progressive-graceful-image` library:**

```ts
import { FC } from "react";
import ProgressiveImage from "react-progressive-graceful-image";

const MyComponent: FC = () => {
  return (
    <div>
      <h1>Responsive Images</h1>
      <ProgressiveImage
        src="high-res-image.jpg"
        placeholder="low-res-image.jpg"
        srcSet={[
          "high-res-image-small.jpg 320w",
          "high-res-image-medium.jpg 640w",
          "high-res-image-large.jpg 1024w",
        ]}
        sizes="(max-width: 320px) 280px,
               (max-width: 640px) 600px,
               1000px"
        onError={(event) => {
          // Handle error, e.g., display a placeholder image
        }}
        alt="Responsive Example Image"
      />
    </div>
  );
};
```
