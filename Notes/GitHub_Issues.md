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

Example Workflow (Using `git pull`)

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

### Check remote URL path (SSH path)   
```
git remote -v   
```

If remote URL is incorrect, update it using   
```
git remote set-url origin git@github.com:your_username/your_repo_name.git   
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

To fix this situation, stop tracking these files while still keeping them on local machine.  

```
git rm --cached *.docx

git commit -m "stop tracking .docx"  
git push origin main   

```

To remove all ignored files from git's tracking at once, to match with .gitignore:  

```
git rm --cached -r .  
git commit -m "rm previously tracked, now ignored files"  
git push origin main  
```

### Renaming Files Locally and Remotely

This section describes how to rename a file within your GitHub repository, ensuring the change is reflected in both your local and remote repositories.

1.  **Rename the File Locally:**
    * Use the `git mv` command to rename the file. This command handles both the file rename and staging the change for commit.
        ```bash
        git mv old_filename new_filename
        ```
        * If the file is in a subdirectory, provide the appropriate path:
            ```bash
            git mv folder/old_filename folder/new_filename
            ```

2.  **Commit and Push the Changes to the Remote Repository:**
    * Commit the file rename with a descriptive commit message:
        ```bash
        git commit -m "Rename old_filename to new_filename"
        git push origin <branch_name>
        ```
        * If you are on your main branch it is usually:
        ```bash
        git push origin main
        ```

### Selective File Removal After Commit

This section outlines the steps to remove specific files from a previously made commit, while retaining other files from that same commit, on your remote repository. 
This is useful when you've committed multiple files together but later decide that certain files should not be included in the remote.

**Scenario:**

You committed three files (file1, file2, file3) with a single commit message. However, you now want to remove file3 from the remote repository while keeping file1 and file2.

1.  **Identify the Parent Commit:**
    * Use `git log --oneline` to view the commit history.
    * Identify the commit hash of the commit *before* the commit that added the three files.
    * Example Output:
        ```
        14a7b96 (HEAD -> main, origin/main) Add file1, file2, file3
        6ce8e82 re-filtering ...
        d9bca5d feature counts ...
        ```
    * In this case, the parent commit hash *before* the three files were committed is `6ce8e82`.

2.  **Reset to the Parent Commit:**
    * Reset your local branch to the parent commit:
        ```bash
        git reset <parent_commit_hash>
        ```
        * Example: `git reset 6ce8e82`
    * This removes the commit with all three files from your branch's history and keeps the files in your working directory as unstaged changes.  
    * Note that the remote repository will still reflect the changes you committed.  
    * At this point, if you don't want to add the desired files, you can `git reset` and `git push --force-with-lease origin main`.  Otherwise, proceed to add the desired file.

3.  **Stage and Commit the Desired Files:**
    * Stage the files you want to keep (file1 and file2):
        ```bash
        git add <file1> <file2>
        ```
    * Commit these files with a new commit message:
        ```bash
        git commit -m "Add file1 and file2 (excluding file3)"
        ```

4.  **Delete the Unwanted File Locally:**
    * Since file3 is now untracked, delete it using your operating system's commands:
        * Linux/macOS: `rm folder_name/file3`
        * Windows: `del folder_name\file3`
        
        I had added the filetype to .gitignore because I didn;t want to delete the file.

5.  **Force Push the Changes:**
    * Because you've rewritten history, you need to force push your local branch to the remote:
        ```bash
        git push --force-with-lease origin main
        ```
        * **Important:** Force pushing overwrites the remote branch's history. Only use this if you are sure you know what you are doing. 
        If other people are working on the same branch, this can cause significant issues.
        * If using an older version of git: `git push -f origin main`


# Github issues
Issues are a great way to track tasks, discussions, and any feature enhancements in a repository.

[Happy Git and GitHub for the useR](https://happygitwithr.com/)
