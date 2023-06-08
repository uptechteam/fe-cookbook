# Styling approaches

[General recomedations](#general-recomedations)   
[Styled Components](#styled-components)    
&nbsp;&nbsp;&nbsp;&nbsp;[Installation](#installation)   
&nbsp;&nbsp;&nbsp;&nbsp;[Creating styled components](#creating-styled-components)   
&nbsp;&nbsp;&nbsp;&nbsp;[Styling props](#styling-props)    
&nbsp;&nbsp;&nbsp;&nbsp;[Styling nested components](#styling-nested-components)   
&nbsp;&nbsp;&nbsp;&nbsp;[Theming with styled-components](#theming-with-styled-components)     
&nbsp;&nbsp;&nbsp;&nbsp;[Resetting global styles with styled-components](#resetting-global-styles-with-styled-components)   
&nbsp;&nbsp;&nbsp;&nbsp;[Using styled-components with TypeScript](#using-styled-components-with-typescript)    
[MUI](#mui)   
&nbsp;&nbsp;&nbsp;&nbsp;[Installation](#installation-1)  
&nbsp;&nbsp;&nbsp;&nbsp;[Basic usage](#basic-usage)   
&nbsp;&nbsp;&nbsp;&nbsp;[Styling](#styling)   
&nbsp;&nbsp;&nbsp;&nbsp;[Theming](#theming)     
&nbsp;&nbsp;&nbsp;&nbsp;[Organizing files structure](#organizing-files-structure)   
&nbsp;&nbsp;&nbsp;&nbsp;[Example of files](#example-of-files)

## General recomedations

1. Keep colors in separate file, like js object.
2. Keep theme in separate file, like js object.
3. Keep styles related to Component in file **styles.js** near the Component in the same folder.

## Styled Components

Official docs you can find [here](https://styled-components.com/docs).

### Installation

To use styled-components in your React project, you first need to install it as a dependency.

Run `pnpm install styled-components`.

### Creating styled components

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

### Styling props

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

### Styling nested components

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

### Theming with styled-components

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

### Resetting global styles with styled-components

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

### Using styled-components with TypeScript

To use styled-components with TypeScript, you will need to install the @types/styled-components package, which provides type definitions for the library.

`npm install styled-components @types/styled-components`

Once you have installed the necessary packages, you can create your styled components like this:

```js
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

```js
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

## MUI

### Installation

Run `pnpm install @mui/material @emotion/react @emotion/styled`.

### Basic usage

Import the MUI components you want to use in your React components. For example, to use a button, you can import it like this:
```js
import { Button } from '@mui/material';

// Usage
<Button>Simple button</Button>
```

### Styling

> There are a different ways of styling, but preferrable is using **styled** function.

* You can pass a style prop to a component to apply custom styles. For example:
```js
<Button style={{ backgroundColor: 'blue', color: 'white' }}>Styled Button</Button>
```

* MUI components come with default styles, which you can override using the sx prop. The sx prop accepts an object containing style properties. For example:
```js
<Button sx={{ backgroundColor: 'blue', color: 'white' }}>Styled Button</Button>
```

* MUI provides the styled function from the @emotion/styled package, allowing you to create custom styles for MUI components. You can define styles and create a custom component using the styled function. For example:
```js
import { styled } from '@mui/system';

const StyledButton = styled(Button)({
  backgroundColor: 'blue',
  color: 'white',
});

// Usage
<StyledButton>Styled Button</StyledButton>
```

### Theming

You can create a theme using the createTheme function from @mui/material/styles. For example:
```js
import { createTheme, ThemeProvider } from '@mui/material/styles';

const theme = createTheme({
  palette: {
    primary: {
      main: '#ff0000',
    },
  },
});

// Usage
<ThemeProvider theme={theme}>
  {/* Your application */}
</ThemeProvider>
```

> Information about all available components you can find [here](https://mui.com/)

### Organizing files structure

1. Create separate folder called *styles* in *src*
2. Files structure should be similar to the following:    
  ...      
  styles   
    - breakpoints.js
    - components.js
    - mixins.js
    - palette.js
    - shape.js
    - typograghy.js
    - theme.js
    
 ### Example of files
 
 ```js
 // breakpoints.js
 
 // use if you need to replace default breakpoints:
 // xs, extra-small: 0px
 // sm, small: 600px
 // md, medium: 900px
 // lg, large: 1200px
 // xl, extra-large: 1536px
 
 export const breakpoints = {
  values: {
    xs: 0,
    sm: 576,
    md: 992,
    lg: 1200,
    xl: 1400,
  },
 };

 export const getBreakpoint = (value) =>
  `@media (min-width:${breakpoints.values[value]}px)`;
 
 ```
 
 ```js
 // components.js
 
 // use to set up base appearance of components
 
 export const components = {
  MuiCssBaseline: {
    styleOverrides: {
    ...
    
  MuiButtonBase: {
    styleOverrides: {
      root: {
        "&.MuiButton-root": {
          textTransform: "none",
          ...
          
 ```
 
 ```js
 // mixins.js
 
 // use if you need to extend default mixins
 
 export const mixins = {
  toolbar: {
    minHeight: 50,
    "@media (min-width: 0px) and (orientation: landscape)": {
      minHeight: 50,
    },
    "@media (min-width: 600px)": {
      minHeight: 50,
    },
  },
 };
 ```
 
 ```js
 // palette.js
 
 // use to define colors set
 
 export const palette = {
  common: {
    white: "#fff",
    black: "#000",
  },
  primary: {
    main: "#41479B",
    dark: "#333",
  },
  secondary: {
    main: "#44756B",
  },
  ...
  
 ```
 > If you want to use custom colors, [here](https://mui.com/material-ui/customization/palette/#adding-new-colors) is a detailed instruction.
 
 ```js
 // shape.js
 
 // use if you want to define borderRadius
 
 export const shape = {
  borderRadius: 8,
};
 ```
 
 ```js
 // typography.js
 
 // use to define typography for different variants
 
 const h1 = {
  fontWeight: 600,
  fontSize: "28px",
  lineHeight: "35px",
};

const h2 = {
  fontWeight: 600,
  fontSize: "24px",
  lineHeight: "34px",
};

...

export const fontSizes = {
  h1,
  h2,
  ...
};

export const typography = {
  ...fontSizes,
  // general properties applied to all variants
  fontFamily: "Arial, sans-serif",
  fontWeightRegular: 400,
  fontWeightMedium: 600,
  fontWeightBold: 700,
  fontSize: 13,
};
 ```
 
 ```js
 // theme.js
 
 // use to create theme and unite all features defined above
 
  import { createTheme } from "@mui/material/styles";
  import { palette } from "./palette";
  import { shape } from "./shape";
  import { breakpoints } from "./breakpoints";
  import { typography } from "./typography";
  import { components } from "./components";
  import { mixins } from "./mixins";

  const theme = createTheme({
    palette,
    spacing: 4, // uses for defining coeficient of spacing
    shape,
    breakpoints,
    typography,
    components,
    mixins,
  });

  export default theme;
 ```
    

