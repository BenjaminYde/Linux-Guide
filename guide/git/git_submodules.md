# Git Submodules

## About 

Git Submodules are a way to include or embed one or more Git repositories as a sub-folder(s) inside another Git repository at a specific path. 
These submodules allow you to keep a Git repository as a subdirectory of another Git repository.

When you add a submodule in Git, you don't add the code of the submodule to the main repository, you only add information about the submodule that is added to the main repository. This information describes which commit the submodule is pointing at. This way, the submodule's code won't automatically be updated if the submodule's repository is updated. This is good, because your code might not work with the latest commit of the submodule, it prevents unexpected behaviour.

Use cases:

- If you have a library that's used in multiple projects, a submodule is perfect to ensure every project gets updated at the same time when the library changes.
- If you have a large project and you want to split up the codebase into smaller, more manageable pieces.
- If you have code that is developed in parallel across multiple repositories but need to include a reference to a particular state of this external code.

See docs about git submodules [here](https://git-scm.com/docs/gitmodules)

## Adding a Submodule

You can add a submodule to your repository using the following command:

```bash
git submodule add -b <branch> <repository> <path>
```

```bash
git submodule add -b main https://github.com/owner/repo.git my-folder
```

This command adds the given Git repository as a submodule, cloned into a directory at the root of your project.

### .gitmodules

First you should notice the new .gitmodules file. This is a configuration file that stores the mapping between the project’s URL and the local subdirectory you’ve pulled it into:

```
[submodule "git_submodules/Python-Guide"]
	path = git_submodules/Python-Guide
	url = git@github.com:BenjaminYde/Python-Guide.git
```

### List Submodules

Use the following command to list all the submodules present in your repository contents:

```bash
git submodule
```

## Cloning a Project with Submodules

When you clone a Git project, submodules won't be included by default. They are not downloaded.     
You need to initialize and update them with these commands:

```bash
git submodule init
git submodule update --recursive
```

- **git submodule init**: This command is used to initialize your submodules. This means that it sets up the necessary Git metadata and configuration files to mark the project directory as a Git repository itself. It prepares the local configuration file (found in .gitmodules), allowing the submodule to be subsequently cloned.

- **git submodule update**: This command fetches the data from those repositories and checks out the appropriate commit (specified by the main repository).
If you haven't initialized the submodules yet, git submodule update won't do anything, because there's no submodule data to fetch and update yet.

Or you can do both with a single command when cloning the repository:

```bash
git clone --recurse-submodules <repository>
```

### Moving a submodule

`git` mv is a convenient command used to move or rename files and directories within a Git repository.

```bash
git mv <path/to/submodule> <path/to/new/submodule>
```

### Make a shallow clone

"Shallow" is a general term that describes a Git repository that doesn't contain the full history of commits. A shallow clone is created using the --depth flag followed by a number that indicates the depth of the clone, i.e., the number of recent commits from each branch that you want to fetch. The term "shallow" doesn't specify how shallow the clone is; it only indicates that you're not getting the full history.

```bash
git submodule add --depth 1 <repository_url> <path>
```

Here, `--depth 1` indicates that you want a shallow clone with only the latest commit from the submodule repository.

The command will perform a shallow clone of the submodule repository, but it will not automatically add the shallow = true line to the .gitmodules file. This means that the shallow state of the submodule will not be preserved when someone else tries to initialize the submodule from the superproject repository.

In `.gitmodules` you will need to **manually** add `shallow = true` to the .gitmodules:

```
[submodule "<submodule_name>"]
    path = <submodule_path>
    url = <repository_url>
    shallow = true
```

## Updating a Submodule To Latest Remote Commit

If you want to update the submodule to the latest commit on a specific branch (like the default branch),        
you can navigate to that submodule directory and run the git pull command, or use the following command:
```bash
git submodule update --remote
```

The `--remote` option tells Git to go into the submodule and pull down the latest changes from that submodule's default branch.

### Select a Specific Branch

When you switch branches or make changes within a submodule, your main repository will recognize this as a change to the submodule.

```bash
cd <submodule_path>
git checkout <branch_name>
```

## Removing a Submodule

Git does not have a built in way to remove submodules... Hopefully this will be resolved in the future, because we now have to do submodule removal manually.

1. Identify the submodule you want to remove from the list of submodules.
2. Use the following command to remove the submodule:
   ```bash
   # Remove the submodule entry from .git/config
   git submodule deinit -f <path_to_submodule>
   ```
   Replace `<path_to_submodule>` with the relative path to the submodule,
3. Use the following command to remove any leftover Git references to the submodule:
   ```bash
   # Remove the submodule directory from the superproject's .git/modules directory
   rm -rf .git/modules/<path_to_submodule>
   ```
4. Use the following command to remove any leftover Git references to the submodule:
   ```bash
   # Remove the entry in .gitmodules and remove the submodule directory located at path/to/submodule
   git rm -f --cached <path_to_submodule>
   ```

## SSH and HTTPS

### Relative URL

Relative URLs in Git submodules are a feature that allows you to specify submodule URLs relative to the URL of the superproject (the main repository containing the submodules). This is particularly useful in scenarios where you want to switch between HTTPS and SSH protocols without having to update the submodule URLs manually. It's also handy when you're working with repository mirrors or moving repositories between different servers.

For example, assume your superproject's repository URL is:

- **SSH**: git@github.com:User/SuperProject.git
- **HTTPS**: https://github.com/User/SuperProject.git

And you have a submodule located at:

- **SSH**: git@github.com:User/SubModule.git
- **HTTPS**: https://github.com/User/SubModule.git
You could specify the submodule URL in .gitmodules like this:

```
[submodule "path/to/submodule"]
    path = path/to/submodule
    url = ../SubModule.git
```

### Switch between SSH and HTTPS

To switch between HTTPS and SSH URLs for Git submodules, especially for CI/CD purposes, you can modify the URL in the .gitmodules file and then synchronize the settings. Here's how to do it:

1. **Edit .gitmodules**: Open your .gitmodules file and change the URL for the submodule from HTTPS to SSH.

From:

```
[submodule "path/to/submodule"]
    path = path/to/submodule
    url = https://github.com/User/SubModule.git
```

To:

```
[submodule "path/to/submodule"]
    path = path/to/submodule
    url = git@github.com:User/SubModule.git
```

2. **Synchronize and Update**:

```bash
git submodule sync  # Syncs the URL in local config
git submodule update --init --recursive  # Fetches the latest content
```

## FAQ

### What Does `<commit-hash>-dirty` Mean for a Git Submodule?

When you see `...-dirty` in the context of a Git submodule, it typically means that there are changes in the submodule directory that have not been committed. These could be modifications to tracked files, or new files that are not yet tracked by Git. The term "dirty" in this context refers to a working directory that is not in a clean state.

To reset a "dirty" submodule and bring it back to a clean state, you can do the following:

**Option 1: Discard the uncommitted changes**

```bash
cd <path_to_submodule>
git reset --hard
git clean -fd
```

**Option 2: Commit the changes**

```bash
cd <path_to_submodule>
git add .
git commit -m "Committing changes in submodule"
git push
```