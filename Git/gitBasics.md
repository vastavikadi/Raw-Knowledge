----- GIT BASICS -----
- High level commands in git are called porcelain and low level commands are called plumbing.
- Porcelain are the ones most used:
a. git add .
b. git commit -m "message"
c. git push
d. git pull
e. git log

- Plumbing commands are:
a. git apply
b. git commit-tree
c. git hash-object


- A repo is essentially just a directory that contains a project (other directories and files). 
- The only difference is that it also contains a hidden .git directory. That hidden directory is where Git stores all of its internal tracking and versioning information for the project.


----- GIT CONFIG COMMANDS -----
- These commands help to configure details of the users
- These commands work at the global level and also at the repo level

```git config get user.name or user.email``` - helps to get the details

### TO SET THE DETAILS:
a. git config set --global user.name "github_username_here" and --local, which stores the configuration in the current repository only.

b. git config set --global user.email "email@example.com"
> note: Git config keys are case-insensitive, so defaultBranch and defaultbranch work the same. Git just prefers lowercase when displaying them.

### SET A DEFAULT BRANCH
- git config set --global init.defaultBranch master

- git config list --local = Git has a command to view the contents of your config

> NOTE: You can actually store any old data in your Git configuration. Granted, only certain keys are used by Git, but you can store whatever you want. Kuch bhi rkho but jo jruri keys hai wo sahi se rkho. For example, you can store your favorite color in your Git configuration if you want to.

> NOTE: We're using master for now because it is Git's default, but later we'll change it to main, which is GitHub's default. Just bear with us for a second.

- The unset subcommand is used to remove a configuration value. For example:
( it cannot be used to remove the whole section like user or core, but it can be used to remove a specific key like user.name or core.editor)
```git config unset <key>```

- ```cat ~/.gitconfig``` - this is the git config files

```git config set pull.rebase false``` - this command sets the pull.rebase configuration option to false, which means that when you run git pull, Git will use a merge strategy to integrate changes from the remote repository into your local branch. In other words, it will create a new merge commit that combines the changes from the remote branch with your local changes.

## Duplicates
- Typically, in a key/value store, like a Python dictionary, you aren't allowed to have duplicate keys. Strangely enough, Git doesn't care.

## Unset All
- The unset --all command is useful if you ever really want to purge all instances of a key from your configuration. Conversely, the unset subcommand by itself only works with a single instance of a key.
```git config unset --all example.key``` - ek key ke sare instances ko remove krne ke liye (duplicates)


```
git config set --append webflyx.ceo "Warren"
git config set --append webflyx.ceo "Carson"
git config set --append webflyx.ceo "Sarah"

- this append flag is useful if you want to store multiple values for a single key. For example, if you wanted to store the names of all the CEOs of a company over time, you could use the append flag to add each new CEO's name to the list.
```
## Remove sections from git config
- The remove-section subcommand is used to remove an entire section from your Git configuration. For example:

```git config remove-section <section>```

## Config Locations
- There are several locations where Git can be configured. From more general to more specific, they are:

- system: /etc/gitconfig, a file that configures Git for all users on the system
- global: ~/.gitconfig, a file that configures Git for all projects of a user
- local: .git/config, a file that configures Git for a specific project
- worktree: .git/config.worktree, a file that configures Git for part of a project

## Git config overriding order

![Config overriding order](aoJbhjW-980x630.png)

----- GIT FILE STATES -----
- untracked: Not being tracked by Git
- staged: Marked for inclusion in the next commit
- committed: Saved to the repository's history

> NOTE: The git status command shows you the current state of your repo. It will tell you which files are untracked, staged, and committed.

----- git add and git commit ------
- A commit is a snapshot of the repository at a given point in time. It's a way to save the state of the repository, and it's how Git keeps track of changes to the project. A commit comes with a message that describes the changes made in the commit.

----- Git Log -----
- A Git repo is a (potentially very long) list of commits, where each commit represents the full state of the repository at a given point in time.

- The git log command shows a history of the commits in a repository. This is what makes Git a version control system. You can see:

