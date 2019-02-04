# git-command-line-lab

This lab is part of the [foundations trainings](https://github.com/octo-technology-downunder/octo-au-foundations) in place at [OCTO Technology Australia](http://careers.octo.com.au/).
It might take you approximately 1h30 / 2h to complete, including the game !

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
	email = notReallyMyEmail@octo.com
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

`git merge <branch>`

Merge is non-destructive (does not modify history), but involves creating extra commits.

* Creates a new commit for *every* merge which ties together the changes from both branches
* It's non destructive, meaning that existing branches are not changed
* If you are working on a feature and need to incorporate changes from master into your branch, you will have to merge master into your branch resulting in additional commits as a result of the merges `git pull`

#### About Rebase

`git rebase <branch>`

Rebase is destructive (modifies history), but can result in a linear history.

* Moves all commits to the tip of the branch that it is being rebased onto.
* You will not create a new commit just for the purpose of merging
* Results in a linear history
* Potential danger - never use on public branches!
* A good way to clean up local branches and incorporate changes from a parent branch eg `git pull --rebase`

If someone else has a copy of your branch be very careful!

#### To do:

Take the time to try out a merge and then a rebase using your projects branches and inspect the difference.

## Remote

A remote is a repository that contains the branches that you track for a given project.


### Play with remote

Git maintains a record of the location of a remote repository. You can check the current remote using `git remote -v`. If you have been following along and started a project from scratch using `git init` you may not have a remote yet, you could open an existing project and inspect the remote or you can add a remote to your current project now.

To add a remote to your project:

You can add the origin for a remote and fetch the existing branches from the remote.

For example, you could add the repository https://github.com/wjt866/git-command-line-lab.git (or your own one - just start a new repo of github), check that it has been added as a remote, and then fetch any changes.

```
git remote add origin <URL>
git remote -v
git fetch 
```

While working on a branch it will be necessary to regularly synchronise the local branch with the remote branch it tracks, in order to integrate recent changes into your branch. 

There are two options for this.
```
git pull 
git pull --rebase
```
Git pull will perform a merge, whereas git pull --rebase will perform a rebase. See above for a description of the difference between merge and rebase, however _a rebase if often a useful way to update local branches from a remote._

Difference between pull and fetch: Git pull performs both a fetch and merge (or rebase if given as an option). A fetch is a useful way to update local branches which track remotes - it won't actually change your working copy. So for example, if you want to see/inspect the latest work on a remote branch you will need to fetch in order to have the most recent up to date work. So fetch will retrieve information from the repository, with the option to merge it, while pull will automatically try and merge the changes.

Of course local changes that have been committed can also be pushed back up to the remote branch easily:

```
git push
```

## Pull Requests
### Do a pull request on github / gitlab / bitbucket etc

When work on a branch is completed it's time to do a pull request. The exact process can be different depending on where the source code is hosted. Typically a PR is made using the gui, and allows other to review code before it is merged.

#### To do

* Make a pull request for a project of your choice in gitlab, github, bitbucket etc. It could be for your own project or for this one (For example change this random word: 'random').

## Cherry Pick

A cherry pick is a way to take one or more commits from a branch and then to add those same commits to another branch. It is useful when you need to integrate only some recent changes from a branch and need to exclude other older commits.

### Cherry pick a single commit

The simplest way to demonstrate cherry pick is to add a commit to a second branch, and then cherry-pick just the one commit onto another branch.

```
echo again >> README.md
git add .
git commit -m 'adding another line for a cherry pick'
git checkout master
git cherry-pick my-new-branch
```
If you run `git log` you will now see the commit has been added to master.

### Cherry pick multiple commits

It's often common to cherry-pick more than one commit.

In this case, we will need some more information about which commits to pick. Retrieve the commit ID by using `git log --oneline` when on the branch you want to cherry pick from.

Then swap back to master and pick the commits you need, for example:

```
git cherry-pick ae8ee74 2da0c39
```

## Stash, Reset, Revert

### Stash
When working there will often be times when you wish to change branches but are not ready to commit the current changes yet. In this case you can 'stash' changes using `git stash` and retrieve them when needed using `git stash pop`

### Reset

Resetting a branch means that commits can be thrown away from history.

If you haven't already set up a remote for the project, do it now and make sure you push your work.

Make a commit.

```
echo something >> README.md
git add .
git commit -m 'commit for testing a reset'
```

Check that the commit has been added with `git log`. Now reset your branch back to master using git reset.

To hard reset and get the code from master:

`git reset --hard origin/master`

To perform a soft reset:

`git reset --soft origin/master`

#### To do

Try both a hard and soft reset. What's the difference? Check the log before and the status after resetting to observe the difference.

### Revert

Reverting changes means that a new commit is made which reverts changes that previous commits have introduced.

Make a commit, then, after checking the hash of the commit via a log:

```
git revert <hash>
```


## Rewrite History

### Amend the last commit

It's easy to amend a previous commit, rather than add an entirely new one (for example, to fix a mistake).

To update the last commit:

```
echo "first line" > README.md
git add  README.md
git commit -m 'a commit message'
echo "second line" >> README.md
git add README.md
git commit --amend
```

#### Rewrite local history

Local history can be re-written to clean up a branch before pushing it to a remote repository.

`git rebase --interactive <hashLastPushedCommit>`

##### To do:

Make a bunch of commits (but don't push them), then rebase from the start of these commits
Then rebase interactively to tidy them, using squash, reword, drop, etc

## Force with lease
### Reduce the risk of destroying other peoples work

If you accidentally pushed some local changes before rebasing them, you would need to do a `--force` push because you would be changing the history of the remote and this is not normally allowed.

But there is a problem with `git push --force`.
 
It will cause the remote repository to lose commits unconditionally and as is not always 'safe'. For exampel, if a team member has recently committed some work to a branch without you knowing you could overwrite their work. This could easily happen during a routine rebase from master which then erases a team members work.

Enter `git push --force-with-lease`

Force with lease helps to ensure that the repository is in the state we expect, allowing us to force push without the ability to overwrite other peoples work.

For a good blog about this see: https://developer.atlassian.com/blog/2015/04/force-with-lease/

## Alias and Plugins

Have a look at the config file at the bottom of the page, and investigate some of the aliases (and add them to your own config), you might want to start with the following:

```
lg
undo
who
last
wip
unwip
pr
```

## What you learnt

* How to customise git configuration
* How to create a project, make commits and understand staging
* How to work with branches
* The difference between merge and rebase
* What remotes are and how to add one
* How to cherry-pick commits
* Handy commands like stash, revert, reset
* How to rewrite local history with interactive rebases
* How to be safer when force pushing by using --force-with-lease

## Next steps

* Add some additional plugins and/or aliases to your git config, you might want to take some inspiration from below:

```
[user]
	name = Erwan Alliaume
	email = notReallyMyEmail@octo.com
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
```
## Further resources

There are many many resources to help you continue learning about git. Some useful ones are:

* `git help <command>`
* Atlassian git tutorials
* Online version of the man pages https://git-scm.com/doc

Thanks : )




