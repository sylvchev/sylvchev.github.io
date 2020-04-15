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
- View diff with: `git diff` **/!\** It the diff between modified and staged files
- View diff scheduled for commit with: `git diff --cached`
- Ignore files by creating a `.gitignore`
- Commit wih: `git commit`
- Commit by skipping the staging area (aka "commit a la svn"): `git commit -a`
- Remove file with: `git rm file`, to keep the file on HDD but stop the git tracking `git rm --cached file`
- Moving file with: `git mv file`

### Git log 

TODO

### Undoing things 

- Changin last commit: `git commit --amend`
- Unstaging a file with: `git reset HEAD file`
- Unmodifying a file (aka svn revert): `git checkout -- file `

###  Working with remote 

- Show the remote with: `git remote` or `git remote -v`
- Adding a remote repository with: `git remote add [shortname] [url]`

### Tagging

- Show the tag: `git tag`
- Add a new tag: `git tag -a tagname -m "Message"`
- Sharing one tag: `git push origin [tagname]` or all tags:`git push origin --tags`

### Tips and tricks 

- Use bash completion for git : download <https://github.com/git/git/blob/master/contrib/completion/git-completion.bash> and source it : `source ~/git-completion.bash`
- Use global aliases : `git config --global alias.co checkout`, `git config --global alias.ci commit` and `git config --global alias.st status`
- Exporting to zip : `git archive --format zip --output /full/path/to/zipfile.zip master` or `git archive master | zip > myarchive.zip`
- Remove file from git but keep it on the disk : `git rm --cached file1.txt`
- my .gitconfig file is available on [gist](https://gist.github.com/sylvchev/14283580f189e318aabefb566248ad86)


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
- Delete a remote branch: `git push [remotename] :[branch]`

### GitHub

- Clone a GitHub repository on your account
- Clone your remote on local: `git clone https://github.com/username/project.git `
- Add the original repository: `git remote add upstream https://github.com/original/project.git`
- Create a branch to work: `git checkout -b topic`, `git commit`
- Merge with upstream: `git fetch upstream`, `git checkout master`, `git merge upstream/master`

### Rebasing 

- Rebase a branch on another: `git rebase [basebranch] [topicbranch]`, then `git checkout basebranch` and `git merge topicbranch`, finally `git branch -d topicbranch`
- Rebase a PR branch on newly pushed master: `git checkout mybranch`, `git rebase master-upstream`, `git merge`, `git commit`
- Rebase branch C, branched from B (but independent from it), on branch A:  `git rebase --onto A B C`
- **Do not rebase commits that you have pushed to a public repository.**

### Distributed Git 

- Check for whitespace commit: `git diff --check`


### Links 

- [ProGit](http://git-scm.com/book/en/), a complete book, this is a must-read.
- [Git tutorial for scientist](http://nyuccl.org/pages/GitTutorial/), nice figures and very useful
- GitHub help on [how to fork a repo](https://help.github.com/articles/fork-a-repo) and [syncing a fork](https://help.github.com/articles/syncing-a-fork)
- [Git-game v2](https://github.com/git-game/git-game-v2), a game to learn command line git.
- [Become a git guru](https://www.atlassian.com/git/tutorials), another tutorial with nice figures.