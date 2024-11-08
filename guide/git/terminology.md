# Terminology

## Basics A

- **Repository (repo)**:The database storing the files.
- **Server**: The computer storing the repo.
- **Client**: The computer connecting to the repo.
- **Working Set/Working Copy**: Your local directory of files, where you make changes.
- **Trunk/Main/Master**: The primary location for code in the repo. Think of code as a family tree —the trunk is the main line.It is the default branch that git creates for you when first creating a repo. In most cases, "master" means "the main branch". Most shops have everyone pushing to master, and master is considered the definitive view of the repo. But it's also common for release branches to be made off of master for releasing.
- **Revision**: What version a file is on (v1, v2, v3, etc.).
- **Alias**: Defining aliases to serve as substitutes for commands provides two major benefits. It simplifies long commands that have many options, making them shorter and easier to remember.It shortens frequently used commands so that you can work more efficiently.
- **Head**: This is not the latest revision, it's the current revision. Usually, it's the latest revision of the current branch, but it doesn't have to be.
- **Origin**: This is the default aliasto the URL of your remote repository. For example, origin/master refers to the master branch on the remote repository named origin.


## Basics B

- **Fetch/Update/Sync**: Synchronize your files with the latest from the repository. This lets you grab the latest revisions of all files.
- **Show Changelog/History**: Shows alist of changes made to a file since it was created.
- **Switch/Checkout**: Download filesfrom the repoof a specific revision.Add: Put a file into the repo for the first time, i.e.begin tracking it with Version Control.
- **Commit**: Upload a file to the local repository (if it changed). The file gets a new revision.
- **CommitMessage**: A short message describing what was changed.
- **Push**: Upload your commits of the local repository to the online repository. When other people fetch, they will see your changes too.
- **Revert**: Throw away your local changes and reload the latest version from the repository.

## Medium

- **Branch**: Create a separate copy/versionof a repositoryfor private use (bug fixing, new features, releases, ...). Branch is both a verb (“branch the code”) and a noun (“Which branch is it in?”).
- **Diff/Change**: Finding the differences between two files. Useful for seeing what changed between revisions.
- **Merge** (or patch): Apply the changes from one file to another, to bring it up to date. For example, you can merge features from one branch into another.
- **Conflict**: When pending changes to a file contradict each other (both changes cannot be applied).
- **Resolve**: Fixing the changes that contradict each other and checking in the correct version.Pull: 
- **Pull** combines multiple commandsin one. Commit, Fetch, Merge to latest branch. Commit the merged results. It is better to avoid this command and perform these steps yourself. This allows you to review your changesstep by step!
- **Tag**: The ability to tag specific points in a repository’s history as being important. Typically, people use this functionality to mark release points (v1.0, v2.0 and so on).

## Advanced

- **Forcing**: You deliberately want to overwrite the commit history on the remote with your local one. 
- **Locking**: Taking control of a file so nobody else can edit it until you unlock it. Some version control systems use this to avoid conflicts.
- **Breaking the lock**: Forcibly unlocking a file so you can edit it. It may be needed if someone locks a file and goes on vacation (or “calls in sick” the day Halo 3 comes out).
- **Pull request**: In their simplest form, pull requests are a mechanism for a developer to notify team members that they have completed a feature. Once their feature branch is ready, the developer files a pull request. This lets everybody involved know that they need to review the code and merge it into the main branch.
- **Forking**:A fork is a copy of a repository that allows you to freely experiment with changes without affecting the original project. A forked repository differs from a clone in that a connection exists between your fork and the original repository itself. In this way, your fork acts as a bridge between the original repository and your personal copy where you can contribute back to the original project using Pull Requests.
- **Rebase**: This moves the entirefeaturebranch to begin on the tip ofthemasterbranch, effectively incorporating all the new commits inmaster. But, instead of using a merge commit, rebasing rewrites the project history by creating brand new commits for each commit in the original branch.
- **Stash**: Takes your uncommitted changessaves them away for later use. At this point you're free to make changes, create new commits, switch branches, and perform any otheroperations. When you're ready, you can come back and re-apply your stashand work further on your changes.
- **Reset**: A reset is an operation that takes a specified commit and resets the branchto match the state of the repository at that specified commit.
