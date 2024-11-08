# CLI

### git init

`git init` is used to initialize a new Git repository. Running this command in a new or empty directory creates a new Git repository with a default `master` branch.

```sh
git init <directory-name>
```

#### --initial-branch
tools/ros_opcua_bridge/resources/UaExpert-1.6.0-414-x86_64.AppImage
Use the specified name for the initial branch in the newly created repository. If not specified, fall back to the default name `main`.

### git clone

`git clone` is used to create a local copy of a remote Git repository. This command creates a copy of the entire repository, including its history, in the specified directory.

```sh
git clone <repo-url>
```

#### --branch
Specifes the branch of the remote repository that should be checked out by default after the clone operation completes.

#### --no-checkout
create a clone of a remote repository without checking out any files. This can be useful when you only want to clone the repository's history and not its working files.

#### --bare
Create a clone of a remote repository that is a bare repository, which means it only contains the Git repository data and does not have a working directory.

#### --mirror
Create a full copy of a remote repository, including all branches, tags, and commit history. This is useful when you want to create a complete backup of a repository or when you want to create a new repository that is a mirror image of an existing repository.

### git fetch

`git fetch` is used to fetch updates from a remote repository without merging them into the current branch. This command updates the remote-tracking branches with the latest changes from the remote repository.

When no remote is specified, by default the `origin` remote will be used.

```sh
git fetch <remote-name>
```

#### --all
Fetch all remotes.

#### --no-tags
By default, tags that point at objects that are downloaded from the remote repository are fetched and stored locally. This option disables this automatic tag following.

### git ls-remote

When you run the `git ls-remote` command, Git will establish a connection to the remote repository and retrieve a list of references along with their associated commit IDs. This information is useful for a variety of purposes, such as checking which branches and tags exist in the remote repository, verifying that a specific commit has been pushed to the remote repository, or fetching information about the latest commits from a remote repository.

The following command will list all references, including branches, tags, and other Git objects.

```sh
git ls-remote
```

```sh
9b18d0c7a47a4016d52d3db12c568b33c6b70e6f HEAD 9b18d0c7a47a4016d52d3db12c568b33c6b70e6f refs/heads/main cc478b2cc10f96e4d2a8e14a57b7f184bcb456e4 refs/tags/v1.0.0 ...
```

#### -h or --heads
This option displays only the references that are branch heads (i.e., the tip of the branch)

```sh
git ls-remote -h
```

```sh
9b18d0c7a47a4016d52d3db12c568b33c6b70e6f    refs/heads/main
```

The following command will only list the branch names:

```sh
git ls-remote --heads | sed 's|.*refs/heads/||' 
```

### git checkout

`git checkout` is used to switch to a different branch or commit. This command updates the working directory to match the specified branch or commit.

```sh
git checkout <branch-name>
```

If the target branch is ahead of the current branch, the `git checkout` command will perform a fast-forward merge, which means that Git will simply move the branch pointer forward to the latest commit in the target branch. Local modifications to the files in the working tree are kept.

If the current branch has commits that are not present in the target branch, Git will create a new merge commit to combine the changes from both branches. This will create a new commit that has two or more parents, representing the merge of the two branches.

#### --force
Forcefully switch to a new branch or commit, even if there are uncommitted changes in the working directory. This can be useful in some cases, but it can also cause data loss if not used carefully.

```sh
git checkout --force <branch-name>
```

#### -b
This option will create a new branch and switch your current branch to the new branch. The following command will create a new branch on the current commit and switch to it in a single step:

```sh
git checkout -b <new-branch>
```

The following command will create a new branch based on an existing branch, you can specify the existing branch name as a second argument:

```sh
git checkout -b <new-branch> <existing-branch>
```

#### --merge
This option will switch to the target branch and perform a merge with the other branch in a single step. It's worth noting that the `git merge` command is generally preferred for merging branches, as it provides more control and options for resolving conflicts.

### git switch

`git switch` is a Git command that allows you to switch between branches or to create a new branch and switch to it. It is the preferred command for branch switching since Git version 2.23, replacing the old `git checkout` command.

```sh
git switch <branch-name>
```

Where `<branch>` is the name of the branch you want to switch to.

If the branch you want to switch to does not exist yet, you can create it and switch to it by using the `-c` or `--create` option:

```sh
git switch -c <new-branch-name>
```

Alternatively, you can create a new branch from a specific commit by providing the commit hash or a branch name as a reference point:

```sh
git switch -c <new-branch-name> <commit-hash/branch-name>
```

When you run `git switch`, Git will update your working directory to reflect the contents of the new branch or commit. If you have uncommitted changes in your current branch, Git will prompt you to either stash them or commit them before switching to the new branch.

#### --force
Forcefully switch to a new branch or commit, even if there are uncommitted changes in the working directory. This can be useful in some cases, but it can also cause data loss if not used carefully.

```sh
git switch --force <branch-name>
```

### git status

`git status` is used to show the current status of the repository. This command shows which files are untracked, which files have been modified, which changes have been staged for commit, and which changes are waiting to be pushed.

