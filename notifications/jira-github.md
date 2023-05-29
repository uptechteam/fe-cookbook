# Integration JIRA tickets with GitHub

To integrate JIRA tickets with pull requests (PRs) in GitHub, you can use the JIRA and GitHub integration provided by Atlassian. This integration allows you to link JIRA issues with GitHub repositories, and it provides a seamless way to track and manage work across both platforms.

1. Enable the JIRA integration in GitHub:

  In your GitHub repository, go to the "Settings" tab and select "Integrations & services" (or "Integrations" in newer versions). Look for the JIRA integration and follow the instructions to enable it. You'll need to provide the URL of your JIRA instance and authorize the integration to access the necessary resources.

  Here is an example of addded JIRA integration to the project:

  ![Jira integration](https://github.com/uptechteam/fe-cookbook/assets/13544983/0cf7889e-e9bc-485d-9237-cb9ba51dbf96)

  Here is an example of how to add JIRA integration to your project from your current organization:

  ![Jira configuration](https://github.com/uptechteam/fe-cookbook/assets/13544983/212e5e65-dca1-4a4b-ac61-e299c9e1658d)

2. Link JIRA issue keys to GitHub PRs:
  Once the integration is enabled, you can start linking JIRA issues to GitHub PRs. In the body of a PR or a commit message, simply include the JIRA issue key, such as "PROJECT-123." GitHub will recognize the JIRA issue key and automatically link it.
  
  Here is an example of PR with JIRA FEC-33 ticket number:
  ![Ticket example](https://github.com/uptechteam/fe-cookbook/assets/13544983/7b6fa436-b24f-4f75-a097-f4f276a59153)

3. View linked JIRA issues:
  On the GitHub PR page, you'll be able to see the linked JIRA issues. They may appear in the sidebar or in a separate section, depending on the GitHub UI version. Clicking on a linked issue will take you directly to the corresponding issue in JIRA.

4. Automatic status updates:
   As you work on the PR and make changes, the JIRA issue linked to the PR will automatically reflect those changes. For example, if the PR is merged, the corresponding JIRA issue may transition to a "Done" status. This synchronization helps keep both platforms updated and aligned.

  ![Ticket example with PR changes](https://github.com/uptechteam/fe-cookbook/assets/13544983/dca211a6-5459-440f-92e0-5de44273b677)

5. Additional features:
  The JIRA and GitHub integration offers various additional features, such as the ability to create branches from JIRA issues, view development information in JIRA, and more. Explore the integration settings and documentation to leverage these features.

Remember that the specific steps and features may vary depending on the versions of JIRA, GitHub, and the integration itself. Make sure to consult the respective documentation for more detailed instructions based on your specific setup.
