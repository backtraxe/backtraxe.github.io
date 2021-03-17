# Git Tutorials


Git is the open source distributed version control system.

<!--more-->

## Install Git

Download [Git](https://git-scm.com/downloads) for Windows, MacOS and Linux.

## Configure Tooling

Configure user information for all local repositories.

```bash
# Sets the name you want attached to your commit transactions
git config --global user.name "USERNAME"

# Sets the email you want attached to your commit transactions
git config --global user.email "USERNAME@email.com"
```

## Create Repositories

Start a new repository or obtain one from an existing URL.

```bash
# Creates a new local repository with the specified name
git init [PROJECT-NAME]

# Downloads a project and its entire version history
git clone https://github.com/username/repo-name.git
```

## Make Changes

Review edits and craft a commit transaction.

```bash
# Lists all new or modified files to be commited
git status

# Shows file differences not yet staged
git diff

# Snapshots the file in preparation for versioning
git add FILENAME

# Shows file differences between staging and the last file version
git diff --staged

# Unstages the file, but preserve its contents
git reset FILENAME

# Records file snapshots permanently in version history
git commit -m "DESCRIPTIONS"
```

## Group Changes

Name a series of commits and combine completed efforts.

```bash
# Lists all local branches in the current repository
git branch

# Creates a new branch
git branch BRANCH-NAME

# Switches to the specified branch and updates the working directory
git checkout BRANCH-NAME

# Combines the specified branch’s history into the current branch
git merge BRANCH-NAME

# Deletes the specified branch
git branch -d BRANCH-NAME
```

## Refactor Filenames

Relocate and remove versioned files.

```bash
# Deletes the file from the working directory and stages the deletion
git rm FILENAME

# Removes the file from version control but preserves the file locally
git rm --cached FILENAME

# Changes the file name and prepares it for commit
git mv OLD-FILENAME NEW-FILENAME
```

## Suppress Tracking

Exclude temporary files and paths.

A text file named `.gitignore` suppresses accidental versioning of files and paths matching the specified patterns.

```txt
*.log
build/
temp-*
```

```bash
# Lists all ignored files in this project
git ls-files --other --ignored --exclude-standard
```

## Save Fragments

Shelve and restore incomplete changes.

```bash
# Temporarily stores all modified tracked files
git stash

# Restores the most recently stashed files
git stash pop

# Lists all stashed changesets
git stash list

# Discards the most recently stashed changeset
git stash drop
```

## Review History

Browse and inspect the evolution of project files.

```bash
# Lists version history for the current branch
git log

# Lists version history for a file, including renames
git log --follow FILENAME

# Shows content differences between two branches
git diff BRANCH-1-NAME BRANCH-2-NAME

# Outputs metadata and content changes of the specified commit
git show COMMIT-ID
```

## Redo Commits

Erase mistakes and craft replacement history.

```bash
# Undoes all commits after COMMIT-ID, preserving changes locally
git reset COMMIT-ID

# Discards all history and changes back to the specified commit
git reset --hard COMMIT-ID
```

## Synchronize Changes

Register a repository bookmark and exchange version history.

```bash
# Downloads all history from the repository bookmark
git fetch BOOKMARK

# Combines bookmark’s branch into current local branch
git merge BOOKMARK/BRANCH-NAME

# Uploads all local branch commits to GitHub
git push ALIAS BRANCH-NAME

# Downloads bookmark history and incorporates changes
git pull
```

## References

1. [Git Cheat Sheets](https://training.github.com/)

