# Introduction

## Welcome to Version Control

What is “version control”, and why should you care? Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.This is also called “source control”.If you want to keep every version of your code (which you would most certainly want to), a Version Control System (VCS) is a very wise thing to use. It allows you to revert selected files back to a previous state, revert the entire project back to a previous state, compare changes over time, see who last modified something that might be causing a problem, who introduced an issue and when, and more. Using a VCS also generally means that if you screw things up or lose files, you can easily recover. In addition, you get all this for very little overhead. 

## Why Need Version Control?

- **Backup and Restore**: Files are saved as they are edited, and you can jump to any moment in time. Need that file as it was on Feb 23, 2007? No problem.
- **Synchronization**: Let’s people share files and stay up to datewith the latest version.
- **Short-term undo**: Monkeying with a file and messed it up? Throw away your changes and go back to the “last known good” version in the database.
- **Long-term undo**: Sometimes we mess up bad. Suppose you made a change a year ago, and it had a bug. Jump back to the old version, and see what change was made that day.
- **Track Changes**: As files are updated, you can leave messages explaining why the change happened (stored in the VCS, not the file). This makesit easy to see how a file is evolving over time, and why.
- **Track Ownership**: A VCS tags every change with the name of the person who made it.
- **Sandboxing, or insurance against yourself**: Making a big change? You can make temporary changes in an isolated area, test and work out the kinks before “checking in” your changes.
- **Branching and merging**: A larger sandbox. You can branch a copy of your code into a separate area and modify it in isolation (tracking changes separately). Later, you can merge your work back into the common area.

## What is Git?

By far, the most widely used modern version control system in the world today is Git. Git is a mature, actively maintained open-source project originally developed in 2005 by Linus Torvalds, the famous creator of the Linux operating system kernel. A staggering number of software projects rely on Git for version control, including commercial projects as well as open source.Having a distributed architecture, Git is an example of a DVCS(Distributed Version Control System).

## Why Use Git?

#### Performance

The raw performance characteristics of Git are very strong when compared to many alternatives. Committing new changes, branching, mergingand comparing past versions are all optimized for performance.

Git is not fooled by the names of the files when determining what the storage and version history of the file tree should be, instead, Git focuses on the file content itself. After all, source code files are frequently renamed, split, and rearranged. The object format of Git's repository files uses a combination of delta encoding (storing content differences), compression and explicitly stores directory contents and version metadata objects.

#### Git is a de facto standard

Git is the most broadly adopted tool of its kind. This makes Git attractive for the following reasons. At Atlassian, nearly all of our project source code is managed in Git.
Vast numbers of developers already have Git experience and a significant proportion of college graduates may have experience with only Git. While some organizations may need to climb the learning curve when migrating to Git from another version control system, many of their existing and future developers do not need to be trained on Git.

#### Flexibility

One of Git's key design objectives is flexibility. Git is flexible in several respects: in support for various kinds of nonlinear development workflows, in its efficiency in both small and large projects and in its compatibility with many existing systems and protocols.

#### Git is a quality open-source project

Git is a very well supported open-source project with over a decade of solid stewardship. The project maintainers have shown balanced judgment and a mature approach to meeting the long-term needs of its users with regular releases that improve usability and functionality.

## Best Practices

#### Commit often

Commits are cheap and easy to make. They should be made frequently to capture updates to a code base. Each commit is a snapshot that the codebase can be reverted to if needed. Frequent commits give many opportunities to revert or undo work.

#### Ensure you're working from latest version

SCM enables rapid updates from multiple developers. It’s easy to have a local copy of the codebase fall behind the global copy. Make sure to git fetch the latest code before making updates. This will help avoid conflicts at merge time.

#### Make detailed notes

Each commit has a corresponding log entry. At the time of commit creation, this log entry is populated with a message. It is important to leave descriptive explanatory commit log messages. These commit log messages should explain the “why” and “what” that encompass the commits content. These log messages become the canonical history of the project’s development and leave a trail for future contributors to review.

#### Use Branches

Branching is a powerful SCM mechanism that allows developers to create a separate line of development. Branches should be used frequently as they are quick and inexpensive. Branches enable multiple developers to work in parallel on separate lines of development. These lines of development are generally different product features. When development is complete on a branch it is then merged into the main line of development.

#### Agree on a Workflow

By default,SCMs offer very free form methods of contribution. It is important that teams establish shared patterns of collaboration. SCM workflows establish patterns and processes for merging branches. If a team doesn't agree on a shared workflow it can lead to inefficient communication overhead when it comes time to merge branches.