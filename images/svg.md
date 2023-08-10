# Work with SVG icons

1.  [General](#general)
2.  [Using Vite Plugin for SVGR](#using-vite-plugin-for-svgr)
3.  [Using Webpack Plugin for SVGR](#using-webpack-plugin-for-svgr)
4.  [Using SvgIcon Component from MUI](#using-svgicon-component-from-mui)
5.  [Using SVG as a component](#using-svg-as-a-component)
6.  [Using the Image Tag](#using-the-image-tag)

## General

Using SVG files in your React app is quite straightforward. SVG (Scalable Vector Graphics) is a widely supported image format that's especially useful for web applications due to its ability to scale without losing quality. Here's how you can use SVG files in your React app:

## Using the Vite Plugin for SVGR

[vite-plugin-svgr](https://www.npmjs.com/package/vite-plugin-svgr "vite-plugin-svgr") is a plugin for **Vite** that uses svgr under the hood to transform SVGs into React components.

You can install it by running the following command:

```bash
# with pnpm
pnpm i vite-plugin-svgr

# with yarn
yarn add vite-plugin-svgr
```

Next, add the plugin inside your app's `vite.config.js`:

```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import svgr from "vite-plugin-svgr";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [svgr(), react()],
});
```

Now, you can import the SVG files as React components:

```javascript
import { ReactComponent as Logo } from "./logo.svg";
```

## Using Webpack Plugin for SVGR

[SVGR](https://react-svgr.com/ "SVGR") is a tool that takes raw SVG files and transforms them into React components. It also has a large ecosystem with support for Create React App, Gatsby, Parcel, Rollup, and other technologies.

Install the package by running the code:

```bash
# with pnpm
pnpm install --save-dev @svgr/webpack

# with yarn
yarn add --dev @svgr/webpack
```

Update your `webpack.config.js`:

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.svg$/i,
        issuer: /\.[jt]sx?$/,
        use: ["@svgr/webpack"],
      },
    ],
  },
};
```

Now, you can import an SVG file as a React component:

```javascript
import Logo from "./logo.svg";

const App = () => {
  return (
    <div className="App">
      <Logo />
    </div>
  );
};

export default App;
```

## Using SvgIcon Component from MUI

The `SvgIcon` is used to create some custom icons and then display it.

Here is an example how to use `SvgIcon` in your app if you are using [MUI](https://mui.com/material-ui/getting-started/ "MUI"):

1. Create `BaseIcon` component to provide a standardized way to render SVG icons with consistent width, height, and styling using Material-UI's `SvgIcon`.

```ts
import { FC } from "react";

import { SvgIcon, SvgIconProps } from "@mui/material";

export const BaseIcon: FC<SvgIconProps> = ({
  width,
  height,
  children,
  viewBox,
  ...props
}) => (
  <SvgIcon
    width={width}
    height={height}
    viewBox={viewBox || `0 0 ${width} ${height}`}
    {...props}
    sx={{
      ...props.sx,
      width: `${width}px`,
      height: `${height}px`,
    }}
  >
    {children}
  </SvgIcon>
);
```

Open up the SVG file in a text editor, and copy-paste the code into a `BaseIcon` component:

```ts
import { FC } from "react";

import { SvgIconProps } from "@mui/material";

import { BaseIcon } from "../../BaseIcon";

export const CheckedIcon: FC<SvgIconProps> = ({ ...props }) => (
  <BaseIcon width={20} height={20} {...props}>
    <rect width="20" height="20" rx="4" fill="#FFFF00" />
    <path
      d="M7.93869 15L4 11.4035L5.79493 9.76448L7.93869 11.7278L14.2051 6L16 7.639L7.93869 15Z"
      fill="black"
    />
  </BaseIcon>
);
```

Then you can use your Icon in React Components:

```javascript
import { CheckedIcon } from "./CheckedIcon";
```

## Using SVG as a component

We can convert an SVG to a React component by returning it from inside a class or functional component.

Open up the SVG file in a text editor, and copy-paste the code into a new component:

```javascript
export const ArrowUndo = () => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      className="ionicon"
      viewBox="0 0 512 512"
    >
      <path d="M245.09 327.74v-37.32c57.07 0 84.51 13.47 108.58 38.68 5.4 5.65 15 1.32 14.29-6.43-5.45-61.45-34.14-117.09-122.87-117.09v-37.32a8.32 8.32 0 00-14.05-6L146.58 242a8.2 8.2 0 000 11.94L231 333.71a8.32 8.32 0 0014.09-5.97z" />
      <path
        d="M256 64C150 64 64 150 64 256s86 192 192 192 192-86 192-192S362 64 256 64z"
        fill="none"
        stroke="currentColor"
        strokeMiterlimit={10}
        strokeWidth={32}
      />
    </svg>
  );
};
```

Now, you can import and render the SVG component in another component like this:

```javascript
import { ArrowUndo } from "./path/ArrowUndo.jsx";

export const Button = () => {
  return (
    <button>
      <ArrowUndo />
    </button>
  );
};
```

> **Note: that this approach only works with Create React App. CRA uses SVGR under the hood to make it possible to transform and import an SVG as a React component. If you’re not using Create React App, I would recommend using other approaches.**

## Using the Image Tag

If you initialize your app using **CRA** (Create React App), you can import the SVG file in the image source attribute, as it supports it off the bat.

```javascript
import YourSvg from "/path/to/image.svg";

const App = () => {
  return (
    <div className="App">
      <img src={YourSvg} alt="Your SVG" />
    </div>
  );
};
export default App;
```

> **NOTE: But if you are not using CRA, you have to set up a file loader system in the bundler you're using.**

Webpack, for instance, has a loader for handling SVGs called [file-loader](https://v4.webpack.js.org/loaders/file-loader/ "file-loader").

> **NOTE: While this approach is straightforward, it does have one disadvantage: unlike the other methods for importing, you cannot style the SVG imported in a img element. As a result, it will be suitable for an SVG that does not need customization, like logos.**
