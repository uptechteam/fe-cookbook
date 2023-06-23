# LinguiJS

1.  [Integration](#integration)
2.  [Switcher](#switcher)
3.  [Mock tests](#mock-tests)

Here you can find [documentation](https://lingui.dev/)

- Advantages:
  - Modern and developer-friendly internationalization library.
  - Extensive tooling and utilities for managing translations.
  - Supports React and other frameworks.
- Disadvantages:
  - Smaller community compared to some other libraries.
  - Less widespread adoption in the React community.

## Integration

1\. Install LinguiJS and its dependencies:

```javascript
pnpm install @lingui/core @lingui/react @lingui/macro babel-plugin-macros
```

2\. Configure Babel to use the LinguiJS macros: Create a **.babelrc** or update your existing Babel configuration file to include the **babel-plugin-macros** plugin:

```javascript
{
  "plugins": ["macros"]
}
```

3\.  Set up your language catalog: Create a folder called **locales** in the root of your project. Inside the **locales** folder, create a file for each supported language, e.g., **en.js** for English and **fr.js** for French. Each file should export an object containing translation messages:

```javascript
// en.js
export default {
  'hello': 'Hello',
  'hello.user': 'Hello, {username}!',
  // Other translations...
};

// fr.js
export default {
  'hello': 'Bonjour',
  'hello.user': 'Bonjour, {username}!',
  // Other translations...
};
```

4\. Initialize the LinguiJS i18n setup: In your app's entry point file, import LinguiJS components and initialize the i18n setup with the language catalog:

```javascript
// app.js
import { I18nProvider } from "@lingui/react";
import { i18n } from "@lingui/core";
import { en, fr } from "./locales"; // import your language catalogs

// Set up LinguiJS i18n
i18n.load("en", en); // load the default language
i18n.load("fr", fr); // load additional language(s)
i18n.activate("en"); // activate the default language

// Render your app
ReactDOM.render(
  <I18nProvider i18n={i18n}>
    <App />
  </I18nProvider>,
  document.getElementById("root")
);
```

5\. Use translations in your React components: Now you can use LinguiJS macros to extract and translate text in your React components. Import the **Trans** component from **@lingui/react** and use it to wrap your translatable text:

```javascript
import { Trans } from '@lingui/react';

const MyComponent = ({ username }) => {
  return (
    <div>
      <Trans id="hello">Hello</Trans>
    </div>
    //or
     <div>
      <Trans id="hello.user">Hello, {username}!</Trans>
    </div>

  );
};
```

## Switcher

Create a component for the language switcher that allows users to switch between different languages. This component should update the active language in the LinguiJS i18n setup:

```javascript
import { i18n } from "@lingui/core";

const LanguageSwitcher = () => {
  const handleChangeLanguage = (language) => {
    i18n.activate(language);
  };

  return (
    <div>
      <button onClick={() => handleChangeLanguage("en")}>English</button>
      <button onClick={() => handleChangeLanguage("fr")}>French</button>
      {/* Add more buttons for additional languages */}
    </div>
  );
};

export default LanguageSwitcher;
```

## Mock tests

To create mock tests for your translations, you can use the **@lingui/jest** package.

1\. Install it:

```javascript
pnpm install --save-dev @lingui/jest
```

2\. Update your **jest.config.js** file to include the LinguiJS Jest presets and mocks:

```javascript
module.exports = {
  // Other Jest config options
  setupFilesAfterEnv: ["@lingui/jest/mocks"],
  preset: "@lingui/jest",
};
```

3\. Now you can write tests for your translations using the LinguiJS testing utilities. For example:

```javascript
import { i18n } from "@lingui/core";
import { render } from "@testing-library/react";

describe("MyComponent", () => {
  it("should display translated text", () => {
    i18n.activate("en"); // Set the desired language for the test

    const { getByText } = render(<MyComponent />);

    expect(getByText("Hello")).toBeInTheDocument();
  });
});
```
