# Styled Components

[Styled Components](#styled-components)    
&nbsp;&nbsp;&nbsp;&nbsp;[Installation](#installation)   
&nbsp;&nbsp;&nbsp;&nbsp;[Creating styled components](#creating-styled-components)   
&nbsp;&nbsp;&nbsp;&nbsp;[Styling props](#styling-props)    
&nbsp;&nbsp;&nbsp;&nbsp;[Styling nested components](#styling-nested-components)   
&nbsp;&nbsp;&nbsp;&nbsp;[Theming with styled-components](#theming-with-styled-components)     
&nbsp;&nbsp;&nbsp;&nbsp;[Resetting global styles with styled-components](#resetting-global-styles-with-styled-components)   
&nbsp;&nbsp;&nbsp;&nbsp;[Using styled-components with TypeScript](#using-styled-components-with-typescript)

Official docs you can find [here](https://styled-components.com/docs).

## Installation

To use styled-components in your React project, you first need to install it as a dependency.

Run `pnpm install styled-components`.

## Creating styled components

To create a styled component, you can use the styled function that comes with styled-components. Here's an example:

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

Styled-components allows you to pass props to your styled components and use them to conditionally style the component. Here's an example:

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

Styled-components allows you to style nested components within your styled component. Here's an example:

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

Styled-components allows you to use themes to create consistent styling across your application. A theme is simply an object that contains the values you want to use for your styles.

To use a theme in your application, you first need to create a theme object. Here's an example:

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

To use this theme in your styled components, you can pass it to the ThemeProvider component from styled-components. Here's an example:

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

## Resetting global styles with styled-components

styled-components provides a way to reset global styles using the createGlobalStyle function. This function allows you to define CSS rules that will be applied globally to your application, similar to the way that a CSS reset stylesheet would work.

To create a global style component, you can use the createGlobalStyle function and define your CSS rules as you would in a regular CSS stylesheet. Here's an example:

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

Once you have defined your GlobalStyle component, you can render it inside your app component:

```js
// App.js

import React from 'react';
import { ThemeProvider } from 'styled-components';
import { GlobalStyle } from './GlobalStyle';
import { theme } from './theme';

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

To use styled-components with TypeScript, you will need to install the @types/styled-components package, which provides type definitions for the library.

`npm install styled-components @types/styled-components`

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
