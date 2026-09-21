# Git & GitHub Notes

Practical Git/GitHub notes for day-to-day work in RStudio and the
terminal. These examples are intentionally situation-based so that I can
remember *why* I used a command, not just the syntax.

> **General rule:** Before a command that resets, removes, cleans,
> rewrites history, or force-pushes, run `git status` and make sure I
> understand what will change.

------------------------------------------------------------------------

## Everyday workflow

### Frequently used commands

``` bash
git status
git add <file1> <file2>
git commit -m "commit message"
git push origin main
```

I usually prefer staging specific files rather than `git add .`,
especially in repositories containing multiple projects.

### Check whether local and remote are in sync

First update my local knowledge of the remote:

``` bash
git fetch origin
git status
```

If Git says:

``` text
Your branch is up to date with 'origin/main'.
```

the committed history of local `main` and `origin/main` is in sync.
Untracked files may still exist locally; they are not part of Git
history until added.

To see commits present on the remote but not locally:

``` bash
git log --oneline HEAD..origin/main
```

To see which files changed remotely:

``` bash
git diff --name-status HEAD..origin/main
```

Common status letters:

``` text
A   added
M   modified
D   deleted
```

To inspect the actual changes:

``` bash
git diff HEAD..origin/main
```

For one file:

``` bash
git diff HEAD..origin/main -- path/to/file
```

------------------------------------------------------------------------

## Updating the local repository from remote changes

### Fetch first when I want to inspect before changing anything

``` bash
git fetch origin
```

`git fetch` downloads information about the latest remote commits but
does **not** modify my working files or merge anything.

This is useful when someone else has pushed changes and I want to see
what changed before pulling.

### Pull remote changes

If I have no local commits that need to be replayed:

``` bash
git pull origin main
```

This fetches and integrates the remote changes.

If I have made and committed local changes and want them replayed on top
of the latest remote work:

``` bash
git pull --rebase origin main
```

A rebase works with **commits**. If I still have uncommitted changes,
Git may refuse to start the rebase. In that case I can commit the work
first or stash it temporarily.

### Merge versus rebase

**Merge**

``` bash
git pull --no-rebase origin main
```

or, after a fetch:

``` bash
git merge origin/main
```

A merge combines divergent committed histories. If both local and remote
contain new commits, Git may create a merge commit.

**Rebase**

``` bash
git pull --rebase origin main
```

or, after a fetch:

``` bash
git rebase origin/main
```

A rebase moves my local commits so that they appear after the current
remote commits, producing a more linear history.

### If I have uncommitted work

One option is to stash it temporarily:

``` bash
git stash
git pull --rebase origin main
git stash pop
```

Useful stash commands:

``` bash
git stash list
git stash drop
git stash clear
```

`git stash clear` deletes all stashes, so use it only when they are no
longer needed.

### Resolve conflicts

If Git reports a conflict:

1.  Open the conflicting file(s) and resolve the marked sections.
2.  Stage each resolved file:

``` bash
git add <conflicted_file>
```

3.  Continue:

For a rebase:

``` bash
git rebase --continue
```

For a merge, after resolving and staging all conflicts:

``` bash
git commit
```

------------------------------------------------------------------------

## Staging and unstaging files

### Stage specific files

``` bash
git add path/to/file1 path/to/file2
```

Staging a specific file does **not** unstage files that were already
staged. Before committing, check:

``` bash
git status
```

Everything listed under **Changes to be committed** will be included in
the commit.

### Unstage one file

``` bash
git restore --staged path/to/file
```

This removes the file from the staging area but keeps the changes in the
working file.

### Unstage an entire folder

Useful when many files in one project folder are checked in the RStudio
Git pane:

``` bash
git restore --staged path/to/project_folder/
```

### Unstage everything

``` bash
git restore --staged .
```

An older equivalent is:

``` bash
git reset HEAD
```

These commands unstage changes; they do not discard the edits in the
working files.

### Discard local edits to a tracked file

If I do **not** want my local changes and want the file restored to the
current committed (`HEAD`) version:

``` bash
git restore path/to/file
```

For all tracked files:

``` bash
git restore .
```

This is different from `git restore --staged`: without `--staged`, the
working-file edits are discarded.

------------------------------------------------------------------------

## `.gitignore` and tracking

### Ignore files or folders anywhere in the repository

Examples:

``` gitignore
*.docx
*.png
**/data/
**/output/
```

`**/data/` and `**/output/` ignore directories with those names at any
depth below the repository root.

### Check why a file is ignored

``` bash
git check-ignore -v path/to/file
```

This reports the `.gitignore` rule responsible for ignoring the file.

### Important: `.gitignore` does not stop tracking files already tracked

