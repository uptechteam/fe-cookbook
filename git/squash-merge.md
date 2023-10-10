# Squash & Merge Documentation for GitHub Pull Requests

## Table of Contents

1. [Introduction](#introduction)
2. [What is Squash & Merge?](#what-is-squash--merge)
3. [When to Use Squash & Merge?](#when-to-use-squash--merge)
4. [How to Squash & Merge on GitHub](#how-to-squash--merge-on-github)
5. [How to Squash & Merge in Terminal](#how-to-squash--merge-in-terminal)
6. [Best Practices](#best-practices)
7. [Conclusion](#conclusion)

## Introduction

This document provides a guide on how to use the "Squash & Merge" feature in GitHub pull requests. Squash & Merge is a powerful option for integrating changes from a feature branch into the main branch while keeping the commit history clean and concise.

## What is Squash & Merge?

Squash & Merge is a merging strategy offered by GitHub to combine the commits from a feature branch into a single commit before merging them into the target branch (usually the main branch). Instead of keeping the entire commit history, it condenses multiple commits into one, providing a cleaner and more streamlined history.

## When to Use Squash & Merge?

Squash & Merge is particularly useful in the following scenarios:

- **Maintaining a Clean Commit History**: If the feature branch contains several small, incremental commits that are not essential for the main branch's history, using Squash & Merge helps avoid clutter and keeps the history focused on significant changes.

- **Pull Request Review Process**: For large features or bug fixes, it is often helpful to create multiple commits during development for better tracking and code review. However, when it comes to merging, squashing those commits can improve the readability of the main branch's history.

## How to Squash & Merge on GitHub

To Squash & Merge a pull request on GitHub, follow these steps:

1. **Open the pull request**: Navigate to the pull request page on GitHub.

2. **Review the changes**: Ensure that the pull request includes all the necessary changes and has passed any required checks or tests.

3. **Initiate the Squash & Merge**: Click on the "Merge" button on the pull request page, then select "Squash and merge" from the dropdown menu.

4. **Edit the commit message**: GitHub will present a text box where you can edit the commit message that will be used for the squashed commit. Provide a descriptive and meaningful message that summarizes the changes in the pull request.

5. **Squash and confirm**: Once you are satisfied with the commit message, click the "Squash and merge" button to complete the process.

6. **Delete the feature branch (optional)**: After the Squash & Merge is successful, you may choose to delete the feature branch to keep the repository tidy.

## How to Squash & Merge in Terminal

To Squash & Merge a pull request using the terminal, follow these steps:

1. **Fetch the latest changes**: Ensure your local repository is up to date by running `git fetch` to get the latest updates from the remote repository.

2. **Checkout the feature branch**: Switch to the feature branch that you want to squash and merge: `git checkout feature-branch`.

3. **Rebase the branch**: Perform an interactive rebase to squash the commits into one. Use the following command: `git rebase -i main`.

4. **Squash the commits**: An interactive editor will open, showing a list of commits. Change the word "pick" to "squash" for all the commits except the first one, then save and close the editor.

5. **Edit the commit message**: Another editor will open, allowing you to edit the commit message for the squashed commit. Provide a meaningful message and save the file.

6. **Complete the rebase**: Finish the rebase by running `git rebase --continue`.

7. **Push the changes**: Push the squashed and rebased branch to the remote repository: `git push origin feature-branch`.

8. **Create the pull request**: Go to GitHub and create a pull request for the squashed and rebased branch.

9. **Merge the pull request**: Follow the steps outlined in the "How to Squash & Merge on GitHub" section to merge the pull request using Squash & Merge.

## Best Practices

- **Clear Commit Messages**: When using Squash & Merge, it's crucial to provide a clear and descriptive commit message that accurately reflects the changes made in the pull request.

- **Regular Rebasing**: To avoid conflicts during the Squash & Merge process, it's good practice to regularly rebase the feature branch on top of the target branch before creating the pull request.

- **Discuss with Team**: Before using Squash & Merge, ensure that it aligns with your team's workflow and version control practices. Some teams prefer a different merging strategy.

## Conclusion

Squash & Merge is an effective way to keep your commit history concise and organized, especially when integrating feature branches into the main branch. By following the steps outlined in this guide and adopting best practices, you can make the most of this feature to enhance your Git workflow on GitHub.

For more details on GitHub's Squash & Merge feature, visit the [GitHub documentation](https://docs.github.com/en/github/collaborating-with-pull-requests/incorporating-changes-into-a-pull-request/squashing-commits). Happy collaborating!
