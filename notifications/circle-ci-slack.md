# Slack notifications configuring

## Why do we need Slack notifications from CI?

Here are some of the reasons why we need to use Slack notifications:

1. Real-time updates: Slack notifications from CI provide real-time updates on the status of the build process. This allows team members to quickly identify any issues that may arise during the build process and take necessary actions to fix them.
2. Improved collaboration: By integrating CI notifications with Slack, team members can easily collaborate and communicate with each other. They can share information, provide feedback, and work together to resolve issues in a timely manner.
3. Increased visibility: Slack notifications from CI provide increased visibility into the build process, allowing team members to track progress and identify bottlenecks. This can help improve the overall efficiency of the development process.
4. Quick feedback: Slack notifications can provide quick feedback on the success or failure of builds, making it easier to identify and address any issues. This can help prevent bugs and other issues from making it into production.
5. Customizable alerts: Slack notifications can be customized to meet the specific needs of the development team. For example, notifications can be sent only when specific conditions are met, such as when a build fails or when a specific branch is merged.

## How to configure CircleCI notifications in Slack

1. Check out the official [tutorial](https://circleci.com/docs/slack-orb-tutorial/) of using the Slack orb to set up Slack notifications in CircleCI.
2. Instead of suggested in tutorial message template you can use your custom message template. Here are examples of custom templates:

For failed jobs:

  ```
    - slack/notify:
        event: fail
        template: ""
        custom: |
          {
            "blocks": [
              {
                "type": "section",
                "text": {
                  "type": "mrkdwn",
                  "text": "MMT FE ${BUILD_INFO} failed to deploy ❌",
                }
              },
              {
                "type": "actions",
                "elements": [
                  {
                    "type": "button",
                    "text": {
                      "type": "plain_text",
                      "text": "View Job"
                    },
                    "url": "${CIRCLE_BUILD_URL}"
                  }
                ]
              }
            ]
          }
        channel: "eatable-robot"
  ```

For succeeded jobs:

  ```
    - slack/notify:
        event: pass
        template: ""
        custom: |
          {
            "blocks": [
              {
                "type": "section",
                "text": {
                  "type": "mrkdwn",
                  "text": "MMT FE ${BUILD_INFO} has been deployed to *${ENV_NAME}* ✅"
                }
              },
              {
                "type": "actions",
                "elements": [
                  {
                    "type": "button",
                    "text": {
                      "type": "plain_text",
                      "text": "View Job"
                    },
                    "url": "${CIRCLE_BUILD_URL}"
                  }
                ]
              }
            ]
          }
        channel: "eatable-robot"
  ```

3. Here is an example of how this message templates look in Slack (successful/failed build deploy):

  ![Slack message](https://github.com/uptechteam/fe-cookbook/assets/13544983/1d7c0064-9713-435c-9eff-80596f307e35)
