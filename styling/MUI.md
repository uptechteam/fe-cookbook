# MUI
 
[Installation](#installation)  
[Basic usage](#basic-usage)   
[Styling](#styling)   
[Theming](#theming)     
[Organizing files structure](#organizing-files-structure)   
[Example of files](#example-of-files)   
[Using MUI with TypeScript](#using-mui-with-typescript)

## Installation

Run `pnpm install @mui/material @emotion/react @emotion/styled`.

## Basic usage

Import the MUI components you want to use in your React components. For example, to use a button, you can import it like this:
```js
import { Button } from '@mui/material';

// Usage
<Button>Simple button</Button>
```

## Styling

> **Note:** Inline styles only for the synopsis, use only `styled` function in your project.

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

## Theming

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

## Organizing files structure

1. Create separate folder called *styles* in *src*
2. Files structure should be similar to the following:    
   <img width="204" alt="Screenshot 2023-06-19 at 17 35 50" src="https://github.com/uptechteam/fe-cookbook/assets/26439649/d951a5f9-5dab-4e21-a616-1ac938850230">

## Example of files

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

## Using MUI with TypeScript

* If you need to pass custom props to MUI components, you can create an interface that extends the original component's props and use it in your component. For example:

```ts
import { TextField, TextFieldProps } from '@mui/material';

interface MyTextFieldProps extends TextFieldProps {
  // Custom props
  placeholderText: string;
}

const MyTextField: React.FC<MyTextFieldProps> = ({ placeholderText, ...rest }) =>
  <TextField placeholder={placeholderText} {...rest} />;
```

* If you're using `styled` function with dynamic props, you need to define additional props. For example:

```ts
import Button from '@mui/material/Button';
import { styled } from '@mui/material/styles';

interface IButtonProps {
  isActive: boolean;
}

const ButtonStyled = styled(Button)<IButtonProps>(({ theme, isActive }) => ({
   color: isActive ? theme.palette.green : theme.palette.grey
}))
```

* If you need to expand default theme options you need to declare them in `theme.ts` file. For example:

```ts
// theme.ts

interface Palette {
   others: {
      completed: CSSProperties["color"];
      completedStroke: CSSProperties["color"];
      draft: CSSProperties["color"];
      draftStroke: CSSProperties["color"];
      processing: CSSProperties["color"];
   }
}

interface PaletteOptions {
   others: {
      completed: CSSProperties["color"];
      completedStroke: CSSProperties["color"];
      draft: CSSProperties["color"];
      draftStroke: CSSProperties["color"];
      processing: CSSProperties["color"];
   }
}

interface PaletteColorOptions {
   main: CSSProperties["color"];
   light?: CSSProperties["color"];
   dark?: CSSProperties["color"];
   contrastText?: CSSProperties["color"];
   secondary?: CSSProperties["color"];
}

export const theme = createTheme({
   // your theme config
});
```

> More detailed instruction you can find [here](#https://mui.com/material-ui/guides/typescript/).