If a file was previously added/committed, adding it to `.gitignore` does
not automatically make Git forget it.

To stop tracking one file while keeping the local copy:

``` bash
git rm --cached path/to/file
git commit -m "Stop tracking file"
git push origin main
```

For a file pattern, for example `.docx` files:

``` bash
git rm --cached '*.docx'
```

Quoting the pattern allows Git to evaluate it rather than the shell.

### Reapply `.gitignore` rules to the whole repository

Use this only when I intentionally want Git's tracked set to be rebuilt
according to the current `.gitignore`.

``` bash
git rm -r --cached .
git add .
git status
git commit -m "Apply updated gitignore rules"
git push origin main
```

**Important:** Inspect `git status` before committing. The `git add .`
step is essential here: it re-adds files that are *not* ignored.

### `.gitignore` changed after files were already staged

Changing `.gitignore` does not automatically change what is already in
the staging area.

Unstage first:

``` bash
git restore --staged .
```

Then stage the intended files again.

### Commit a file even though it is ignored

For a one-time exception:

``` bash
git add -f path/to/file
```

Alternatively, create an exception in `.gitignore`:

``` gitignore
*.png
!path/to/file.png
```

Note: negation rules can be affected by ignored parent directories. If
the parent directory itself is excluded, Git may need additional
negation rules for the parent path.

------------------------------------------------------------------------

## Clone a repository onto a new computer

Go to the directory where I keep GitHub repositories:

``` bash
cd ~/github
```

On GitHub, open the repository → **Code** → **SSH**, then copy the SSH
URL:

``` text
git@github.com:<username>/<repo-name>.git
```

Clone:

``` bash
git clone git@github.com:<username>/<repo-name>.git
```

If I want to work with it as an RStudio Project and there is no `.Rproj`
file, create/open an RStudio Project in that directory.

------------------------------------------------------------------------

## Remotes and repository connections

### Check remote URLs

``` bash
git remote -v
```

### Change the `origin` URL

``` bash
git remote set-url origin git@github.com:<username>/<repo-name>.git
```

### Check branch tracking

``` bash
git branch -vv
```

A branch tracking the remote should look approximately like:

``` text
* main 123abc [origin/main] Commit message
```

If `main` is not tracking `origin/main`:

``` bash
git branch --set-upstream-to=origin/main main
```

------------------------------------------------------------------------

## Renaming files, folders, and repositories

### Rename a file

``` bash
git mv old_filename new_filename
git commit -m "Rename file"
git push origin main
```

For a file in a subdirectory:

``` bash
git mv folder/old_filename folder/new_filename
```

### Rename a folder

``` bash
git mv old/folder new/folder
```

Then commit and push.

### Rename a GitHub repository

1.  On GitHub: **Settings → General → Repository name → Rename**.
2.  Update the local remote URL:

``` bash
git remote set-url origin git@github.com:<username>/<new-repo-name>.git
```

3.  Optionally rename the local directory from one level above it:

``` bash
mv <old-repo-name> <new-repo-name>
```

Renaming the local folder is for organization; Git itself does not
require the local directory name to match the remote repository name.

------------------------------------------------------------------------

## Branches and pull requests

In collaborative work, a branch allows development without immediately
changing the stable `main` branch. A pull request (PR) is a GitHub
collaboration/review mechanism for proposing that branch's changes be
merged into another branch.

### Create and switch to a new branch

Modern syntax:

``` bash
git switch -c feature/methods_v2
```

Older equivalent:

``` bash
git checkout -b feature/methods_v2
```

### Commit work on the branch

Prefer staging the intended files explicitly:

``` bash
git add path/to/file1 path/to/file2
git status
git commit -m "Add lasso, ridge, elastic net, and GBM methods"
git push --set-upstream origin feature/methods_v2
```

Then go to GitHub and create a pull request. The PR page can show the
differences from `main`, provide a description, request reviewers, and
support discussion before merging.

**Mental model:** the terminal manages Git history and communicates with
the remote; GitHub provides the hosted repository plus collaboration
features such as PRs, issues, and reviews.

------------------------------------------------------------------------

## One codebase, two remote repositories

Scenario: I have one local codebase and want the same commits available
in two GitHub repositories.

Create the second GitHub repository as an **empty repository**---do not
initialize it with a README, `.gitignore`, or license.

Check the existing remote:

``` bash
git remote -v
```

Example:

``` text
origin  git@github.com:<username>/<repo-name>.git (fetch)
origin  git@github.com:<username>/<repo-name>.git (push)
```

Add a second remote. The name `team` is simply a local nickname for that
remote:

``` bash
git remote add team git@github.com:<organization>/<repo-name>.git
```

Verify:

