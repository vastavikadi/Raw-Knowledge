## Fork
- A fork is a copy of a repo (linked to the original via some metadata).
- Forking a repo allows you to freely experiment with changes without affecting the original project.

- No command for forking a repo. Forking is not a Git operation, but it is a feature offered by many Git hosting services such as GitHub, GitLab, and Bitbucket.

> Note: Forking a repo is different from cloning a repo. Cloning creates a local copy of the repo on your machine, while forking creates a copy of the repo on the hosting service.

### How to disable rerere
```git config set --local rerere.enabled false``` - this command disables the rerere feature for the current repository. It prevents Git from automatically resolving conflicts based on previous resolutions.

```rm -rf .git/rr-cache``` - This command removes the rerere cache directory, which stores information about previously resolved conflicts. By deleting this directory, you effectively clear the history of conflict resolutions, preventing Git from using that information in future merges.

### Standard Open Source COntribution Steps
- Fork their repo into your account
- Clone your fork to your local machine
- Create a new branch (let's call it your_feature)
- Make changes
- Commit and push changes to your fork's remote your_feature branch
- Create a pull request to original_owner/repo main from your_username/repo your_feature

> Always add a second upstream remote to your forked repo. This allows you to fetch and merge changes from the original repository into your fork, keeping it up-to-date with the latest changes. Use the following command to add the upstream remote:

```bash
git remote add upstream <original_owner_repo_url>
```


> Then the original owner can review your changes. If they like them, they can merge the changes straight from your fork into their repository.


## Head
- Branches are references to commits, and HEAD is a reference to the branch you're currently on. Branches reference latest commits, and HEAD references the latest commit on the current branch. When you switch branches, HEAD updates to point to the latest commit on that branch.

```cat .git/HEAD``` - This command displays the current HEAD reference, which shows the branch you're currently on and the latest commit associated with that branch. Output will look like this: `ref: refs/heads/main`, indicating that HEAD is pointing to the latest commit on the `main` branch. Now, cat the latest commit hash using the following command:

```cat .git/refs/heads/main``` - This command displays the latest commit hash for the `main` branch, which is the commit that HEAD is currently pointing to. The output will be a long string of characters representing the unique identifier of the latest commit on the `main` branch.

## Reflog
- Reflog is a mechanism in Git that records updates to the tip of branches and other references. It allows you to view the history of changes made to your branches, including commits, merges, and resets. Reflog is particularly useful for recovering lost commits or undoing changes.

##### How to Use
- Suppose you have a branch called `feature` and you did a commit on it, but then you accidentally reset the branch to a previous commit and deleted the branch and switched back to the `main` branch. You can use reflog to find the commit hash of the lost commit and recover it.
- To view the reflog for the `feature` branch, you can use the following command:

```bash
git reflog feature
```
```
Then you will find the commit hash of the lost commit in the reflog output. You can then use the following command to create a new branch from that commit:
```bash
git checkout -b recovered_feature <commit_hash> - This command creates a new branch called `recovered_feature` starting from the lost commit, allowing you to recover your changes and continue working on them.
```

- ```git merge <commit_hash>``` - This command merges the lost commit into your current branch, allowing you to recover your changes without creating a new branch. This way, you can use ```git reflog``` to find the lost commit and merge it back into your current branch, effectively restoring your changes.

```
Instead of this whole big process:
git reflog # find the commit sha at HEAD@{1}
git cat-file -p <commit sha>
git cat-file -p <tree sha>
git cat-file -p <blob sha> > slander.md
git add .
git commit -m "B: recovery"
```

- You can use the following command to view the reflog for all branches:

```bash
git reflog
```

## GIT MERGE
- The git merge command can be used to integrate changes from different branches or commits. It allows you to merge a "commitish" into your current branch. A "commitish" is an object that resembles a commit, such as a branch name, tag, specific commit SHA, or a reflog entry like HEAD@{1}.