Who made a commit
When the commit was made
What was changed

- A Commit Hash: Each commit has a unique identifier called a "commit hash". This is a long string of characters that uniquely identifies the commit. Here's an example of mine:

5ba786fcc93e8092831c01e71444b9baa2228a4f

> NOTE: For convenience, you can refer to any commit or change within Git by using the first 7 characters of its hash. For mine, that's 5ba786f.

```
git log (assuming the log is long enough) starts an interactive pager. You can scroll through the log with the arrow keys, and exit by pressing q.
```

```
use the -n and --no-pager options to limit the maximum number of commits shown, and more importantly, to run it without the interactive pager. E.g.:

git --no-pager log -n 10
```

```git log <remote>/<branch>``` - this command shows the log of a remote branch. For example, if you want to see the log of the main branch on the origin remote, you would run git log origin/main.

## How git commit hashes are generated
- git commit hashes are generated using the SHA-1 hashing algorithm and hence also referred as SHAs.
- it depends on the contents of the files being committed, the commit message, the author, Parent (previous) commit hashes and the timestamp. This means that even a small change in any of these factors will result in a completely different hash.

## GIT directory
- All the data in a Git repository is stored directly in the (hidden) .git directory. That includes all the commits, branches, tags, and other objects we'll learn about later.

- Git is made up of objects that are stored in the .git/objects directory. A commit is just a type of object.

## Extras
- Luckily, Git has a built-in plumbing command, cat-file, that allows us to see the contents of a commit without needing to futz around with the object files directly.

```git cat-file -p <hash>``` - This command will show you the contents of the commit object, including the tree (the snapshot of the files), the parent commit(s), the author, and the commit message.

## Trees and Blobs

- tree: git's way of storing a directory
- blob: git's way of storing a file

```
Here's what I got when I inspected my last commit:

> git cat-file -p 5ba786fcc93e8092831c01e71444b9baa2228a4f


tree 4e507fdc6d9044ccd8a4a3061324c9f711c4667d
author ThePrimeagen <the.primeagen@aol.com> 1705891256 -0700
committer ThePrimeagen <the.primeagen@aol.com> 1705891256 -0700

A: add contents.md

Notice that we can see:

- The tree object
- The author
- The committer
- The commit message
> However, we cannot see the contents of the contents.md file itself! That's because the blob object stores it.
```

> NOTE: tree objects store the names of the files in a directory, along with the hash of the blob object that contains the contents of each file. This is how Git can reconstruct the state of the repository at any given commit.
> tree me gye (tree ek folder hai) usme boht sare blobs hote hai (blob ek file hai) aur har blob ka ek unique hash hota hai. Ye hash tree me store hota hai. Jab bhi hum commit karte hai to git ye tree aur blobs ko use karke ek snapshot create karta hai. 
> git cat-file -p <blob-hash> - ye command blob (file) ke contents ko show karta hai. 
>

## Storing Data
- Git stores an entire snapshot of files on a per-commit level. No just the changes we do in each file

## Optimization
- While it's true that Git stores entire snapshots, it does have some performance optimizations so that your .git directory doesn't get too unbearably large.

- Git compresses and packs files to store them more efficiently.
- Git deduplicates files that are the same across different commits. If a file doesn't change between commits, Git will only store it once.

## BRANCHES
- git branch allows you to keep track of different changes separately.

- a branch is just a named pointer to a specific commit. When you create a new branch, Git creates a new pointer that points to the current commit. As you make new commits on that branch, the pointer moves forward to point to the new commits.
- The commit that the branch points to is called the tip of the branch.
- Because a branch is just a pointer to a commit, they're lightweight and "cheap" resource-wise to create. When you create 10 branches, you're not creating 10 copies of your project on your hard drive.

