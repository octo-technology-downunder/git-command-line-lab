# git-command-line-lab
Git is a widely used system for version control. This lab focuses on core git operations that will help you to become more productive from the command line.

`git help <command>`

## Intro

## Conguration Scope

Configuration for git is not just about .gitignore files which usefully exclude files from being added to git source control.

For git, configuration exists at three different levels: *System* (`/etc/gitconfig`), *Global* (` ~/.gitconfig`) and *Project* (`.git/config`). These are accessed via the `git config` command. For a quick description of these (level, location, creation of a config) see URL : https://gist.github.com/lifuzu/9490352

Alternatively, if you run the command `git help config` you will see also see a description similar to this in the forth paragraph, along with additional documentation for each option in the help docs.

The order that the files are read is system, global, then local, with the last (local) value taking precedence.

### Update your Global scope configuration

If we want to update global configuration settings, we can do this in a few ways (using the options that you can find in the git help files).

For example: 

`git config --edit --global` will open the config file in your default editor.


The file you open will look something like the following example which is from `git help config` under heading 'EXAMPLES'.


 
            #
                       # This is the config file, and
                       # a '#' or ';' character indicates
                       # a comment
                       #
            ; core variables
            [core]
                    ; Don't trust file modes
                    filemode = false
 
            ; Our diff algorithm
            [diff]
                    external = /usr/local/bin/diff-wrapper
                    renames = true
 
            ; Proxy settings
            [core]
                    gitproxy=proxy-command for kernel.org
                    gitproxy=default-proxy ; for all the rest
 
            ; HTTP
            [http]
                    sslVerify
            [http "https://weak.example.com"]
                    sslVerify = false
                    cookieFile = /tmp/cookie.txt


Alternatively it's possible to simply list your settings directly using:
 
`git config --global --list`

It's also possible to access and set fields individually.

For example, to read an individual field:

`git config --global --get user.name`

Or to remove an individual field:

`git config --global --unset user.name`

To set your name:

`git config --global --add user.name 'Your Name'`

#### To do:

* configure your name and email address in global
* see it is updating global config in ~/.gitconfig
* update any other settings, for example default editors etc
* add basics aliases to your git config, for example:

```

[user]
	name = Erwan Alliaume
	email = ealliaumeXXXXXocto.com
[alias]
        ci = commit
        st = status
        br = branch
        co = checkout
        df = diff
        cp = cherry-pick
        gb = branch
        
```
`git config --global --add alias.ci commit`

Formatted reference:

https://git-scm.com/docs/git-config
        
## Create a Project

### Initialise a project

Create a new directory and initialise a project.

```
mkdir toto
cd toto
git init 
```
We will use this project to investigate how changes to files can be added to a repository. Run the following commands to create a basic readme file, add these files to git.

``` 
echo hello > README.md
git add .   
git status                 
```

### Staging

Files which have been added to git are now in the 'staging' area. The command `git add .` adds all files to the staging area. Essentially the staging area stores information about what will go into the next commit.

In this case the files have been added to git, but not yet committed. The status command shows the working tree status which shows the files that will be commited (in this case all have been added), as well as what may not yet be tracked by git. 

If we add another change to our file `echo world >> README.md` and check `git status` again, we will see that some changes are ready to be committed, while others are not yet staged for commit. Add this change to staging.

Another way to interact with staging is to use `git add -i` which can be used to interactively add files to the staging area.

Additionally, the diff of the files added to staging can be checked with `git diff --cached`

### Make a commit

Go ahead and make a commit using the -m option to add a message.

`git commit -m 'update readme'`

And to show the log for the last commit:

`git log -1`

#### To do:

* Add another line to the readme file, stage the file, and then use the `git commit --amend` option to amend the previous commit, rather than adding a new one. Use git log to check that you only have one commit.

## Create a Branch

Branches are typically used with git workflow strategies. This means that commits are not made directly on master, but rather are made on branches before being merged to master.

Because we have been working on master, we can create a branch which is based on master (taking the current history and 'branching out' from there), by using the git checkout command:

`git checkout -b my-new-branch`

Swap between branches using checkout, in this case the `-b` is used to create a new branch.

Additionally, some typical options for `git branch` are:

`-l` List local branches
`-r` List remote branches
`-a` List local and remote branches
`-d` Delete a branch

#### To do:

* Checkout a new branch, make a commit on the branch, check the log, and then switch back to master.

Bonus: Inspect content of the commit with `git show <commit>`

When back in master, check `git log`, you can see the commit from the branch does not show up - the history of the two branches has diverged, or forked. If we have completed some work or a series of commits on the branch we can now merge them.

## Merge and Rebase - Integrating changes

Now that we have a master and feature branch we can merge the feature branch back into master.

`git merge my-new-branch`

Check the logs again and you will see the commit has now been merged to master.

### Difference between Merge and Rebase

Both merge and rebase are concerned with integrating changes from one branch into another.

#### About Merge

Merge is non-destructive (does not modify history), but involves creating extra commits.

* Creates a new commit for *every* merge which ties together the changes from both branches
* It's non destructive, meaning that existing branches are not changed
* If you are working on a feature and need to incorporate changes from master into your branch, you will have to merge master into your branch resulting in additional commits as a result of the merges

#### About Rebase

Rebase is destructive (modifies history), but can result in a linear history.

