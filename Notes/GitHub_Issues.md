# Update your local Git repository from GitHub

## 1. Fetching Changes

The `git fetch` command downloads the latest changes from the remote repository without merging them into your local branches.

**Steps:**

- Open your (R studio) terminal or Git Bash.
- Navigate to your local repository directory.
- Execute the following command:
    ```
    git fetch origin
    ```
    - `git fetch`: Retrieves the latest changes.
    - `origin`: The default name for your remote repository (usually your GitHub repo).

## 2. Merging or Rebasing Changes

After fetching the changes, you have two options: merging or rebasing.

### Merging (Recommended but may prompt a Warning)

Merging takes the changes from the remote branch and creates a new commit on your local branch that 
merges them with your uncommitted local work; like a new "merge commit" that combines your local changes with the remote changes.

**Steps:**

- Ensure you are on the branch you want to update (e.g., `main`).
- Execute one of the following commands:
  - `git merge origin/main` (If you have already done a fetch)
  - `git pull origin main` (This combines fetch and merge into one command) - **Note:** This command might show 
      a warning about how to reconcile divergent branches. To be explicit, you can use:  
    - `git pull --no-rebase origin main` (Explicitly performs a merge)
  - Replace `main` with the appropriate branch name if you're working on a different branch.

**Understanding the `git pull` Warning:**

Git often suggests being explicit about how to handle divergent branches when using `git pull` without 
specifying `--rebase` or `--no-rebase`. This is to ensure you understand whether you want to merge (the default) 
or rebase. Using `--no-rebase` explicitly tells Git you want to perform a merge, which is the default behavior if 
no option is specified.

### Rebasing (Making Your Work Look Like It Happened After)

Rebasing takes your local commits and temporarily moves them aside. Then, it updates your local branch with 
the latest changes from the remote. Finally, it puts your commits back on top of those new changes. 
This makes it look like your work happened *after* the remote work.

**Steps:**
-  Ensure you are on the branch you want to update.
-  Execute the following command:
    ```
    git rebase origin/main
    ```
- Replace `main` with the appropriate branch name if needed.

## 3. Resolving Conflicts

If Git detects conflicts (changes to the same lines of code), it will mark the conflicting files.

**Steps:**
- Open the conflicting files in your editor and manually resolve the differences.
- After resolving each conflict, use:
    ```
    git add <conflicted_file>
    ```
- Once all conflicts are resolved, continue the merge or rebase:   
    - For merge: `git commit` (Git will automatically create a merge commit message).
    - For rebase: `git rebase --continue`.

Example Workflow (Using `git pull`)
    - `git fetch origin` (Optional, if you want to see the changes before merging)
    - `git pull origin main` (or the branch you want to update)
    - Resolve any conflicts  

## Notes

- **Branch:** Make sure you're on the correct branch before running `git merge` or `git rebase`.
- **Stash changes:** If you have uncommitted changes, use `git stash` to save them temporarily and `git stash pop` to reapply them later.

## Detours

### Frequently used git commands
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

If you have added files to `.gitignore` but they are still being tracked by git, you can clear the cache for all files in the repository. 
This will remove all files from the staging area, allowing you to re-add them according to your updated `.gitignore` rules.  
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

### You git added a file but want to pull it back from the staging area
```
git restore --staged path/to/file 

git restore --staged . # to unstage all files
```

### Renaming Files Locally and Remotely

This section describes how to rename a file within your GitHub repository, ensuring the change is reflected in both your local and remote repositories.
-  **Rename the File Locally:**  
    - Use the `git mv` command to rename the file. This command handles both the file rename and staging the change for commit.  
    - If the file is in the root directory of your repository, use:  
      ```
      git mv old_filename new_filename
      ```
    - If the file is in a subdirectory, provide the appropriate path:  
        ```
        git mv folder/old_filename folder/new_filename
        ```

-  **Commit and Push the Changes to the Remote Repository:**
    - Commit the file rename with a descriptive commit message:
        ```
        git commit -m "Rename old_filename to new_filename"
        git push origin <branch_name>
        ```
    - If you are on your main branch it is usually:
        ```
        git push origin main
        ```

### Selective File Removal After Commit

To remove specific files from a previous commit, while retaining other files from that same commit, on your remote repository. 
This is useful when you've committed multiple files together but later decide that certain files should not be included in the remote.

**Scenario:**

You committed three files (file1, file2, file3) with a single commit message. However, you now want to remove file3 from the remote repository while keeping file1 and file2.

-  **Identify the Parent Commit:**
    * Use `git log --oneline` to view the commit history.
    * Identify the commit hash of the commit *before* the commit that added the three files.
    * Example Output:
    
        ```
        14a7b96 (HEAD -> main, origin/main) Add file1, file2, file3
        6ce8e82 re-filtering ...
        d9bca5d feature counts ...
        ```
    * In this case, the parent commit hash *before* the three files were committed is `6ce8e82`.

-  **Reset to the Parent Commit:**
    * Reset your local branch to the parent commit:
    
        ```
        git reset <parent_commit_hash>
        ```
        * Example: `git reset 6ce8e82`
    * This removes the commit with all three files from your branch's history and keeps the files in your working directory as unstaged changes.  
    * Note that the remote repository will still reflect the changes you committed.  
    * At this point, if you don't want to add the desired files, you can `git reset` and `git push --force-with-lease origin main`.  Otherwise, proceed to add the desired file(s).

-  **Stage and Commit the Desired Files:**
    * Stage the files you want to keep (file1 and file2):
    
        ```
        git add <file1> <file2>
        ```
    * Commit these files with a new commit message:
    
        ```
        git commit -m "Add file1 and file2 (excluding file3)"
        ```

-  **Delete the Unwanted File Locally:**
    * Since file3 is now untracked, delete it using `rm`. I sometimes add the file types to .gitignore when I don't wish to delete the file.

-  **Force Push the Changes:**
    * Because you've rewritten git history, you need to force push the local branch to the remote
    
        ```
        git push --force-with-lease origin main
        ```
        * **Important:** Force pushing overwrites the remote branch's history. Only use this if you are sure you know what you are doing. 
        If other people are working on the same branch, this can cause significant issues.
        * If using an older version of git: `git push -f origin main`

### Commit a specific file type when it's in the repo's gitignore list

I have the *.png* file type in my `.gitignore` file. But I want to commit a specific *.png* file. In this situation, just add the file as a "negation"
in `.gitignore`.

```!<path/to/png/file/wrt/repoHEAD/filename.png>```

### Reset the staging area to the last commit

```git reset HEAD``` will unstage all changes currently in the staging area.   

This command is also useful for me when I delete a folder from GitHub repo to a non-GitHub folder, and still see the files being tracked
even when the folder/files were not previously committed. It acts as a *refresh* button!    

### Reset after modifying `.gitignore`

When trying to add, commit and push a folder, I noticed using `git status` that a few files were missing because of the way `.gitignore` is set up. 
I modified the `.gitignore` to include the file types (because that's what I wanted to do for this specific repo; not force push the files). 
Modifying `.gitignore` doesn't automatically modify the staging area. You need to `git reset <path/to/file/or/folder>` or `git reset` 
to unstage everything. Then `git add <path/to/file/or/folder>` and `commit` and `push`.   

# Github issues
Issues are a great way to track tasks, discussions, and any feature enhancements in a repository.

[Happy Git and GitHub for the useR](https://happygitwithr.com/)