```
# Create a new branch named "feature-branch"
git branch feature-branch

# Switch to the newly created branch
git checkout feature-branch

git branch -a (to see all the branches in the repo)

git branch -r (to see all the remote branches in the repo)

git branch -d <branch-name> (to delete a branch)

git branch -D <branch-name> (to force delete a branch) and it can delete a branch even if it has unmerged changes. (capital D is used to force delete a branch, while lowercase d is used to delete a branch that has been fully merged) . Deletes multiple branches at once by specifying them all in the command, separated by spaces. For example: git branch -D branch1 branch2 branch3

git checkout -b <branch-name> (to create and switch to a new branch in one command)

git switch -c <branch-name> (to create and switch to a new branch in one command)

git switch <branch-name> (to switch to an existing branch)

git branch -m <old-branch-name> <new-branch-name> (to rename a branch) ( capital M is used to rename the current branch like git branch -M <new-branch-name>, while lowercase m is used to rename a different branch)

git branch --set-upstream-to=origin/<remote-branch> <local-branch> (to set the upstream branch for a local branch) (mtlb local branch ke commits kis remote branch me push honge, wo set krne ke liye)

git branch --unset-upstream <local-branch> (to unset the upstream branch for a local branch)

git branch --list (to list all branches, including remote branches)

git branch --contains <commit-hash> (to list all branches that contain a specific commit)

git branch --merged (to list all branches that have been merged into the current branch)

git branch --no-merged (to list all branches that have not been merged into the current branch)

git branch --sort=<key> (to sort branches by a specific key, such as name or date)
```


## Git logs
```
The first gflag for git log is --decorate. It can be one of:

short (the default)
full (shows the full ref name)
no (no decoration)

--decorate is now automatically applied as of version 2.12.2. Here are more options you should know:

- Run git log --decorate=full. You should see that instead of just using your branch name, it will show the full ref name. 
- A ref is just a pointer to a commit. All branches are refs, but not all refs are branches.

- Run git log --decorate=no. You should see that the branch names are no longer shown at all.

- The second is --oneline. This flag will show you a more compact view of the log.

git log --oneline

- git log --oneline --graph --all --decorate --color or git log --oneline --graph --all - this command shows the log in a more visual way, with a graph of the commits and their relationships to each other. It also shows all branches and tags, and uses color to make it easier to read.

- git log --oneline --graph --parents - this command shows the log in a more visual way, with a graph of the commits and their relationships to each other. It also shows the parent commits of each commit, which can be useful for understanding the history of a branch.
```

- The "heads" (or "tips") of branches are stored in the .git/refs/heads directory.

-- in the .git/refs/heads directory, each branch is represented by a file that contains the hash of the commit that the branch points to. For example, if you have a branch named "main", there will be a file named "main" in the .git/refs/heads directory that contains the hash of the commit that "main" points to and it is going to be the latest commit hash of the main branch. When you create a new commit on that branch, Git updates the file to point to the new commit hash.


# GIT MERGE
- A merge commit is the result of merging two branches together.
  
- Let's say we start with this:

```
A - B - C    main
   \
    D - E    feature_1
```

- And we merge feature_1 into main by running this while on main:
```
git switch main
git merge feature_1
```
The merge will:

- Find the "merge base" commit, or "best common ancestor" of the two branches. In this case, A.
- Replay the changes from main, starting from the best common ancestor, into a new commit.
- Replay the changes from feature_1 onto main, starting from the best common ancestor.
- Records the result as a new commit, in our case, F.
- F is special because it has two parents, C and E.

```
After:

A - B - C - F    main
   \     /
     D - E        feature_1
```

```git merge <remote>/<branch>``` - this command merges a remote branch into the current branch. For example, if you want to merge the main branch from the origin remote into your current branch, you would run git merge origin/main.

> note: Remember, if you mess up a commit message, you can change it with the --amend flag. For example:

```
git commit --amend -m "F: Merge branch 'add_classics'"
```

- If you prefer VS Code over Vim, run this command to make it your default editor:

```
git config --global core.editor "code --wait"
```
- or if you prefer nano, run this command:

```
git config --global core.editor "nano"
```
### Fast-forward merge

- The simplest type of merge is a fast-forward merge. Let's say we start with this:

```
      C     feature_1
     /
A - B       main
```

