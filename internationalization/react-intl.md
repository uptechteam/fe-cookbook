# React-Intl 

1.  [Integration](#integration)
2.  [Switcher](#switcher)
3.  [Mock tests](#mock-tests)

*   Advantages:
    *   Comprehensive internationalization support with features like message formatting, pluralization, and date/time formatting.
    *   React components for easy integration and usage.
    *   Strong community support and widely adopted.
*   Disadvantages:
    *   Can have a learning curve for complex use cases.
    *   Requires additional setup and configuration.

## Integration

1\. Install the necessary dependencies. Make sure you have React and React-Intl installed in your project. You can install them using pnpm or yarn:

```javascript
pnpm install react-intl
```

2\. Set up the localization messages. Create localization message files for each language you want to support. These files will contain translations for different keys used in your app. For example, create a file **en.json** for English and **fr.json** for French:

```javascript
// en.json
{
  "app.greeting": "Hello!",
  "app.message": "Welcome to my app!",
  "language.en": "English",
  "language.fr": "French"
}

// fr.json
{
  "app.greeting": "Bonjour!",
  "app.message": "Bienvenue dans mon application !",
  "language.en": "Anglais",
  "language.fr": "Français"
}
```

3\.  Implement the language change functionality. In your app's parent component, handle the language change functionality. You can use the **setState** hook or a state management library like Redux to manage the current language state. When the language changes, update the **IntlProvider** with the new locale and messages. Here's an example:

```javascript
import { useState } from 'react';
import { IntlProvider } from 'react-intl';
import LanguageSwitcher from './LanguageSwitcher';
import Greeting from './Greeting';
import enMessages from './locales/en.json';
import frMessages from './locales/fr.json';

const App = () => {
  const [locale, setLocale] = useState('en');
  const messages = locale === 'en' ? enMessages : frMessages;

  const handleLanguageChange = (language) => {
    setLocale(language);
  };

  return (
    <IntlProvider locale={locale} messages={messages}>
      <LanguageSwitcher
        languages={['en', 'fr']}
        currentLanguage={locale}
        onLanguageChange={handleLanguageChange}
      />
      <Greeting />
    </IntlProvider>
  );
};

export default App;
```

4\. Use the **FormattedMessage** component in your app components, use the **FormattedMessage** component from **react-intl** to display localized messages. The **FormattedMessage** component uses the **id** of a message to retrieve the appropriate translation from the provided messages. Here's an example:

```javascript
import { FormattedMessage } from 'react-intl';

const Greeting = () => {
  return (
    <div>
      <FormattedMessage id="app.greeting" defaultMessage="Hello!" />
      <FormattedMessage id="app.message" defaultMessage="Welcome to my app!" />
    </div>
  );
};

export default Greeting;
```

## Switcher

Create a language switcher component that allows users to switch between different languages. This component will update the locale in the **IntlProvider** component from **react-intl**. Here's an example of a language switcher component:

```javascript
import { FormattedMessage } from 'react-intl';

const LanguageSwitcher = ({ languages, currentLanguage, onLanguageChange }) => {
  return (
    <div>
      {languages.map((language) => (
        <button
          key={language}
          onClick={() => onLanguageChange(language)}
          disabled={currentLanguage === language}
        >
          <FormattedMessage id={`language.${language}`} defaultMessage={language} />
        </button>
      ))}
    </div>
  );
};

export default LanguageSwitcher;
```

## Mock tests

To mock tests involving **react-intl**, you can use the **react-intl-test-utils** library. It provides helper functions for testing components that use **react-intl**. Here's an example of a test using **react-intl-test-utils**:

1\. Install **react-intl-test-utils** before running the tests:  

```javascript
pnpm install react-intl-test-utils
```

2\. Add tests

```javascript
import { render } from '@testing-library/react';
import { IntlProvider } from 'react-intl';
import MyComponent from './MyComponent';

const messages = {
  'app.greeting': 'Hello!',
};

test('renders the greeting message', () => {
  const { getByText } = render(
    <IntlProvider locale="en" messages={messages}>
      <MyComponent />
    </IntlProvider>
  );

  const greetingElement = getByText(/Hello!/i);
  expect(greetingElement).toBeInTheDocument();
});
```