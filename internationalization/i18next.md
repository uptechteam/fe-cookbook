## i18next

1.  [Integration](#integration)
2.  [Switcher](#switcher)
3.  [Mock tests](#mock-tests)

*   Advantages:  
    \- Standalone library with broad platform and framework support.  
    \- Rich feature set including language detection, interpolation, and pluralization.  
    \- Large community and good ecosystem support.
*   Disadvantages:  
    \- Requires additional configuration and setup.  
    \- Can have a steeper learning curve compared to more straightforward solutions.

## Integration

In order to add i18next in a React app, you'll need to follow these steps:

1\. Install the required packages:

```plaintext
pnpm install i18next react-i18next i18next-http-backend
```

2\. Create a translation file structure: Create a folder called **locales** in the root directory of your project. Inside the **locales** folder, create a file for each language you want to support, following the format **language.json** (e.g., **en.json**, **fr.json**).

Each language file should contain key-value pairs for the translations. Here's an example of **en.json**:

```javascript
{
  "greeting": "Hello, world!",
  "buttonText": "Click me"
}
```

3\. Initialize i18next in your app: Create a file called **i18n.js** in the root directory of your project and add the following code:

```javascript
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import Backend from 'i18next-http-backend';

i18n
  .use(Backend)
  .use(initReactI18next)
  .init({
    lng: 'en',
    fallbackLng: 'en',
    debug: true,
    interpolation: {
      escapeValue: false // React already escapes variables
    }
  });

export default i18n;
```

4\. Wrap your app with the **I18nextProvider**: Open your **index.js** file (or the entry point of your app) and add the following code:

```javascript
import ReactDOM from 'react-dom';
import { I18nextProvider } from 'react-i18next';
import i18n from './i18n'; // import the i18n instance you created

import App from './App';

ReactDOM.render(
  <I18nextProvider i18n={i18n}>
    <App />
  </I18nextProvider>,
  document.getElementById('root')
);
```

5\. Use translations in your components: Import the **useTranslation** hook from **react-i18next** in any component where you want to use translations. Here's an example:

```javascript
import { useTranslation } from 'react-i18next';

function MyComponent() {
  const { t } = useTranslation();

  return (
    <div>
      <h1>{t('greeting')}</h1>
      <button>{t('buttonText')}</button>
    </div>
  );
}

export default MyComponent;
```

6\. Load translations dynamically (optional):  
By default, i18next loads translations synchronously. However, if you have a large number of translations, it's better to load them asynchronously. To do this, you can modify your i18n.js file like this:

```javascript
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import Backend from 'i18next-http-backend';

i18n
  .use(Backend)
  .use(initReactI18next)
  .init({
    lng: 'en',
    fallbackLng: 'en',
    debug: true,
    interpolation: {
      escapeValue: false // React already escapes variables
    },
    backend: {
      loadPath: '/locales/{{lng}}.json' // specify the path to your language files
    }
  });

export default i18n;
```

This configuration will make i18next load the translation files asynchronously from the specified path (/locales/{{lng}}.json). 

☝️ Make sure to adjust the path based on your project's structure.

## Switcher

To switch the language in your React app using i18next, you can follow these steps:

1\. Implement the language switch functionality:  
When a user selects a language, you can call the i18n.changeLanguage function to update the language in i18next. Here's an example using a dropdown:

```javascript
import { useTranslation } from 'react-i18next';

const LanguageSwitcher = () => {
  const { i18n } = useTranslation();

  const handleChangeLanguage = (event) => {
    const selectedLanguage = event.target.value;
    i18n.changeLanguage(selectedLanguage);
  };

  return (
    <select onChange={handleChangeLanguage}>
      <option value="en">English</option>
      <option value="fr">French</option>
      {/* Add more language options as needed */}
    </select>
  );
}
```

2\. Update translations dynamically:  
By default, i18next will update the translations in the components automatically when the language is changed. However, if you have components that don't automatically update, you can use the **useEffect** hook to manually update them. Here's an example:

```javascript
import { useEffect } from 'react';
import { useTranslation } from 'react-i18next';

function MyComponent() {
  const { t, i18n } = useTranslation();

  useEffect(() => {
    // Manually trigger re-render when language changes
    // Update any component-specific translations here
  }, [i18n.language]);

  return (
    // Component content
  );
}

export default MyComponent;
```

## Mock tests

To test the component above, you can use mocking to provide a mock implementation for **react-i18next** and **useTranslation**. Here's an example using Jest and the **react-i18next** package's **I18nextProvider**:

```javascript
import { render } from '@testing-library/react';
import { I18nextProvider } from 'react-i18next';
import MyComponent from './MyComponent';
import i18n from './i18n';

jest.mock('react-i18next', () => ({
  // Mock the useTranslation hook
  useTranslation: () => ({ t: key => key }),
}));

test('renders greeting in the default language', () => {
  // Render the component with the I18nextProvider and the mock i18n instance
  const { getByText } = render(
    <I18nextProvider i18n={i18n}>
      <MyComponent />
    </I18nextProvider>
  );

  // Assert that the greeting is rendered with the default language key
  expect(getByText('greeting')).toBeInTheDocument();
});
```