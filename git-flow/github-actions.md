# GitHub Actions Documentation

GitHub Actions is a powerful automation and continuous integration/continuous deployment (CI/CD) platform provided by GitHub. It allows you to define custom workflows that automate various tasks in your software development process. This documentation will guide you through the basics of using GitHub Actions in your repository.

## Table of Contents

1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [Workflow Structure](#workflow-structure)
4. [Triggering Workflows](#triggering-workflows)
5. [Using Marketplace Actions](#using-marketplace-actions)
6. [Secrets](#secrets)
7. [Debugging Workflows](#debugging-workflows)
8. [Example Workflows](#best-practices)
9. [Conclusion](#conclusion)

## 1. Introduction

GitHub Actions enables you to automate tasks in your GitHub repositories, such as building, testing, and deploying your code. You can define custom workflows using YAML syntax, which are executed based on various events or triggers within your repository.

## 2. Getting Started

To get started with GitHub Actions:

1. **Enable Actions:** Go to the "Actions" tab in your GitHub repository and follow the prompts to enable Actions.

2. **Create a Workflow:** In your repository, create a new directory called `.github/workflows`. Inside this directory, create a new YAML file, e.g., `example.yml`, and define your workflow in it.

3. **Write a Workflow:** Define your workflow with jobs, steps, and actions. Use the available actions and tools to build, test, and deploy your code.

## 3. Workflow Structure

A workflow consists of one or more jobs, and each job consists of one or more steps. Steps are individual tasks that execute in sequence. You can use existing actions or write your own custom actions to define the steps.

A GitHub Actions workflow is defined using YAML syntax and consists of several key components that define the automation tasks to be performed. Below is an overview of the workflow structure:

### 3.1 Runs On

GitHub Actions can run on the following operating systems:

1. **Linux**: GitHub Actions primarily runs on Linux virtual machines, and it's the default environment for most workflows.

2. **macOS**: GitHub Actions also supports macOS virtual machines, allowing you to build and test applications specifically designed for Apple platforms.

3. **Windows**: GitHub Actions provides Windows virtual machines for running workflows that target Windows-based applications.

### Supported Runners

GitHub Actions uses runners to execute workflows. A runner is a machine that's hosted by GitHub or your own infrastructure. GitHub provides various types of runners:

1. **GitHub-hosted runners**: These are virtual machines provided by GitHub to run workflows. They come in different configurations based on the operating system and tooling.

2. **Self-hosted runners**: You can set up your own machines as runners to execute GitHub Actions workflows. This allows for more customization and control over the runner environment.

### System Requirements for Self-Hosted Runners

If you plan to use self-hosted runners for your GitHub Actions workflows, here are the general system requirements:

- **Operating System**: Self-hosted runners can run on Linux, macOS, and Windows.
- **Processor**: Modern multi-core processors are recommended for better performance.
- **Memory**: The minimum required RAM may vary based on the types of workflows you intend to run. As a guideline, at least 2GB of RAM is recommended, but more may be needed for complex workflows.
- **Disk Space**: Sufficient disk space is necessary to store repositories, dependencies, and artifacts generated during workflows.
- **Network**: Reliable internet connectivity is essential for the runner to communicate with GitHub's servers.

### Software Requirements for Self-Hosted Runners

To set up self-hosted runners, ensure that the following software is installed on the runner machine:

- **Git**: You need Git installed on the runner machine to clone repositories and interact with version control.
- **Docker (if used)**: If your workflows involve Docker containers, you need Docker installed on the runner machine.
- **Language Runtimes**: Depending on the programming languages you use in your workflows, you might need to install the corresponding language runtimes (e.g., Node.js, Python, Java).

### 3.2 Workflow Name

The name of the workflow is used to identify it in the GitHub Actions dashboard and can be any descriptive string.

```yaml
name: My Workflow
```

### 3.3 Events (on)

The `on` keyword specifies the events that trigger the workflow. These events can be actions like push, pull requests, issue comments, scheduled events, etc.

```yaml
on:
  push:
    branches:
      - main
      - feature/*
  pull_request:
    types:
      - opened
      - synchronize
```

### 3.4 Jobs

A workflow can consist of one or more jobs. Each job is a set of steps that execute on the same runner. Jobs run in parallel by default but can also be defined to run sequentially.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Build application
        run: npm run build
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Install dependencies
        run: npm install
      - name: Run tests
        run: npm test
```

### 3.5 Steps

Each job consists of a series of steps. Steps are individual tasks that are executed in sequence. They can be either predefined actions provided by GitHub or custom shell commands.

```yaml
steps:
  - name: Checkout code
    uses: actions/checkout@v2
  - name: Install dependencies
    run: npm install
  - name: Build application
    run: npm run build
```

### 3.6 Environment

The `env` keyword allows you to set environment variables that will be available to all the steps in a job.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      MY_VARIABLE: 'some-value'
    steps:
      ...
```

### 3.7 Services

The `services` keyword allows you to define additional services that are run alongside the job. These services can be used to provide additional dependencies, databases, etc.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    services:
      mysql:
        image: mysql:5.7
        env:
          MYSQL_ROOT_PASSWORD: password
        ports:
          - 3306:3306
    steps:
      ...
```

### 3.8 Secrets

Secrets are encrypted environment variables that can be used in your workflows. They allow you to store sensitive information securely, such as API keys and passwords.

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        env:
          SECRET_API_KEY: ${{ secrets.API_KEY }}
        run: deploy_script.sh
```

### 3.9 Artifacts

Artifacts allow you to persist data between jobs in a workflow. They can be used to pass build outputs, test results, or other data between different stages of the workflow.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build application
        run: npm run build
        # Save build artifacts
        # These files can be accessed in subsequent jobs
        # using the `needs` keyword and the `artifacts` property.
        # e.g., ${{ needs.build.outputs.artifacts_folder }}
        # or ${{ needs.build.outputs.some_file_path }}
        # or ${{ needs.build.artifacts.some_artifact_name }}
        # See the documentation for more details.
        id: build
        continue-on-error: true
        if: always()
        env:
          artifacts_folder: dist/
        # Artifacts are saved under the `dist` folder
        # which can be accessed in subsequent jobs
        # using the `needs` keyword and the `artifacts` property.
        - name: Upload artifacts
          uses: actions/upload-artifact@v2
          with:
            name: my-artifact
            path: ${{ env.artifacts_folder }}
```

These are the essential components that make up the structure of a GitHub Actions workflow. You can customize your workflow by adding more jobs, steps, and actions based on your specific use case and requirements.

## 4. Triggering Workflows

Workflows can be triggered in response to various events, such as:

- Push to a specific branch
- Pull requests
- Issue comments
- Repository dispatches
- Scheduled events
- And more...

### 4.1 Push Event

The most common event is the `push` event, which occurs when code is pushed to the repository. You can specify the branches that should trigger the workflow.

```yaml
name: Build and Test on Push

on:
  push:
    branches:
      - main
      - development
```

### 4.2 Pull Request Event

You can trigger a workflow when a pull request is opened or when changes are pushed to an existing pull request.

```yaml
name: Build and Test on Pull Request

on:
  pull_request:
    branches:
      - main
```

### 4.3 Scheduled Event

You can schedule a workflow to run at specific intervals, such as daily or weekly.

```yaml
name: Nightly Build

on:
  schedule:
    - cron: '0 0 * * *'  # Run every day at midnight (UTC)
```

### 4.4 Issue Comment Event

You can trigger a workflow when a new comment is posted on an issue.

```yaml
name: Comment Notification

on:
  issue_comment:
    types: [created]
```

### 4.5 Manual Trigger

You can create a workflow that is triggered manually by you or other repository collaborators.

```yaml
name: Manual Deployment

on:
  workflow_dispatch:
```

### 4.6 Custom Event

You can also create custom events using repository dispatches to trigger workflows from external systems or APIs.

```yaml
name: Custom Event

on:
  repository_dispatch:
    types: [custom-event-name]
```

### 4.7 Tags 

You can trigger a GitHub Actions workflow when a new Git tag is created in your repository. This can be useful for performing specific tasks, such as creating releases or deploying your application when you tag a specific version.

```yaml
name: Build and Deploy on Tag

on:
  create:
    tags:
      - 'v*'  # Match tags starting with 'v', e.g., v1.0.0, v2.3.1
```

These are just a few examples of how you can trigger workflows using different events in GitHub Actions. You can combine these events or use other events to suit your specific use case.

## 5. Using Marketplace Actions

GitHub Marketplace offers a wide range of community-contributed actions that you can use directly in your workflows. These actions cover a variety of tasks like linting, testing, building, deploying, and more.

Using GitHub Marketplace actions in your workflows is straightforward. Follow these steps:

1. **Navigate to the GitHub Marketplace:** Go to [https://github.com/marketplace](https://github.com/marketplace).

2. **Search for the Action:** Look for the specific action you want to use in your workflow, such as "eslint," "maven," "docker," etc.

3. **Open the Action Page:** Click on the action to open its page.

4. **Get Usage Instructions:** On the action's page, you'll find usage instructions, including the YAML code you need to include in your workflow file.

5. **Copy the YAML Code:** Copy the YAML code provided on the action's page.

6. **Edit Your Workflow File:** Open the YAML file (e.g., `.github/workflows/build.yml`) in your repository where you want to use the action.

7. **Paste the Code:** Paste the copied YAML code under the `steps` section in your workflow file.

8. **Customize the Action:** Modify the action's inputs and parameters according to your needs. These inputs are usually provided in the YAML code on the action's page.

9. **Commit and Push:** Save your changes and commit them to your repository.

Here's a general example of how you might use a marketplace action in a workflow:

```yaml
name: Build and Test

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      # Use the GitHub Marketplace action here
      - name: Run ESLint
        uses: github/eslint-action@v5
        with:
          eslint-args: --ext .js,.jsx .

      # Add more steps as needed for your workflow
      - name: Install Dependencies
        run: npm install

      - name: Build
        run: npm run build

      - name: Run Tests
        run: npm test
```

## 6. Secrets

Secrets are encrypted environment variables that can be used in your workflows. They allow you to store sensitive information securely, such as API keys and passwords.

To define and use secrets in GitHub Actions, follow these steps:

1. Navigate to your GitHub repository in which you want to use the secrets.

2. Click on the "Settings" tab at the top of the repository.

3. In the left sidebar, click on "Secrets".

4. Click on the "New repository secret" button.

5. Give your secret a name (e.g., API_KEY) and provide the value of the secret (e.g., my_secret_api_key).

6. Click "Add secret" to save the secret.

Once you have defined a secret, you can use it in your GitHub Actions workflows using the `${{ secrets.SECRET_NAME }}` syntax. Here's an example of how to use a secret in a workflow:


Here, we provide some useful examples of common workflows:

### Build and Test Example

```yaml
name: Build and Test

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Install dependencies
      run: npm install

    - name: Build
      run: npm run build

    - name: Run tests
      run: npm test
```

### Deploy to Production Example

```yaml
name: Deploy to Production

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Install dependencies
      run: npm install

    - name: Build
      run: npm run build

    - name: Deploy
      uses: some-action/deploy@v1
      with:
        token: ${{ secrets.DEPLOY_TOKEN }}
```

## 7. Debugging Workflows

To test a GitHub Actions workflow locally, you can use the `act` tool, which allows you to run GitHub Actions workflows locally on your development machine. `act` simulates the GitHub Actions runner environment, allowing you to test your workflows before pushing them to your repository. Here's how you can use `act` to test a workflow locally:

### 7.1 **Install `act`:**
   - First, you need to install the `act` tool on your local machine. Follow the installation instructions for your operating system from the official `act` repository: [https://github.com/nektos/act](https://github.com/nektos/act)

### 7.2 **Clone your GitHub repository:**
   - Clone the GitHub repository that contains the workflow you want to test to your local machine.

### 7.3 **Navigate to the repository directory:**
   - Open your terminal or command prompt and change to the directory of your cloned repository.

### 7.4 **Run `act`:**
   - To run the workflow defined in your repository, simply execute the `act` command in your terminal.

```bash
act
```

### 7.5 **Observe the output:**
   - `act` will simulate the GitHub Actions runner and execute the workflow steps in the same order as they would be executed in the GitHub environment.
   - Observe the output and ensure that the workflow behaves as expected.
   - `act` will also provide a summary of the workflow run, showing if it was successful or if there were any errors.

### 7.6 **Customize the event:**
   - By default, `act` runs the workflow with a `push` event. If your workflow triggers on other events, such as `pull_request`, `schedule`, etc., you can specify the event with the `-e` flag.

```bash
act -e pull_request
```

### 7.7 **Test different branches and events:**
   - You can also test different branches, tags, or other events by specifying them with the `-b` flag.

```bash
act -b my-feature-branch
act -b v1.0.0
```

By using `act`, you can iteratively test your GitHub Actions workflow locally, making it easier to troubleshoot and validate your workflow before pushing it to your repository. Please note that some actions might not work perfectly in a local environment due to dependencies or other external factors. In such cases, testing in an actual GitHub repository environment is recommended for complete validation.

## 8. Example Workflow
```yaml
name: CD
on:
  push
jobs:
    install:
        name: Install 🧶
        runs-on: ubuntu-latest
        if: ${{ github.ref == 'refs/heads/master'}}
        continue-on-error: false
        steps:
          - name: 🛑 Cancel Previous Runs
            uses: styfle/cancel-workflow-action@0.10.0

          - name: Checkout
            uses: actions/checkout@v3
            with:
                token: ${{ secrets.GIT_TOKEN || github.token  }}

          - name: Get yarn cache directory path
            id: yarn-cache-dir-path
            run: echo "::set-output name=dir::$(yarn cache dir)"

          - name: Cache yarn files
            uses: actions/cache@v3
            id: yarn-cache-files
            with:
                path: ${{ steps.yarn-cache-dir-path.outputs.dir }}
                key: ${{ runner.os }}-yarn-files-${{ hashFiles('**/yarn.lock') }}
                restore-keys: ${{ runner.os }}-yarn-files-

          - name: Setup yarn access
            run: |
                git config --global url."https://${{ secrets.GIT_TOKEN || github.token  }}:x-oauth-basic@github.com/".insteadOf "https://github.com/"
                sed -i "s|Builder/|git+https://github.com/project/|" package.json
          - name: Install JS dependencies
            run: yarn install

          - name: Pack artifact
            run: tar czf /tmp/artifact.tar.gz .

          - name: Upload artifact
            uses: actions/upload-artifact@v3
            with:
                name: frontend-artifact
                path: /tmp/artifact.tar.gz
                retention-days: 1

    chromatic-deployment:
      name: Chromatic Deployment 📗
      if: ${{ github.ref == 'refs/heads/master'}}
      runs-on: ubuntu-latest
      steps:
        - name: 🛑 Cancel Previous Runs
          uses: styfle/cancel-workflow-action@0.10.0

        - name: Checkout
          uses: actions/checkout@v1

        - name: Install 🧶
          run: yarn

        - name: Publish to Chromatic 📗
          uses: chromaui/action@v1
          with:
            projectToken: ${{ secrets.CHROMATIC_PROJECT_TOKEN }}

    eslint:
      name: Eslint 🤡
      needs: [install]
      runs-on: ubuntu-latest
      continue-on-error: false
      steps:
          - name: Download artifact
            uses: actions/download-artifact@v3
            with:
                name: frontend-artifact

          - name: Unpack artifact
            run: tar xf artifact.tar.gz

          - name: Setup problem matcher for eslint
            run: echo "::add-matcher::.github/configs/eslint-matcher.json"

          - name: Run eslint
            run: yarn eslint

    stylelint:
      name: Stylelint 💅
      needs: [install]
      runs-on: ubuntu-latest
      continue-on-error: false
      steps:
          - name: Download artifact
            uses: actions/download-artifact@v3
            with:
                name: frontend-artifact

          - name: Unpack artifact
            run: tar xf artifact.tar.gz

          - name: Run stylelint
            run: yarn stylelint

    typescript:
        name: Typescript 🎪
        needs: [install]
        runs-on: ubuntu-latest
        continue-on-error: false
        steps:
            - name: Download artifact
              uses: actions/download-artifact@v3
              with:
                  name: frontend-artifact

            - name: Unpack artifact
              run: tar xf artifact.tar.gz

            - name: Run typescript compile
              run: yarn tsc --noEmit
    tests:
      name: Unit tests 🧪
      needs: [install]
      runs-on: ubuntu-latest
      steps:
        - name: 🛑 Cancel Previous Runs
          uses: styfle/cancel-workflow-action@0.10.0

        - name: Checkout
          uses: actions/checkout@v3
          with:
            token: ${{ secrets.GIT_TOKEN || github.token  }}

        - name: Download artifact
          uses: actions/download-artifact@v3
          with:
              name: frontend-artifact

        - name: Unpack artifact
          run: tar xf artifact.tar.gz

        - name: Testing 🧪
          run: yarn test

    deploy-production:
      name: Deploy Production 🚀
      needs: [install]
      runs-on: ubuntu-latest
      steps:
        - name: 🛑 Cancel Previous Runs
          uses: styfle/cancel-workflow-action@0.10.0

        - name: Checkout
          uses: actions/checkout@v3
          with:
            token: ${{ secrets.GIT_TOKEN || github.token  }}

        - name: Download artifact
          uses: actions/download-artifact@v3
          with:
              name: frontend-artifact

        - name: Unpack artifact
          run: tar xf artifact.tar.gz

        - name: Build TS application
          run: yarn build

        - name: Deploy to Netlify 🚀
          uses: nwtgck/actions-netlify@v1.2
          with:
            publish-dir: './build'
            production-branch: master
            production-deploy: true
            github-token: ${{ secrets.GITHUB_TOKEN }}
            deploy-message: "Deploy to Netlify from GitHub Actions 🚀"
            enable-pull-request-comment: false
            enable-commit-comment: false
            overwrites-pull-request-comment: false
            github-deployment-environment: 'deploy-production'
            alias: ${{ github.head_ref }}
          env:
            NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
            NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}

    E2E:
       name: Playwright 🧪
       needs: [deploy-production]
       runs-on: ubuntu-latest
       timeout-minutes: 60
       steps:
         - name: 🛑 Cancel Previous Runs
           uses: styfle/cancel-workflow-action@0.10.0

         - name: Checkout
           uses: actions/checkout@v3
           with:
            token: ${{ secrets.GIT_TOKEN || github.token  }}

          - name: Download artifact
            uses: actions/download-artifact@v3
            with:
                name: frontend-artifact

         - name: Unpack artifact
           run: tar xf artifact.tar.gz

         - name: Install Playwright 🧪
           run: npm install @playwright/test && npx playwright install

         - name: Run E2E Tests on Netlify URL
           run: yarn test:e2e
           env:
           PLAYWRIGHT_TEST_BASE_URL: https://example.netlify.app
```
The provided GitHub Actions workflow is named "CD" and is triggered on `push` events. The workflow consists of several jobs, each serving a specific purpose in the Continuous Deployment process. Below is an explanation of each job:

1. **install:**
   - This job installs and prepares the necessary dependencies for the application to be deployed.
   - It checks out the code, sets up the yarn cache, installs JavaScript dependencies, and packs the artifact to be used in other jobs.
   - It is conditional to run only when the branch is `master`.

2. **chromatic-deployment:**
   - This job is responsible for deploying the application to Chromatic, a visual testing tool for UI components.
   - It is also conditional to run only when the branch is `master`.

3. **eslint:**
   - This job performs linting using ESLint on the application code.
   - It uses the previously packed artifact from the `install` job and then sets up problem matchers for ESLint to display proper error messages.

4. **stylelint:**
   - This job performs linting using Stylelint on the application's stylesheets.
   - It uses the previously packed artifact from the `install` job.

5. **typescript:**
   - This job checks the TypeScript types for the application.
   - It uses the previously packed artifact from the `install` job.

6. **tests:**
   - This job runs unit tests for the application.
   - It uses the previously packed artifact from the `install` job.

7. **deploy-production:**
   - This job is responsible for deploying the application to Netlify for production.
   - It builds the TypeScript application, deploys it to Netlify, and sets some additional configurations for deployment.
   - It uses the previously packed artifact from the `install` job.
   - This job is also conditional to run only when the branch is `master`.

8. **E2E:**
   - It is likely intended for end-to-end (E2E) testing using Playwright on the deployed production site.

Please note that some parts of the workflow contain comments (lines starting with `#`) that provide additional information or explanations for future actions or jobs.

Overall, this GitHub Actions workflow automates various stages of Continuous Deployment, including linting, type checking, unit testing, and deploying the application to Chromatic and Netlify for production. It follows best practices to ensure that the codebase is of high quality and properly deployed for production use.

## 9. Conclusion

GitHub Actions is a versatile automation platform that can streamline your software development processes. By leveraging its features, you can build, test, and deploy your code with confidence, ultimately improving your team's productivity and software quality.

Now, you're ready to dive into the world of GitHub Actions and start automating your workflows! Happy coding!