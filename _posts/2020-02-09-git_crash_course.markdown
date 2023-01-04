---
layout: post
title: Crash course on git
date: 2020-02-09 13:00:00
description: Survival kit
---

### Git Basic

- Initializing a repository in an existing directory: `git init`
- Cloning a repository: `git clone git://server/project`

For Windows user :
- <https://git-cola.github.io/> for GUI, or <https://www.gitkraken.com/git-client> for a non-free alternative
- <https://git-for-windows.github.io/> for `git-bash`, the command line interface
 

### Repository

- Two states for files: __tracked__ or __untracked__
- Tracked files can be __unmodified__, __modified__, or __staged__. 
- Know the state with: `git status`
- Add new file or staged modified files: `git add file`
- View diff with: `git diff` :warning: It shows the diff between modified and staged files
- View diff scheduled for commit with: `git diff --cached`
- Ignore files by creating a `.gitignore`
- Commit wih: `git commit`
- Commit by skipping the staging area (aka "commit a la svn"): `git commit -a`
- Remove file with: `git rm file`, to keep the file on HDD but stop the git tracking `git rm --cached file`
- Moving file with: `git mv file`

### Git log 

- Tree view of your repro: `git log --graph --date-order`, or add in your `~/.gitconfig` an alias section with
  [alias]
     lgr = log --graph --date-order --pretty=format:'%Cblue%h %Cgreen%ci %Cred%an, %Cblue%m %Creset%s %Cred%d'
- Display diff with from 2 commits ago: `git diff HEAD~2 HEAD` or `git diff @~2` as HEAD is implicit
- List commit for a specific file: `git log --abbrev-commit myfile`
- Coming back in time : `git checkout commit-hash`, detaching HEAD. You could come back to the topmost commit of your branch with `git switch -`

### Undoing things 

- Changin last commit: `git commit --amend`
- Unstaging a file with: `git reset HEAD file`
- Unmodifying a file (aka svn revert): `git checkout -- file `

###  Working with remote 

- Show the remote with: `git remote` or `git remote -v`
- Adding a remote repository with: `git remote add [shortname] [url]`

### Tagging

- Show existing tags: `git tag`
- Add a new tag: `git tag -a tagname -m "Message"`
- Sharing one tag: `git push origin [tagname]` or all tags: `git push origin --tags`
- Get tags from remote: `git fetch --all --tags --prune` and checkout to the desired tags: `git checkout tags/<tag_name> -b <branch_name>`


### Branching and merging 

- Create a branch: `git branch [branchname]`
- Switch to branch: `git checkout [branchname]`
- Create and switch with `git checkout -b [branchname]`
- Delete branch: `git branch -d [branchname]`
- Merge with branch: `git merge [branchname]`, solve conflict with `git mergetool`

### Remote branches 

- Remote branches are readonly and denoted (remote)/(branch) instead of (branch) for local branches.
- Synchronize with remote: `git fetch (remote)`, e.g. `git fetch origin`
- Push the branch on remote: `git push (remote) (branch)`, or `git push (remote) (localBranch:remoteBranch)`
- After a `git fetch origin` new branches are available that can be merge into current branch with `git merge origin/(branch)`, it is also possible to start a local branch with the same name `git checkout -b (branch) (remote)/(branch)` or `git checkout --track (remote)/(branch)`
- If you forgot to track a remote branch on your local branch, you could use `git branch -u remote/branch` to follow the remote branch.
- Delete a remote branch: `git push [remotename] :[branch]`

### GitHub

- Clone a GitHub repository on your account
- Clone your remote on local: `git clone https://github.com/username/project.git `
- Add the original repository: `git remote add upstream https://github.com/original/project.git`
- Create a branch to work: `git checkout -b topic`, `git commit`
- Merge with upstream: `git fetch upstream`, `git checkout master`, `git merge upstream/master`

### Rebasing 

- Rebase a branch on another: `git rebase [basebranch] [topicbranch]`, then `git checkout basebranch` and `git merge topicbranch`, finally `git branch -d topicbranch`
- Rebase a PR branch on newly pushed master: `git checkout mybranch`, `git rebase master-upstream`, `git merge`, `git commit`, or more simply `git checkout mybranch`, `git rebase upstream/master`,` git push -f origin mybranch`
- Rebase branch C, branched from B (but independent from it), on branch A:  `git rebase --onto A B C`
- **Do not rebase commits that you have pushed to a public repository if your are on a public branch**


### Tips and tricks 

- Use bash completion for git : use zsh, or else download <https://github.com/git/git/blob/master/contrib/completion/git-completion.bash> and source it : `source ~/git-completion.bash`.
- Use global aliases : `git config --global alias.co checkout`, `git config --global alias.ci commit` and `git config --global alias.st status`
- Exporting to zip : `git archive --format zip --output /full/path/to/zipfile.zip master` or `git archive master | zip > myarchive.zip`
- Remove file from git but keep it on the disk : `git rm --cached file1.txt`
- my .gitconfig file is available on [gist](https://gist.github.com/sylvchev/14283580f189e318aabefb566248ad86)
- To store credential in https: `git config credential.helper store`, then your command, like `git pull`, and `git config --global credential.helper 'cache --timeout=6400'`
- Check for whitespace commit: `git diff --check`


### Links 

- [ProGit](http://git-scm.com/book/en/), a complete book, this is a must-read.
- [Learn Git Branching](https://learngitbranching.js.org/), an incredible tutorial, with realistic environment in your browser!
- [Git tutorial for scientist](http://nyuccl.org/pages/GitTutorial/), nice figures and very useful
- GitHub help on [how to fork a repo](https://help.github.com/articles/fork-a-repo) and [syncing a fork](https://help.github.com/articles/syncing-a-fork)
- [Git-game v2](https://github.com/git-game/git-game-v2), a game to learn command line git.
- [Become a git guru](https://www.atlassian.com/git/tutorials), another tutorial with nice figures.