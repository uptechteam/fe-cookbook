# Dark Mode in React App

- [CSS Variables with Context](#css-variables-with-context)
- [Styled components](#styled-components)
- [MUI](#mui)
- [Media Queries](#media-queries)

## CSS Variables with Context

1\. Create a context or state management system to manage the dark mode state across your app. This can be done using React's built-in Context API, Redux, or any other state management library of your choice. The context/state will store a boolean value indicating whether the dark mode is enabled or disabled.

```javascript
// DarkModeContext.js
import { createContext, useState } from "react";

export const DarkModeContext = createContext();

export const DarkModeProvider = ({ children }) => {
  const [darkMode, setDarkMode] = useState(false);

  const toggleDarkMode = () => {
    setDarkMode((prevMode) => !prevMode);
  };

  return (
    <DarkModeContext.Provider value={{ darkMode, toggleDarkMode }}>
      {children}
    </DarkModeContext.Provider>
  );
};
```

2\. Wrap your app component with the **DarkModeProvider** in your main **index.js** file or at the top level of your component hierarchy.

```javascript
import ReactDOM from "react-dom";
import App from "./App";
import { DarkModeProvider } from "./DarkModeContext";

ReactDOM.render(
  <DarkModeProvider>
    <App />
  </DarkModeProvider>,
  document.getElementById("root")
);
```

3\. In any component where you want to access or update the dark mode state, import the **DarkModeContext** and use the **useContext** hook to access the context values.

```javascript
import { useContext } from "react";
import DarkModeContext from "./DarkModeContext";

function MyComponent() {
  const { darkMode, toggleDarkMode } = useContext(DarkModeContext);

  return (
    <div className="App" data-dark-mode={darkMode}>
      {/* Your component content */}
      <button onClick={toggleDarkMode}>
        {darkMode ? "Switch to Light Mode" : "Switch to Dark Mode"}
      </button>
    </div>
  );
}

export default MyComponent;
```

4\. In your app's main stylesheet or style definition file, define two sets of styles: one for light mode and one for dark mode. You can use CSS variables to make it easier to switch between the two modes.

```css
/* App.css */
.App {
  background-color: var(--bg-color);
  color: var(--text-color);
  /* Rest of your styles */
}

/* Light mode */
:root {
  --bg-color: #ffffff;
  --text-color: #000000;
  /* Rest of your light mode styles */
}

/* Dark mode */
[data-dark-mode="true"] {
  --bg-color: #000000;
  --text-color: #ffffff;
  /* Rest of your dark mode styles */
}
```

## Styled components

1\. Install styled-components: Install the styled-components library in your project by running the following command in your project directory:

```plaintext
pnpm install styled-components
```

2\. Create a ThemeProvider: In your project, create a file called **ThemeProvider.js** (or any other name you prefer) in a suitable directory. In this file, you will define your theme and wrap your app with a **ThemeProvider** component provided by styled-components.

```javascript
// ThemeProvider.js

import { ThemeProvider as StyledThemeProvider } from "styled-components";

// Define your theme object
const lightTheme = {
  // Define your light theme styles
  body: "#ffffff",
  text: "#000000",
};

const darkTheme = {
  // Define your dark theme styles
  body: "#333333",
  text: "#ffffff",
};

// Create a ThemeProvider component
const ThemeProvider = ({ children, isDarkMode }) => {
  // Use the isDarkMode prop to determine which theme to use
  const theme = isDarkMode ? darkTheme : lightTheme;

  return <StyledThemeProvider theme={theme}>{children}</StyledThemeProvider>;
};

export default ThemeProvider;
```

3\. Create a GlobalStyle component: In the same directory as **ThemeProvider.js**, create another file called **GlobalStyle.js**. This file will contain global CSS styles that will be applied to your entire app.

```javascript
// GlobalStyle.js

import { createGlobalStyle } from "styled-components";

// Define your global styles
const GlobalStyle = createGlobalStyle`
  body {
    background-color: ${(props) => props.theme.body};
    color: ${(props) => props.theme.text};
  }
`;

export default GlobalStyle;
```

4\. Implement dark mode toggle: In your main app component, create a state variable to store the current dark mode state. Then, create a toggle function to switch between light and dark mode.

```javascript
import { useState } from "react";
import ThemeProvider from "./ThemeProvider";
import GlobalStyle from "./GlobalStyle";

const App = () => {
  const [isDarkMode, setIsDarkMode] = useState(false);

  const toggleDarkMode = () => {
    setIsDarkMode((prev) => !prev);
  };

  return (
    <ThemeProvider isDarkMode={isDarkMode}>
      <div>
        <h1>Your App</h1>
        <button onClick={toggleDarkMode}>Toggle Dark Mode</button>
      </div>
      <GlobalStyle />
    </ThemeProvider>
  );
};

export default App;
```

5\. Style your components: Now you can start styling your components using styled-components and access the theme object within your styles.

```javascript
import styled from "styled-components";

const Button = styled.button`
  background-color: ${(props) => props.theme.body};
  color: ${(props) => props.theme.text};
  /* Other button styles */
`;
```

## MUI

1\. If you haven't already, install Material-UI in your React project by running the following command:

```plaintext
pnpm install @mui/material @emotion/react @emotion/styled
```

2\.  Create a Dark Mode Theme: In your **theme.js** file (or wherever you defined your theme), add the configuration for the dark mode. Modify your theme object to include the palette configuration for both light and dark modes. For example:

```javascript
import { createTheme } from "@mui/material/styles";

const lightTheme = createTheme({
  palette: {
    mode: "light",
    primary: {
      main: "#2196f3",
    },
    // Add more palette colors if needed
  },
});

// Dark mode configuration
const darkTheme = createTheme({
  palette: {
    mode: "dark",
    primary: {
      main: "#64b5f6",
    },
    // Add more palette colors if needed
  },
});

export { lightTheme, darkTheme };
```

3\. Toggle Between Themes: Update your **App** component to toggle between the light and dark themes based on the current mode. You can use the **ThemeProvider** component from MUI to dynamically switch between themes. Here's an example:

```javascript
import { useState } from "react";
import { ThemeProvider } from "@mui/material/styles";
import { lightTheme, darkTheme } from "./theme"; // Path to your theme file
import DarkModeToggle from "./DarkModeToggle";

function App() {
  const [isDarkMode, setIsDarkMode] = useState(false);

  const toggleDarkMode = () => {
    setIsDarkMode((prevMode) => !prevMode);
  };

  return (
    <ThemeProvider theme={isDarkMode ? darkTheme : lightTheme}>
      <div>
        <h1>My App</h1>
        <DarkModeToggle isDarkMode={isDarkMode} onToggle={toggleDarkMode} />
        {/* Rest of your app */}
      </div>
    </ThemeProvider>
  );
}

export default App;
```

4\. To style components specifically for dark mode, you can leverage MUI's built-in classes and styles. MUI provides a **styled** function that allows you to define component styles conditionally based on the theme. Here's an example:

```javascript
import { styled } from "@mui/material/styles";
import { Box } from "@mui/material";

export const StyledContainer = styled(Box)(({ theme }) => ({
  color: theme.palette.mode === "dark" ? "#ffffff" : "#000000",
  backgroundColor: theme.palette.mode === "dark" ? "#333333" : "#f5f5f5",
}));
```

## Media Queries

You can use a `media query` to apply different styles based on the user's color scheme preference. A user indicates their preference through an **operating system setting** (e.g. light or dark mode) or a **user agent setting**. Styles will change automatically depending on user's color scheme preference.

```css
@media (prefers-color-scheme: dark) {
  /* Styles for dark mode */
  body {
    background-color: #222;
    color: #fff;
  }
}

@media (prefers-color-scheme: light) {
  /* Styles for light mode */
  body {
    background-color: #fff;
    color: #333;
  }
}
```