```sh
git status
```

### git add

`git add` is used to add changes to the staging area, preparing them to be committed. This command stages changes made to files in the working directory.

```sh
git add <file-name>
```

### git restore

`git restore` is used to restore files to a previous state. This command restores the specified file to the version that was last committed.

```sh
git restore <file-name>
```

### git clean

`git clean` is used to remove untracked files from the working directory. This command removes any files that are not currently under version control and have not been added to the staging area.

```sh
git clean
```

### git commit

`git commit` is used to save changes to the local repository with a commit message. This command creates a new commit that includes any changes that have been staged for commit.

```sh
git commit -m "<my-message>"
```

#### -a
This will add all changes to the staging area and create a new commit with the specified commit message. The `-a` option stands for "all" and tells Git to stage all changes, including modifications and deletions, but not untracked files.

#### --amend
This will amend the most recent commit with any new changes you've made and change the commit message to the new message you've provided. This is useful if you forgot to include something in the previous commit or if you made a mistake in the commit message.

#### -n / --no-verify
By default, the pre-commit and commit-msg hooks are run. When any of `--no-verify` or `-n` is given, these are bypassed.

### git push

`git push` is used to push committed changes to a remote repository. This command uploads the committed changes to the specified remote repository.

```sh
git push
```

#### pushing changes to a remote branch for the first time

-u / --set-upsteam : add an upstream (tracking) reference

```sh
git push -u origin <branch-name>
```

#### force pushing

```sh
git push --force orign <branch-name>
```

#### push a mirror of the repo

```sh
git push --mirror <repo-url>
```

### git pull

`git pull` is used to fetch and merge changes from a remote repository to the local repository. This command fetches the changes from the specified remote repository and merges them into the current branch.

It is essentially a combination of two other Git commands, `git fetch` and `git merge`. Its a convenient shortcut for updating your local repository with changes from a remote repository

```sh
git pull
```

### git branch

`git branch` is used to list all branches in the repository and create a new branch. This command shows a list of all branches in the repository and highlights the current branch.

```sh
git branch <branch-name>
```

#### -d
:Deletes the branch with the given name. This operation will fail if the branch has unmerged changes.

#### -r / --remotes
Lists the names of remote branches that your local Git repository knows about. These branches are cached locally after you clone the remote repository. This command is useful for quickly seeing what remote branches are available without connecting to the remote repository.

#### -a / --all
Lists all local and remote branches in your repository.

#### -c
Creates a new branch based on the current branch and switches to it.

#### --heads

### git merge

`git merge` is used to merge changes from one branch into another. This command merges the changes from the specified branch into the current branch.

```sh
git merge <branch-name>
```

For example, if you want to merge changes from the `feature-branch` into the `main` branch, you can run:

```sh
git checkout main
git merge feature-branch
```

#### --abort
Used to abort the merge process if there are conflicts that cannot be resolved. This will revert the working tree to the state before the merge

```sh
git merge feature-branch
# Resolve conflicts
git merge --abort
```

### git stash

> [!CAUTION]
> TODO

### git log

`git log` is used to show a history of commits in the repository. This command shows a list of all commits, including their commit messages, timestamps, and authors.

```sh
git log
```

By default, running `git log` will display a list of all the commits in the repository, starting with the most recent. Each commit is displayed with its hash, author, date, and commit message.

#### --oneline
Displays each commit on a single line, with just the first few characters of the commit hash and the commit message

#### --graph
: Displays the commit history as a graph, with branches and merges shown visually.

#### -- author

```sh
git log --author="John Doe"
```

#### --since

```sh
git log --since="2 weeks ago"
```

#### --pretty
With this option, you can specify your own output format using placeholders for various commit fields.

```sh
git log --pretty=format:"<format string>"
```

In the format string, you can use placeholders to specify which fields you want to include in the output and how you want to format them. For example, `%h` is a placeholder for the abbreviated commit hash, `%an` is a placeholder for the author name, and `%s` is a placeholder for the commit subject.

```sh
git log --pretty=format:"%h %an %ad %s"

7b55b1a John Doe Tue Feb 23 15:23:11 2021 Update README.md
4d3f9e9 Jane Smith Mon Feb 22 10:15:27 2021 Add LICENSE file
```

### git remote

`git remote` is used to manage remote repositories. This command shows a list of all remote repositories that are associated with the current repository.

```sh
git remote
```

#### -v

be verbose

> [!CAUTION]
> TODO


### git tag

`git tag` is used to create, list, and delete tags. This command is used to tag specific commits in the repository.

```sh
git tag <tag-name>
```

### git grep

`git grep` is used to search for a specific string in the repository. This command searches for the specified string in all files in the repository.

```sh
git grep <seach-term>
```

### git revert

> [!CAUTION]
> TODO

### git reset

`git reset` is used to unstage changes and reset the repository to a previous state. This command resets the repository to the specified commit and removes any changes that have been staged for commit.

```sh
git reset <commit-hash>
```

