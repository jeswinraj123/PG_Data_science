# GIT commands

### Set up ssh and copy ssh public key to github

On your local machine:

`ssh-keygen -t rsa -b 4096`

This will generate files in:
C:\Users\username\.ssh

Copy the content of file id_rsa.pub into clipboard.

Go to github -> Settings -> ssh keys -> add new ssh key -> paste the clipboard -> save

### Clone repo to your local machine

Go to Github -> repositories -> click on the repo -> Code -> HTTPS or ssh -> copy the url

On your machine:

```git
git clone https://github.com/manojkmgit/myapp2.git
#or
git clone git@github.com:manojkmgit/myapp2.git
```

### How to connect a local repo to a remote Git repo

Go to Github and create a new repo. Don't create any additonal files.
Take note of the repo url in https or ssh format.
Say it is:

git@github.com:manojkmgit/myapp2.git

From your local machine, you have 2 options to use this newly created repo. Either you initialize a new local repo now or you push an already initialized local repo to this new remote repo.

#### Option 1: Create a new repository on the command line

```
echo "# myapp2" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:manojkmgit/myapp2.git
git push -u origin main
```

#### Option 2: Push an existing repository from the command line

```
git remote add origin git@github.com:manojkmgit/myapp2.git
git branch -M main
git push -u origin main
```

If you ever need to merge unrelated histories

```
git pull origin main --allow-unrelated-histories
```

### Basic commands

Check current branch

```
git branch
```

Fetch branches/tags from one or more repos along with history etc.
It will fetch origin remote.

```
git fetch
```

It will fetch all remotes.

```
git fetch --all
```

It will fetch a particular branch from remote repo.

```
git fetch origin myproject/feature1
git fetch repo_alias myproject/feature1
```

If you have added remote repo as name of 'repo_alias', not as 'origin', then same command will look like this.

```
git fetch repo_alias myproject/feature1
```

It will synchronize your local repository with the remote repo's main branch.

```
git fetch origin
```

Check all branches on local

```
git branch -a
```

Check all branches on remote

```
git branch -r
```

Create a new local branch

```
git branch feature1
```

Checkout to another branch

```
git checkout feature1
```

Create and checkout to a new branch in same command

```
git branch -M feature1 or git checout -b feature1
```

Stage added/modified/deleted files.

```
git add .
```

Check status

```
git status
```

Check difference between working and the index or a tree, between the index and a tree, between two tress, between two files etc.

```git
git diff main feature1
```

Unstage the deleted file after staging

```
git restore --staged <file>
```

Delete file using git command. It will delete the file from git and disk.

```
git rm <file>
```

Commit the changes

```
git commit -m "Code updated"
```

Stage modified/deleted files and commit in one command. It will not do anything for new files.

```
git commit -am "Code updated"
```

Stage added/modified/deleted files and commit in one command. It will stage and commit for new files too.

```
git add . && git commit -am "Code updated"
```

Push local changes to remote default 'main' branch.
Here, -u is --set-upstream

```
git push
git push -u git@github.com:manojkmgit/myapp2.git main
git push git@github.com:manojkmgit/myapp2.git main
```

Delete a local branch only
Here, -D is --delete --force

```
git branch -d feature1
git branch -D feature1
```

Delete a remote branch and local too

```
git push origin --delete feature1
git branch -d feature1
```

After that, run below on other machines

```
git fetch --all --prune
```

View expanded details on Git objects such as blobs, trees, tags, and commits.

```
git show 0c708f
git show 93b3e6a --oneline
git show 93b3e6a --oneline --pretty=fuller
```

### Using log command

It shows the current HEAD and its ancestry i.e. it shows commit logs
It lists commits that are reachable by following the parent links from the given commit(s), but exclude commits that are reachable from the one(s) given with a ^ in front of them. The output is given in reverse chronological order by default.

```
git log
git log --oneline
git log --oneline --graph
git log --all --decorate --oneline --graph
git log --all --decorate --oneline --graph --pretty="%h" --date=human
git log --all --decorate --oneline --graph --pretty="%s" --date=human
git log --graph --pretty="%C(yellow) Hash: %h %C(blue)Date: %ad %C(red) Message: %s " --date=human
git log --oneline main..origin/main
git log origin/main
```

