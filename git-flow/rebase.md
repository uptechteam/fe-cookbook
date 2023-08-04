# Git Rebase Documentation

## Table of Contents

1. [Introduction](#introduction)
2. [What is Rebase?](#what-is-rebase)
3. [When to Use Rebase?](#when-to-use-rebase)
4. [How to Rebase](#how-to-rebase)
    1. [Interactive Rebase](#interactive-rebase)
    2. [Rebase onto a Different Branch](#rebase-onto-a-different-branch)
5. [Common Scenarios](#common-scenarios)
    1. [Rebase vs. Merge](#rebase-vs-merge)
    2. [Resolving Conflicts during Rebase](#resolving-conflicts-during-rebase)
6. [Best Practices](#best-practices)
7. [Conclusion](#conclusion)

## Introduction

This document provides a guide on how to use `git rebase` – a powerful feature in Git that allows you to modify the commit history of a branch. Rebase can be used to apply changes from one branch onto another, among other purposes.

## What is Rebase?

Rebase is a Git command that allows you to move or combine a sequence of commits to a new base commit. It re-writes the commit history by replaying each commit on top of a different commit or branch. This can lead to a cleaner, linear history, reducing the number of merge commits.

## When to Use Rebase?

Rebase is particularly useful in the following scenarios:

- Keeping a clean commit history: By rebasing, you can create a linear history without unnecessary merge commits, making it easier to track changes and understand the project's evolution.
- Preparing for pull requests: Before submitting a pull request, you can rebase your branch onto the latest changes from the target branch to avoid potential conflicts.
- Squashing commits: Rebase allows you to combine multiple commits into one, keeping the commit history clean and concise.

## How to Rebase

### Interactive Rebase

To perform an interactive rebase, follow these steps:

1. **Open the terminal**: Navigate to your repository using the command line.

2. **Fetch the latest changes**: Run `git fetch` to get the latest updates from the remote repository.

3. **Choose the base commit**: Identify the commit you want to use as the new base. You can use a branch name or a commit hash.

4. **Initiate rebase**: Execute the command `git rebase -i <base-commit>`. Or you can use the another way `git rebase -i HEAD~<number-of-commits>`. For example, if you want to rebase the last 3 commits, you can use `git rebase -i HEAD~3`.

5. **Interactive editor**: An interactive editor will open, showing a list of commits that will be included in the rebase. You can choose to re-order, squash, edit, or drop commits. Here the example how it look like 
```
pick 68ed932 [FEC-53]-validation-yup
pick 58824a6 [FEC-53]-add-react-hook-form
pick 2704e35 [FEC-53]-update-hook-form
pick 248c188 [FEC-53]-add-formik
pick 07e292b [FEC-53]-update-formik

# Rebase 46ed046..ccb84f0 onto 46ed046 (5 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
"~/WorkSpace/fe-cookbook/.git/rebase-merge/git-rebase-todo" 33L, 1465B
```
6. **Save and exit**: After making the desired changes, save the file and exit the editor. In command line, you can use `:wq` to save and exit.

I know that Vim can be challenging to work in for people who are not familiar with it. In this article, we'll cover some fundamental topics like how to exit Vim, rather than quitting Vim.

I will also include the command that you can use to reveal the corresponding help documentation. To do that, first we need to press ESC a few times, and run the command provided, for example, :h vim-modes, and press Enter.

### How to Exit Vim
Press ESC once (sometimes twice)
Make sure you are using the English input method
The next step depends on the current status and your expectations:
If you didn't make any changes, type :q and press Enter/return
If you made some changes and would like to keep them, type :wq and press Enter/return
If you made some changes and would rather discard them, type :q! and press Enter/return
If you want to understand in more detail how this works, let's dive in.

### Rebase onto a Different Branch

To rebase your current branch onto a different branch:

1. **Open the terminal**: Navigate to your repository using the command line.

2. **Fetch the latest changes**: Run `git fetch` to get the latest updates from the remote repository.

3. **Identify the target branch**: Determine the branch you want to rebase onto.

4. **Initiate rebase**: Execute the command `git rebase <target-branch>`.

5. **Resolve conflicts**: If there are any conflicts, Git will notify you. Resolve the conflicts, stage the changes, and continue the rebase using `git rebase --continue`.

6. **Finish the rebase**: Once all conflicts are resolved, the rebase is complete.

## Common Scenarios

### Rebase vs. Merge

Both rebase and merge are used to integrate changes from one branch into another, but they achieve different results.

- **Rebase**: Moves the branch's commits on top of the latest commit in the target branch, creating a linear history. This makes the branch look as if it was started from the latest state of the target branch.

- **Merge**: Integrates the entire branch's history as a new commit on the target branch. This creates a merge commit, preserving the original branch's commit history.

### Resolving Conflicts during Rebase

During a rebase, if there are conflicting changes between the base and the branch being rebased, Git will pause the process and ask you to resolve the conflicts manually. Once the conflicts are resolved, you can continue the rebase by staging the changes and running `git rebase --continue`.

## Best Practices

- **Backup**: Before performing a rebase, make sure to create a backup or work on a separate branch to avoid losing any important changes.

- **Keep Rebases Local**: Avoid rebasing branches that have already been pushed to a shared repository. Rebase creates a new commit history, which can cause confusion for other team members.

## Conclusion

Rebasing is a valuable tool for maintaining a clean and organized commit history in Git. It allows you to integrate changes effectively and prepare your branches for merging or pull requests. However, it's essential to use rebase responsibly and understand its impact on the commit history.

For more details, check out the [official Git documentation](https://git-scm.com/docs/git-rebase). Happy coding!