* Moves all commits to the tip of the branch that it is being rebased onto.
* You will not create a new commit just for the purpose of merging
* Results in a linear history
* Potential danger - never use on public branches!
* A good way to clean up local branches and incorporate changes from a parent branch.

If someone else has a copy of your branch make changes non-destructively, be careful!

git revert

For example:
``

https://www.atlassian.com/git/tutorials/merging-vs-rebasing
see help docs for more

#### To do:

Add some commits to master, then rebase the feature branch
Make multiple commits on master and a branch and compare the difference between merge and rebase.

## Remote

## Pull Requests

## Cherry Pick

## Stash, Reset, Revert

## Rewrite History

## Force with lease

## Alias and Plugins

Additional references:
Formatted git pages
Atlassian


######### TODO    lab to be made

some internal content to be inspired
https://docs.google.com/presentation/d/1Mb5-LCGBXjNXJh0n3Tto9MOQzEQLeXlhcW5MqATHmAw/edit#slide=id.p46
https://drive.google.com/drive/u/1/folders/1Uxv2SasAy0Cu-8Bnpwy6Fx9ELizCpk79?ogsrc=32

   





###### play with remote

git remote add origin URL
git fetch 
git pull 
git pull --rebase    ; differences with the previous one
git push

###### do a pull request on github / gitlab

###### play with cherry-pick

create a branch do a commit
come back to master, cherry pick the commit from the other branch

###### play with stash, reset and revert

do some commit
git reset --hard origin/master ; to get the code from master
do some update (not committed)
git stash    ; to put changes on the stack
git stash pop    ; to get them back
git commit ...    
git reset --soft origin/master   ; differences with Hard
git commit ...
git revert <hash>  ; see it is generating a new commuit in the reflog

####### rewrite history

######### amend last commit

echo "first line" > README.md
git add  README.md
git commit -m 'xxxxx'
echo "second line" >> README.md
git add README.md
git commit --amend             how to update the last commit

######### rewrite local history

do few commits: push one a remote
do few more commit (don't push)

git rebase --interactive <hashLastPushedCommit>
squash, rename, remove, reorder commit

######### explain --force-with-lease

if you rewrote part of the history of something already push
you need --force
but --force is dangerous
--force-with-lease is better and explain why

####### more alias and plugins

pick some of those, at least, it is the one i'm using everyday
lg
undo
who
last
wip
unwip
pr


[user]
	name = Erwan Alliaume
	email = ealliaumeWWWWWWWocto.com
[color]
	diff = auto
	status = auto
	branch = auto
	interactive = auto
    ui = auto
[help]
        autocorrect = 10
[rerere]
	enabled = true
[diff]
	renamelimit = 0
[alias]
        ci = commit
        st = status
        br = branch
        co = checkout
        df = diff
        cp = cherry-pick
        gb = branch
        gba= !git branch -a
        who = shortlog -s -n -e --
        count = rev-list HEAD --count
        ds    = diff --stat
        staged = diff --cached
        unstage = reset HEAD
        please = push --force-with-lease
        stsh = stash --keep-index
        staash = stash --include-untracked
        staaash = stash --all
        info = !git remote show origin
        ignored = "!git ls-files --others --exclude-standard"
        lg = log --graph --pretty=format:'%Cred%h%Creset %C(yellow)[%an]%Creset %s %Cgreen(%cr)%Creset' --abbrev-commit --date=relative
        ealliaume = log --graph --pretty=format:'%Cred%h%Creset %C(yellow)[%an]%Creset %s %Cgreen(%cr)%Creset' --abbrev-commit --date=relative --committer ealliaume
        commiter = log --graph --pretty=format:'%Cred%h%Creset %C(yellow)[%an]%Creset %s %Cgreen(%cr)%Creset' --abbrev-commit --date=relative --committer
        log1 = log --pretty=oneline --abbrev-commit --decorate
        undo = !git reset --soft HEAD^    # Le flag --soft va conserver les modifications dans le répertoire de travail.
        unpush = git push -f origin HEAD^:master
        last = log -1 HEAD
        first = rev-list --max-parents=0 HEAD
        rz = reset --hard HEAD
        lost = !"git fsck | awk '/dangling commit/ {print $3}' | git show --format='SHA1: %C(yellow)%h%Creset %f' --stdin | awk '/SHA1/ {sub(\"SHA1: \", \"\"); print}'"
        heads = !"git log origin/master.. --format='%Cred%h%Creset;%C(yellow)%an%Creset;%H;%Cblue%f%Creset' | git name-rev --stdin --always --name-only | column -t -s';'"
        wip = !"git add -A; git ls-files --deleted -z | xargs -0 -I {} git rm {}; git commit -m \"wip\""
        unwip = !"git log -n 1 | grep -q -c wip && git reset HEAD~1"
        patch = !"git format-patch origin/master.. -o /git/imaginatio/_patchs"
        rb = !"git wip;git rebase -i --autosquash origin/master;git unwip"
        pr = !"git fetch;git wip;git rebase --stat origin/master;git unwip;git heads"
        prd = !"git fetch;git wip;git rebase --stat origin/develop;git unwip"

[core]
	autocrlf = false
	excludesfile = /Users/erwan/.gitignore






force 
One of the only times you should be force-pushing is when you’ve performed a local cleanup after you’ve pushed a private feature branch to a remote repository (

        