- And we run this while on main:

git merge feature_1

- Because feature_1 has all the commits that main has, Git automatically does a fast-forward merge. It just moves the pointer of the "base" branch (here the base branch is main) to the tip of the "feature" branch:

```
            feature_1
A - B - C   main
```

- Notice that with a fast-forward merge, no merge commit is created.

Happy path: keep main behind feature_1, then use Merge while main is checked out. If you add a commit to main first, the branches will diverge and it will stop being a fast-forward merge.


- This is a common workflow when working with Git on a team of developers:

1. Create a branch for a new change
2. Make the change
3. Merge the branch back into main (or whatever branch your team dubs the "default" branch)
4. Remove the branch
5. Repeat

## IMPORTANT: ```git switch -c <branch-name> <commit-hash>``` - this command creates a new branch and switches to it, starting from the specified commit hash. This is useful if you want to create a new branch based on an older commit, rather than the latest commit on the current branch.

> Note: The -c flag is short for --create, and it tells Git to create a new branch. If you omit the -c flag, Git will assume you want to switch to an existing branch.
> Be on the branch that will receive the changes.


## GIT REBASE
- Rebase is a way to integrate changes from one branch into another.
- It works by taking the commits from one branch and "replaying" them on top of another branch. This can be useful for keeping a clean commit history, or for integrating changes from a feature branch into the main branch. ( replaying ka matlb hai ki ek branch ke commits ko dusre branch ke upar apply krna, jaise ki wo commits waha pe hi create hue ho. )
- The difference between merge and rebase is that merge creates a new commit that combines the changes from both branches, while rebase moves the commits from one branch on top of another branch. This can result in a cleaner commit history, but it can also be more complex to manage, especially if there are conflicts between the branches.

![Rebase concept](image.png)

- To use rebase to bring changes from main onto a current branch (let's pretend we're on one called jdsl), we would run this while on the jdsl branch:
- 
```
git rebase main
```

#### This will do the following:

- Identify the latest commit from main and use it as the temporary new base for the rebase process
- Replay each commit from jdsl one at a time onto this temporary location
- Update the jdsl branch to point to the last replayed commit in the temporary location, making this the new permanent jdsl.
- The rebase does not affect the main branch; jdsl now includes all changes from main.

> Note: If you want to rebase a branch onto a specific commit, you can use the commit hash instead of the branch name. For example, if you want to rebase jdsl onto commit abc123, you would run:

```
git rebase abc123
```

> Note: Rebase update_dune onto main means that we want to take the changes from the update_dune branch and apply them on top of the main branch. This is useful if we want to incorporate the latest changes from main into our feature branch before merging it back into main.
- How to do it: Switch to the branch you want to rebase (update_dune) and then run the command git rebase main. This will take the commits from update_dune and replay them on top of the latest commit in main.

> Note: git rebase X rewrites the history of the branch you're currently on, not the branch X that you specify as the base.

- Rebase rewrites the history of the branch you're currently on, not the branch X that you specify as the base. This means that if you rebase a branch onto another branch, the commits from the rebased branch will be replayed on top of the specified base branch, and the original commits from the rebased branch will no longer exist in their original form.

> Note: Warning - Never be on a public branch like main or master and then run git rebase <branch-name>. This will rewrite the history of the public branch and can cause problems for other developers who are working on the same branch. Always create a new branch for your changes and rebase that branch onto the public branch instead.
> Rebase a branch means that branch is going to be the base branch and the other branch is going to be the feature branch. The commits from the feature branch are going to be replayed on top of the base branch. The history of the feature branch is going to be rewritten, but the history of the base branch is not going to be affected. The base branch is going to remain the same, but the feature branch is going to be changed. The commits from the feature branch are going to be applied on top of the base branch, and the feature branch is going to point to the last commit that was applied on top of the base branch.

### Clear your doubts about rebase

<p>
The branch you name in <git rebase feature> is not the branch that gets changed.

The branch you're currently on is the one that gets rewritten.

Let's walk through it carefully.

