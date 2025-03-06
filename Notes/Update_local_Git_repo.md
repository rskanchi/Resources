# Update your local Git repository from GitHub

This document outlines the process of updating your local Git repository to reflect changes made on a remote GitHub repository.

## 1. Fetching Changes

The `git fetch` command downloads the latest changes from the remote repository without merging them into your local branches.

**Steps:**

1.  Open your (R studio) terminal or Git Bash.
2.  Navigate to your local repository directory.
3.  Execute the following command:

    ```bash
    git fetch origin
    ```

    * `git fetch`: Retrieves the latest changes.
    * `origin`: The default name for your remote repository (usually your GitHub repo).

## 2. Merging or Rebasing Changes

After fetching the changes, you have two options: merging or rebasing.

### Merging (Recommended for Most Cases)

Merging creates a new "merge commit" that combines your local changes with the remote changes.

**Steps:**

1.  Ensure you are on the branch you want to update (e.g., `main` or `master`).
2.  Execute one of the following commands:

    * `git merge origin/main` (If you have already done a fetch)
    * `git pull origin main` (This combines fetch and merge into one command)

    * Replace `main` with the appropriate branch name if needed.

### Rebasing (For a Cleaner History)

Rebasing rewrites your local branch's history by placing your commits on top of the latest remote commits. Use this if you want a linear history and understand the potential risks.

**Steps:**

1.  Ensure you are on the branch you want to update.
2.  Execute the following command:

    ```bash
    git rebase origin/main
    ```

    * Replace `main` with the appropriate branch name if needed.

## 3. Resolving Conflicts

If Git detects conflicts (changes to the same lines of code), it will mark the conflicting files.

**Steps:**

1.  Open the conflicting files in your editor and manually resolve the differences.
2.  After resolving each conflict, use:

    ```bash
    git add <conflicted_file>
    ```

3.  Once all conflicts are resolved, continue the merge or rebase:

    * For merge: `git commit` (Git will automatically create a merge commit message).
    * For rebase: `git rebase --continue`.

## Example Workflow (Using `git pull`)

1.  `git fetch origin` (Optional, if you want to see the changes before merging)
2.  `git pull origin main` (or the branch you want to update)
3.  Resolve any conflicts.

## Notes

* **Branch:** Make sure you're on the correct branch before running `git merge` or `git rebase`.
* **Stash changes:** If you have uncommitted changes, use `git stash` to save them temporarily and `git stash pop` to reapply them later.


## Detours

### git commands
```
git add xx
git commit -m "comment"
git push origin main
```

### Clear git cache for tracked files
```
git rm -r --cached .
```

### Your branch is ahead of 'origin/main' by n commits
Check local commits using ```git log origin/main..HEAD``` 
Reset last few commits, say 4 using ```git reset --soft HEAD~4```

Remove deleted files from tracking ```git add -u```. It stages only modified and deleted files.  
git commit -m "remove D files from tracking". This saves the change.  
git push origin main

Suppose you added .docx to .gitignore file. If they are still showing in the staging area, then they are being tracked by git; likely because they were committed at least once before the .docx was added to .gitignore.  

To fix this situation:  
```git rm --cached *.docx``` to stop tracking these files while still keeping them on local machine.

git commit -m "stop tracking .docx"
git push origin main

To remove all ignored files from git's tracking at once, to match with .gitignore:
git rm --cached -r .
git commit -m "rm previously tracked, now ignored files"
git push origin main

With the above, all files from the remote repo may be removed?

# Github issues
Issues are a great way to track tasks, discussions, and any feature enhancements in a repository.