Another example
List all the commits which are reachable from foo or bar, but not from baz.

```
git log foo bar ^baz
```

Below two commands do same thing.

```
git log origin..HEAD
git log HEAD ^origin
```

### Merging via command line

If you do not want to use the merge button or an automatic merge cannot be performed, you can perform a manual merge on the command line. However, merging to main branch may not be allowed from local machine in all work places.

Step 1: From your project repository, make the changes in feature branch.

```
git fetch origin
git checkout -b feature1 origin/feature1
or
git checkout -b feature1
echo "my new file" >> newfile.txt
git add .
git commit -m "new file added"
git merge main
```

Step 2: Merge the changes from feature into main branch and update on GitHub.

```
git checkout main
git merge --no-ff feature1
git push origin main
```

If origin/main has been updated by other users on remote repo, then you can merge remote change to main of your local repo.

```
git log --oneline main..origin/main
git checkout main
git log origin/main
git merge origin/main
```

### Creating pull requests

You can create the pull requests from Github portal or you can install gh cli on local machine and then create and merge the pull requests from local machine itself. But, this level of privilege may not be provided to individual developers. Refer to other readme files for gh cli.

### Using git aliases

You can use git aliases to set up short commands for long commands.

Create an alias to perform add and commit in a single command.

```
git config --global alias.ac '!git add -A && git commit -m'
```

Usage

```
git ac 'My commit message'
```

Unset aliases when needed.

```
git config --global --unset alias.<your_alias>
```

More examples

```
git config --global alias.adog  'log --all --decorate --oneline --graph'
git adog
git config --global alias.dog  'log --decorate --oneline --graph'
git dog
```

### Managing remote origin

The word 'origin' is just a convention to refer to remote repo url. Basically origin is the default shortname that Git uses for a remote repository when you clone that remote repository. So it's just the default. When you add a remote repo, you can use any other word instead of 'origin'.

Check current remote origin

```
git remote -v
git config remote.origin.url
git config --get remote.origin.url
git config --get-all remote.origin.url
git config --list | grep remote
```

Remove remote origin

```
git remote remove origin
```

Add remote origin

```
git remote add origin git@github.com:manojkmgit/myapp2.git
```

Set remote url

```
git remote set-url origin git@github.com:manojkmgit/myapp2.git
```

### Set up git configs

```
git config --list
git config --global user.name "First Middle"
git config --global user.email "XXXXX@YYYY.com"
git config --global color.ui true
git config --global core.editor emacs
git config --global core.editor vim
```

### Reverting the changes and commits

You can undo the changes or commits in many ways.

#### Soft reset

Remove commits but preserve changes. git status will show the diff.
It will remove the commits from commit history but not change the files and index.

```
git reset --soft HEAD~1
git reset --soft b006030
git push -u origin
```

#### Hard reset

Remove commits and also remove changes.
Changes will be removed from the working directory and from the index.

```
git reset --hard HEAD~1
git reset --hard b006030
```

#### Mixed reset

Remove commits but keep changes in the working directory but NOT in the index.

```
git reset --mixed HEAD~1
git reset --mixed b006030
```

#### Revert

Revert also resets but it also records this new action in the history.

```
git revert HEAD
```

#### Recover after hard reset

You can use reflog to view log of where your HEAD and branch references have been for the last few months. 

```
git reflog
git reflog show
git reset HEAD@{2}
```

### Stashing the changes

The git stash command takes your uncommitted changes (both staged and unstaged), saves them away for later use, and then reverts them from your working copy.

```
git stash
```

If you have untracked files to stash, then add -u

```
git stash -u
```

Popping your stash removes the changes from your stash and reapplies them to your working copy.

```
git stash pop
```

Alternatively, you can reapply the changes to your working copy and keep them in your stash with git stash apply:

```
git stash apply
```

More examples

```
git stash list
git stash save "add code to our site"
git stash pop stash@{2}
git stash show
```

Or pass the -p option (or --patch) to view the full diff of a stash:

```
git stash show -p or git stash show stash@{1} -p
```