Suppose we have:

```
A---B---C---D  (main)   ← you're here
         \
          E---F  (feature)
```

- You're on main.

- Now you run:

```git rebase feature```

- Git does not think:

"Copy main onto feature."

- Instead, Git thinks:

- "Take the commits that belong to the current branch (main) but not to feature, and replay them on top of feature."

- Step 1: Find the common ancestor

The common ancestor is C.

```
A---B---C---D  (main)
         \
          E---F  (feature)
```

- Step 2: Find commits unique to the current branch (main)

Only:
D

- Step 3: Temporarily remove D

```
A---B---C      (main temporarily)
         \
          E---F  (feature)
```

Step 4: Move main to point at feature

```
A---B---C
         \
          E---F  (main, feature)
```

Step 5: Replay D

Git creates a new commit D':

```
A---B---C
         \
          E---F---D'  (main)
```

```
Notice what happened:

feature never moved.
main moved.
The old D is abandoned.
A new commit D' is created.

That's why people say rebase rewrites the current branch.
```

```
Compare it to merge

If you were on main and did:

git merge feature

Git would create:

      E---F
     /     \
A---B---C---D---M  (main)

Again, only main changes because you're on main.

Why doesn't feature change?

Because branches are just labels (pointers).
```
</p>

> Note: Ques: It's generally good manners to rebase a public branch onto your own personal branch
> Ans: No. Rebase a public branch means being on that public branch( that public branch is your current branch) and then running git rebase <personal-branch>. this is going to rewrite the history of the public branch and can cause problems for other developers who are working on the same branch. Always create a new branch for your changes and rebase that branch onto the public branch instead.


## GIT RESET


#### git reset soft

- The git reset command can be used to undo the last commit(s) or any changes in the index (staged but not committed changes) and the worktree (unstaged and not committed changes).

```git reset --soft COMMITHASH```
- the commit hash tells git to reset the current branch to the specified commit hash. This means that the current branch will point to the specified commit, and any commits that came after it will be discarded. The --soft option tells git to only reset the commit history, but not the index or worktree.
- The --soft option is useful if you just want to go back to a previous commit, but keep all of your changes. Committed changes will be uncommitted and staged, while uncommitted changes will remain staged or unstaged as before.

Example: if you have commit J after I and want to go back to I, you can use git reset --soft I. This will move the HEAD pointer back to I, but keep all the changes from J staged and ready to be committed again. Same for hard and mixed. The difference is that soft only affects the commit history, while hard and mixed also affect the index and worktree.

Example: 
Suppose titles.md is staged and we use git reset then it will unstage titles.md and it will be in the untracked state. If we use git reset --soft then titles.md will remain staged and it will be in the staged state. Because the --soft option only affects the commit history, not the index or worktree. It moves the HEAD pointer to the specified commit, but does not change the index or worktree. This means that any changes that were staged or unstaged before the reset will remain staged or unstaged after the reset.

### git reset hard
```git reset --hard COMMITHASH```
- here commit hash - tells git to reset the current branch to the specified commit hash. This means that the current branch will point to the specified commit, and any commits that came after it will be discarded. The --hard option tells git to also reset the index and worktree to match the specified commit, discarding any changes in the index or worktree.

- can use HEAD~1, HEAD~2, etc. to refer to the previous commits. HEAD~1 means the commit before the current commit, HEAD~2 means two commits before the current commit, and so on. This is useful if you want to go back to a specific commit without having to look up its hash.

The --hard flag makes your working directory and staging area match the specified commit exactly, discarding any local changes. This is useful if you just want to go back to a previous commit and discard all the changes.

## Git Remote
- A remote is a version of your project that is hosted on the internet or network somewhere. It can be on GitHub, GitLab, Bitbucket, or any other Git hosting service. Remotes are used to collaborate with other developers and to share your code with the world. "remotes," which are just external repos with mostly the same Git history as our local repo.

