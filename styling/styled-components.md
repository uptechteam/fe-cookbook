# Styled Components
 
[Installation](#installation)   
[Creating styled components](#creating-styled-components)   
[Styling props](#styling-props)    
[Styling nested components](#styling-nested-components)   
[Theming with styled-components](#theming-with-styled-components)     
[Resetting and normalizing global styles](#resetting-and-normalizing-global-styles)   
[Using styled-components with TypeScript](#using-styled-components-with-typescript)    
[Using css function and mixins](#using-css-function-and-mixins)    
[Best practices for naming styled-components](#best-practices-for-naming-styled-components)   
[Best practices and edge cases](#best-practices-and-edge-cases)

Official docs you can find [here](https://styled-components.com/docs).

> **Note:** there are a lot of hardcoded values for styles(colors, font sizes, etc.). This was made to avoid redundant code in examples. Please, use these variables from `theme` object in real project.

## Installation

To use `styled-components` in your React project, you first need to install it as a dependency.

Run `pnpm install styled-components`.

## Creating styled components

To create a styled component, you can use the styled function that comes with `styled-components`. Here's an example:

```js
// styles.js

import styled from 'styled-components';

const Button = styled.button`
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  border-radius: 5px;
  border: none;
  cursor: pointer;

  &:hover {
    background-color: #0069d9;
  }
`;
```

## Styling props

`Styled-components` allows you to pass props to your styled components and use them to conditionally style the component. Here's an example:

```js
// styles.js

import styled from 'styled-components';

const Button = styled.button`
  background-color: ${(props) => (props.primary ? '#007bff' : '#fff')};
  color: ${(props) => (props.primary ? '#fff' : '#007bff')};
  padding: 10px 20px;
  border-radius: 5px;
  border: ${(props) =>
    props.primary ? 'none' : '1px solid #007bff'};
  cursor: pointer;

  &:hover {
    background-color: ${(props) =>
      props.primary ? '#0069d9' : '#007bff'};
    color: #fff;
  }
`;
```

## Styling nested components

> **Note:** Don't use deep nesting(more than one level of nesting)!

`Styled-components` allows you to style nested components within your styled component. Here's an example:

```js
// styles.js

import styled from 'styled-components';

const Container = styled.div`
  background-color: #f2f2f2;
  padding: 20px;

  h1 {
    color: #007bff;
    font-size: 24px;
  }

  p {
    color: #444;
    font-size: 16px;
  }
`;
```

## Theming with styled-components

`Styled-components` allows you to use themes to create consistent styling across your application. A theme is simply an object that contains the values you want to use for your styles.

You can configure theme structure as you want and expand existing themes.

To use a theme in your application, you first need to create a theme object. Here are examples:

```js
// theme.js

const theme = {
  colors: {
    primary: '#007bff',
    secondary: '#6c757d',
    danger: '#dc3545',
    success: '#28a745',
  },
  fontSizes: {
    small: '14px',
    medium: '16px',
    large: '18px',
  },
};
```

```js
// theme.js

import { defaultTheme } from '@styles/defaultTheme';

const theme = {
    ...defaultTheme,
    typography: {
        fontFamily: 'Montserrat, Arial, sans-serif',
        fontSize: '16px',
        heading: {
            fontSize: '28px',
            fontWeight: '600',
        },
        // ... other typography styles
    },
    spacing: {
        unit: 10,
        padding: {
            small: '10px',
            medium: '20px',
            large: '30px',
        },
        // ... other spacing values
    },
    // ... other theme properties
};
```

To use this theme in your styled components, you can pass it to the `ThemeProvider` component from `styled-components`. Here's an example:

```js
// App.js

import React from 'react';
import styled, { ThemeProvider } from 'styled-components';

const Button = styled.button`
  background-color: ${(props) => props.theme.colors.primary};
  color: #fff;
  padding: 10px 20px;
  border-radius: 5px;
  border: none;
  cursor: pointer;

  &:hover {
    background-color: #0069d9;
  }
`;

const App = () => {
  return (
    <ThemeProvider theme={theme}>
      <Button>Click me</Button>
    </ThemeProvider>
  );
};

export default App;
```

## Resetting and normalizing global styles

`normalize.css` is a small CSS file that aims to make the default styles consistent across different browsers. It applies a set of CSS rules that reset or normalize the default styles of HTML elements, ensuring a more consistent baseline appearance for web pages.

To add normalize.css to a React project, you can follow these steps:

1. Run `pnpm install normalize.css`
2. Import `normalize.css` in your project

```js
// App.js or index.js

import "normalize.css";
```

`Styled-components` provides a way to reset global styles using the `createGlobalStyle` function. This function allows you to define CSS rules that will be applied globally to your application, similar to the way that a CSS reset stylesheet would work.

To create a global style component, you can use the `createGlobalStyle` function and define your CSS rules as you would in a regular CSS stylesheet. Here's an example:

```js
import { createGlobalStyle } from 'styled-components';

export const GlobalStyle = createGlobalStyle`
  body {
    margin: 0;
    padding: 0;
    font-family: 'Helvetica Neue', sans-serif;
  }

  * {
    box-sizing: border-box;
  }
`;
```

Once you have defined your `GlobalStyle` component, you can render it inside your app component:

```js
// App.js

import React from 'react';
import { ThemeProvider } from 'styled-components';
import { GlobalStyle } from './GlobalStyle';
import { theme } from './theme';
import "normalize.css";

const App = () => {
  return (
    <ThemeProvider theme={theme}>
      <GlobalStyle />
      <h1>Hello World!</h1>
    </ThemeProvider>
  );
};

export default App;
```

## Using styled-components with TypeScript

To use `styled-components` with TypeScript, you will need to install the `@types/styled-components` package, which provides type definitions for the library.

`pnpm install styled-components @types/styled-components`

Once you have installed the necessary packages, you can create your styled components like this:

```ts
// styles.js

import styled from 'styled-components';

interface ButtonProps {
  primary?: boolean;
}

const Button = styled.button<ButtonProps>`
  background-color: ${(props) =>
    props.primary ? props.theme.colors.primary : '#fff'};
  color: ${(props) => (props.primary ? '#fff' : props.theme.colors.primary)};
  padding: 10px 20px;
  border-radius: 5px;
  border: none;
  cursor: pointer;

  &:hover {
    background-color: ${(props) =>
      props.primary ? '#0069d9' : props.theme.colors.secondary};
  }
`;
```

With TypeScript, you can also specify the types of your theme object to ensure that the theme values are used correctly throughout your application.

```ts
// theme.js

export interface Theme {
  colors: {
    primary: string;
    secondary: string;
    danger: string;
    success: string;
  };
  fontSizes: {
    small: string;
    medium: string;
    large: string;
  };
}

export const theme: Theme = {
  colors: {
    primary: '#007bff',
    secondary: '#6c757d',
    danger: '#dc3545',
    success: '#28a745',
  },
  fontSizes: {
    small: '14px',
    medium: '16px',
    large: '18px',
  },
};
```

## Using css function and mixins

When you should use it:

1. When applying styles based on props.
2. When working with theme variables.
3. For generating complex or dynamic styles.
4. When defining global styles using createGlobalStyle.
5. For reusing styles using mixins.

Examples:

```js
import styled, { css } from 'styled-components';

// Using global styles
const GlobalStyles = createGlobalStyle`
  ${css`
    body {
      font-family: 'Roboto', sans-serif;
      background-color: ${props => props.theme.backgroundColor};
    }
    
    a {
      color: ${props => props.theme.linkColor};
      text-decoration: none;
    }
  `}
`;

// Simple component
const Button = styled.button`
  ${props => css`
    background-color: ${props.primary ? 'blue' : 'gray'};
    color: ${props.primary ? 'white' : 'black'};
  `}
`;

// Mixin
const ellipsisMixin = css`
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
`;

const Title = styled.h1`
  ${ellipsisMixin}
  font-size: 24px;
`;
```

## Best practices for naming styled-components

- Use descriptive and meaningful names for styled components.
- Avoid generic or ambiguous names.
- Follow a consistent naming convention.
- Consider the component's role or hierarchy in the name.
- Keep names concise and readable.
- Avoid namespace collisions.

## Best practices and edge cases

1. Keep styled components in separate file called `styles.js` near the component.

 <img width="171" alt="Screenshot 2023-06-19 at 17 23 06" src="https://github.com/uptechteam/fe-cookbook/assets/26439649/7c6e3922-03a6-4294-bd0e-7bdd8b85041d">

2. Don't duplicate styles, use `css` function to avoid duplication. Read more [here](#using-css-function-and-mixins).
3. If you want to share styled component across multiple components keep them in a more top-level folder where it can be imported by more than one component folder.
4. You semantic tags through styling.
5. Using dynamic props, keep the parameter name either short or destructuring it right away.

 ```js
 const Headline = styled.h1`
        color: ${(p) => p.color};
      `;

      const Text = styled.span`
        padding: ${({ padding }) => padding}px;
      `;
 ```
6. Avoid using `!important` in stylesheets. If you do need to use !important in a style sheet, make sure that it is only used sparingly and only when absolutely necessary.
