# git-command-line-lab
Git - Basics operations and how to use it from the command line

######### TODO    lab to be made

###### system

- see the differences between config scope: https://gist.github.com/lifuzu/9490352
- configure your name and email address in global
- see it is updating ~/.gitconfig
- add basics aliases in  ~/.gitconfig fro example

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
        
###### create a project and basics operation

mkdir toto
cd toto
git init         
echo hello > README.md
git add .
git status                      show the staging area and explain what is it
git commit -m 'update readme'   
git log -1     show le last commit

echo "second line" >> README.md
git add .
git commit --amend             how to update the last commit



###### create a branch

git checkout -b my-new-branch
do some commit on the branch
git log on the branch see new commit
go back to master, don't see the new commits :   git co master
merge la branch sur master: git merge my-new-branch

###### differences between merge an rebase
https://www.atlassian.com/git/tutorials/merging-vs-rebasing

###### play with remote

git remote add origin URL
git fetch 
git pull 
git pull --rebase    ; differences with the previous one
git push


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





        




