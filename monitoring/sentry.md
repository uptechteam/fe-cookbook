# Sentry

## What is it?

Sentry is an open-source error monitoring and crash reporting tool used by developers and organizations to track and debug software errors and exceptions in real-time. It is designed to help developers identify and fix issues in their applications quickly. 

With Sentry, developers can integrate the tool into their applications and collect data on errors, exceptions, and crashes that occur during runtime. When an error or exception is detected, Sentry captures relevant information such as the stack trace, environment variables, and user context, providing valuable insights into the root cause of the issue.

The captured data is then aggregated and displayed in the Sentry dashboard, where developers can view and analyze the errors, track their occurrence frequency, and prioritize them based on their impact. Sentry also provides features such as issue assignment, notifications, and release tracking, enabling teams to collaborate and resolve issues efficiently.

## How to use Sentry in React applications

To use Sentry in a React application, you can follow these general steps:

1. Check out the official guide of creating a new project in Sentry [admin panel](https://docs.sentry.io/product/sentry-basics/integrate-frontend/create-new-project/).

2. Configure [alert rules](https://uptech-team.sentry.io/alerts/rules/) for your project.

  ![Alerts configuring](https://github.com/uptechteam/fe-cookbook/assets/13544983/d634225e-ac50-4a11-9796-de9cccbecef0)

- Click to the "Create alert" button and set conditions for the new alert rule.

  ![Create alert](https://github.com/uptechteam/fe-cookbook/assets/13544983/c9dd4adf-782a-45e0-9e6e-88c89a4f187b)
  
- Create a rule of alerts for non-production environments. Note that here you could configure notifications in Slack channels from your organizations.

  ![Non-prod envs](https://github.com/uptechteam/fe-cookbook/assets/13544983/7fb58fe6-ddbf-4ae5-ab5b-3ebf67a1cf34)

- Create a rule of alerts for the production environment.

  ![Prod env](https://github.com/uptechteam/fe-cookbook/assets/13544983/c07f8b04-0bb8-49fa-ac02-1d86dbe2727d)

3. Get the project DSN key:

 ![Create alert](https://github.com/uptechteam/fe-cookbook/assets/13544983/1e0ea85f-41f1-4bcc-8e1c-d9d284abc60a)

4. Add this key in your ```.env``` file.
5. Add other Sentry env variables to your ```.env``` file:

  ```js
  REACT_APP_SENTRY_DSN=
  REACT_APP_SENTRY_AUTH_TOKEN=
  REACT_APP_SENTRY_ORG=your-organization-name
  REACT_APP_SENTRY_PROJECT=your-project-name
  REACT_APP_ENV=local // make this env variable dynamic for different environments
  ```

6. Check out Sentry configuration [guide](https://docs.sentry.io/platforms/javascript/guides/react/) for React applications.

7. Configure Sentry in your ```index.tsx``` file:

  ```typescript
  import * as Sentry from "@sentry/react";

  Sentry.init({
    dsn:
      process.env.REACT_APP_ENV !== "local"
        ? process.env.REACT_APP_SENTRY_DSN
        : "",
    environment: process.env.REACT_APP_ENV,
    release: process.env.REACT_APP_RELEASE_VERSION, // Configure this variable in your CI to make it dynamic across diffent environments
    integrations: [new BrowserTracing()],
    tracesSampleRate: 1.0,
  });
  ```

8. Configure your error boundary component:

  ```typescript
  import { Component, ErrorInfo, ReactNode } from "react";
  import { withScope, captureException, SeverityLevel } from "@sentry/react";
  import { Error } from "@pages";

  interface IProps {
    children: ReactNode;
  }

  interface IState {
    error: null | Error;
  }

  export class ErrorBoundary extends Component<IProps, IState> {
    constructor(props: IProps) {
      super(props);
      this.state = { error: null };

      this.clearState = this.clearState.bind(this);
    }

    public componentDidCatch(error: Error, errorInfo: ErrorInfo): void {
      this.setState({ error });
      withScope((scope) => {
        scope.setTag("Custom-Tag", "ErrorBoundary");
        scope.setLevel("Error" as SeverityLevel);
        Object.keys(errorInfo).forEach((key) => {
          scope.setExtra(key, errorInfo[key as keyof ErrorInfo]);
        });
        captureException(error);
      });
    }

    public clearState() {
      this.setState({ error: null });
    }

    public render(): ReactNode {
      const { error } = this.state;

      if (error) {
        return <Error clearState={this.clearState} />;
      }
      const { children } = this.props;
      return children;
    }
  }
  ```

9. You can also create a helper function ```reportError``` for catching errors in Sentry:

```typescript
interface IReportError {
  error: string | Error;
  tagKey?: string;
  tagValue?: string;
  extraInfo?: Extras;
}

export const reportError = ({
  error,
  tagKey,
  tagValue,
  extraInfo,
}: IReportError) => {
  withScope((scope) => {
    if (tagKey && tagValue) {
      scope.setTag(tagKey, tagValue);
    }
    if (extraInfo) {
      scope.setExtras(extraInfo);
    }
    scope.setLevel("Error" as SeverityLevel);

    captureException(error);
  });
};
```

10. After that, you can call it whenever you need in your code:

    - In Axios interceptors:

      ```typescript
        Axios.interceptors.response.use(undefined, (error) => {
          reportError({
            error: `Request failed (${error.response.config.method}/ ${error.response.config.url})`,
            extraInfo: error.response.config.data
              ? JSON.parse(error.response.config.data)
              : {},
          });

          const { title, description } = parseServerException(error);
          displayToastError(title, description);
          return Promise.reject(error);
        });
      ```

    - In other places in your app:

    ```typescript
    try {
      await uploadFileToS3(file, uploadURL).then(() =>
        setValue(fieldName, imageKey, { shouldDirty: true })
      );
    } catch {
      reportError({ error: "Image file uploading error" });
    }
    ```

11. To enable readable stack traces in your Sentry errors, you need to upload your source maps to Sentry. Check out [guides](https://docs.sentry.io/platforms/javascript/guides/react/sourcemaps/uploading/) of how to upload source maps.

12. If you use ```craco``` on your project, feel free to use this config:

 ```typescript
  const CracoAlias = require("craco-alias");
  const SentryWebpackPlugin = require("@sentry/webpack-plugin");

  module.exports = {
    devtool: "source-map",
    plugins: [
      {
        plugin: CracoAlias,
        options: {
          source: "tsconfig",
          baseUrl: ".",
          tsConfigPath: "./tsconfig.path.json",
        },
      },
      {
        plugin: SentryWebpackPlugin,
        options: {
          org: process.env.REACT_APP_SENTRY_ORG,
          project: process.env.REACT_APP_SENTRY_PROJECT,
          include: "build/static/",
          urlPrefix: "~/static/",
          authToken: process.env.SENTRY_AUTH_TOKEN,
          release: process.env.REACT_APP_RELEASE_VERSION,
        },
      },
    ],
  };

  export {};
  ```

13. Here is an example of CI job (CircleCI) for uploading source maps to Sentry:

```
  notify-sentry-deploy:
    executor: app-executor
    steps:
      - checkout
      - attach_workspace:
          at: .
      - run: |
          cat bash.env > $BASH_ENV
      - run: |
          printenv REACT_APP_RELEASE_VERSION
          printenv REACT_APP_ENV
      - run:
          name: Create release and notify Sentry of deploy
          command: |
            curl -sL https://sentry.io/get-cli/ | bash
            sentry-cli releases -o $REACT_APP_SENTRY_ORG new -p $SENTRY_PROJECT $REACT_APP_RELEASE_VERSION
            sentry-cli releases set-commits $REACT_APP_RELEASE_VERSION --auto --ignore-missing
            sentry-cli releases files $REACT_APP_RELEASE_VERSION upload-sourcemaps "~/static/"
            sentry-cli releases finalize $REACT_APP_RELEASE_VERSION
            sentry-cli releases deploys $REACT_APP_RELEASE_VERSION new -e $REACT_APP_ENV
```