``` bash
git remote -v
```

Example:

``` text
origin  git@github.com:<username>/<repo-name>.git (fetch)
origin  git@github.com:<username>/<repo-name>.git (push)
team    git@github.com:<organization>/<repo-name>.git (fetch)
team    git@github.com:<organization>/<repo-name>.git (push)
```

First push to the second remote:

``` bash
git push -u team main
```

For later commits:

``` bash
git add <filename>
git commit -m "commit message"

git push origin main
git push team main
```

The same local commit can therefore be pushed to both repositories.

------------------------------------------------------------------------

## Troubleshooting and recovery

### Local branch is ahead of `origin/main`

See the local commits that have not been pushed:

``` bash
git log --oneline origin/main..HEAD
```

If I intentionally want to combine/rework recent *unpushed* commits, I
can reset them. For example, to undo the last four commits while keeping
their changes staged:

``` bash
git reset --soft HEAD~4
```

Use this only after confirming those are the commits I intend to
rewrite.

### Accidentally committed many staged files --- not pushed yet

This can happen because files were already checked/staged in the RStudio
Git pane before I ran `git add` for the one file I intended to commit.

Undo the most recent commit while keeping the file changes:

``` bash
git reset HEAD~1
```

Then inspect:

``` bash
git status
```

Stage only the intended files and recommit.

### Stage modified and deleted tracked files, but not untracked files

``` bash
git add -u
```

Then:

``` bash
git commit -m "Update tracked files"
git push origin main
```

### RStudio can commit but not push

Check repository state:

``` bash
git status
```

Check the remote URL:

``` bash
git remote -v
```

Check branch tracking:

``` bash
git branch -vv
```

If needed:

``` bash
git branch --set-upstream-to=origin/main main
```

RStudio's Git pane can occasionally need refreshing even when Git itself
is fine.

------------------------------------------------------------------------

## Destructive or history-rewriting operations --- use carefully

These commands are useful, but they can discard work or alter shared
history. Run `git status` first and inspect exactly what will be
affected.

### Make local tracked files exactly match the remote

Use this only when I explicitly **do not want to preserve local tracked
changes**:

``` bash
git fetch origin
git reset --hard origin/main
```

`git reset --hard` discards local changes to tracked files.

If I also want to remove untracked files/directories, preview first:

``` bash
git clean -fdn
```

Only after checking the preview:

``` bash
git clean -fd
```

`git clean -fd` permanently removes untracked files and directories.

### Remove a file from the current remote repository after it was committed

If the file is not sensitive and I simply no longer want it in the
current version of the repository, there is usually **no need to rewrite
history**:

``` bash
git rm path/to/file
git commit -m "Remove file"
git push origin main
```

The file still exists in earlier Git history.

### Remove a file from an earlier commit by rewriting history

History rewriting is a different situation---for example, when I
specifically need to alter existing commits rather than simply remove a
file going forward.

A previous workflow I used was:

``` bash
git log --oneline
git reset <parent_commit_hash>
```

Then stage only the files that should remain:

``` bash
git add <file1> <file2>
git commit -m "Add intended files"
```

If the rewritten history had already been pushed:

``` bash
git push --force-with-lease origin main
```

**Warning:** `--force-with-lease` rewrites remote branch history. Avoid
doing this casually on a shared branch. Coordinate with collaborators
first.

If a committed file contained credentials, secrets, protected data, or
other information that must actually be removed from repository history,
simply deleting it in a new commit is not sufficient. That requires
deliberate history cleanup and, for exposed credentials, credential
rotation.

------------------------------------------------------------------------

## Move a subfolder into its own repository while retaining history

Example structure:

``` text
github/
└── Rshiny/
    └── Vennify/
```

Goal: make `Vennify` its own repository while retaining the relevant
commit history.

Work on a separate clone rather than the only working copy:

``` bash
git clone https://github.com/<username>/Rshiny.git
```

Install `git-filter-repo` if needed:

``` bash
brew install git-filter-repo
```

or:

``` bash
python3 -m pip install --user git-filter-repo
```

Inside the cloned repository:

``` bash
git filter-repo --subdirectory-filter Vennify
```

If the tool requires it for this disposable clone, use `--force` only
after confirming I am working in the intended copy.

Rename the local directory if desired, then configure the new remote:

``` bash
git remote remove origin
git remote add origin https://github.com/<username>/Vennify.git
git push -u origin main
```

------------------------------------------------------------------------

## GitHub Issues

GitHub Issues are useful for tracking tasks, bugs, discussions, feature
ideas, and future enhancements associated with a repository.

------------------------------------------------------------------------

## Reference

[Happy Git and GitHub for the useR](https://happygitwithr.com/)