- in git, another repo is called a "remote." The standard convention is that when you're treating the remote as the "authoritative source of truth" (such as GitHub) you would name it the "origin". By "authoritative source of truth" we mean that it's the one you and your team treat as the "true" repo. It's the one that contains the most up-to-date version of the accepted code.

```git remote add <name> <uri>``` here name is the name you want to give to the remote (like origin), and uri is the URL of the remote repository. This command adds a new remote to your local Git repository, allowing you to fetch and push changes to and from that remote.

TO MAKE IT MORE CLEAR: Suppose we have a local repo and a lot of commits in that, then we make another new empty repo locally, and i want to make old local repo as the remote of the new empty repo, then i can do that by using git remote add <name> <uri> command. This will add the old local repo as a remote to the new empty repo, allowing me to fetch and push changes to and from that remote.

```git remote -v``` - this command shows the URLs of the remote repositories that are associated with your local Git repository. The -v flag stands for "verbose," and it shows both the fetch and push URLs for each remote.

```git ls-remote``` - this command shows the URLs of the remote repositories that are associated with your local Git repository. It lists all the remotes that are configured for your repository, along with their URLs.

```git remote get-url <name>``` - this command shows the URL of the remote repository associated with the specified name. For example, if you have a remote named "origin," you can run git remote get-url origin to see the URL of that remote.

#### git fetch
- this command downloads commits, files, and refs from a remote repository into your local repo. It updates your remote-tracking branches (like origin/main) to reflect the latest changes in the remote repository, but it does not modify your local branches or working directory. This allows you to see what changes are available on the remote without affecting your current work. ( simply, git fetch kia then ye changes ko show krega in git status, .git folder update hota bas but locally files and content abhi download nahi hoga. wo krne ko ```git pull``` use krna padega. )

## Git Push
- The git push command pushes (sends) local changes to any "remote" - in our case, GitHub. For example, to push our local main branch's commits to the remote origin's main branch we would run:

```git push origin main```


#### Alternative Options
- You can also push a local branch to a remote with a different name:

```git push origin <localbranch>:<remotebranch>```


- You can also delete a remote branch by pushing an empty branch to it:

```git push origin :<remotebranch>```


## Git Pull
- Fetching is nice, but most of the time we want the actual file changes from a remote repo, not just the metadata( git fetch bas metadata lata).

```git pull [<remote>/<branch>]```

- The [...] syntax means that the bracketed remote and branch are optional. If you execute git pull without anything specified it will pull your current branch from the remote repo.

## gitignore
- A problem arises when we want to put files in our project's directory, but we don't want to track them with Git. A .gitignore file solves this. 

- Your .gitignore file does not necessarily need to be at the root of your project.
- It's fairly common to have multiple .gitignore files in different directories throughout a project. A nested .gitignore file only applies to the directory it's in and its subdirectories.
- It would be rough if .gitignore files only accepted exact filepath section names. Luckily, they don't!

##### Wildcards for the gitignore file
- The * character matches any number of characters except for a slash (/). For example, to ignore all .txt files, you could use the following pattern:

*.txt

This would ignore files like /princess_diaries.txt and /contacts/your_mom.txt since they're both .txt files.

##### Rooted Patterns
- Patterns starting with a / are anchored to the directory containing the .gitignore file. For example, this would ignore a main.py in the root directory, but not in any subdirectories:

/main.py

This ignores /main.py but not /src/main.py since /src is a subdirectory.

##### Negation
- You can negate a pattern by prefixing it with an exclamation mark (!). For example, to ignore all .txt files except for important.txt, you could use the following pattern:

*.txt
!/important.txt

This would not ignore /important.txt, but would ignore /self_affirmations/important.txt.

##### Comments
- You can add comments to your .gitignore file by starting a line with a #. For example:

# Ignore all .txt files
*.txt

- Comments are especially helpful when doing something unconventional or complex, especially when collaborating.

##### Order Matters
- The order of patterns in a .gitignore file determines their effect, and patterns can override each other. For example:

temp/*
!temp/instructions.md

- Everything in the temp/ directory would be ignored except for instructions.md. If the order were reversed, instructions.md would be ignored.