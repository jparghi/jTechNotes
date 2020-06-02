# Issue : HEAD detached.. ?? what to do

jigishkp01-mac:platform jigishkp$ git branch
* (HEAD detached at 4746959f). ??

If you can’t solve the problem in short timeframe, delete the local branch and re checkout the image. 

2.	How to commit code ?

git status
git add .
git status
git commit -sa
git review <remote branch>

if update existing review, replace the `git commit -sa` with the following or update the description:
`git commit --amend`

•	git add -A stages all changes
•	git add . stages new files and modifications, without deletions
•	git add -u stages modifications and deletions, without new files

  519  git status
  520  git add .
  521  git status
  522  git commit -sa
  523  git status
  524  git commit -sa
  525  git commit -sa.   (once you update the message, hit :wq and enter. )
  526  git status.           //
  527  git log
  528  git review stable/mario
  529  history


3.	add the changes in the created review

git review -d 9889.  // changes the branch to review branch
git commit --amend.  
git review nso/casablanca. Or git review 
4.	How to switch branch ?

Example: Current branch is 
jigishkp01-mac:sdwan jigishkp$ git branch
  nso/amsterdam
* stable/Mario

You want to switch to “nso/Casablanca”

Answer : 
git checkout nso/casablanca

5.	git replace local version with remote version

jigishkp01-mac:deployments jigishkp$ git pull --rebase origin master
Enter passphrase for key '/Users/jigishkp/.ssh/id_rsa': 
From ssh://gerrit.stratlab.local:30004/nso/deployments
 * branch            master     -> FETCH_HEAD
Already up to date.
HEAD is up to date.

6.	If you want to overwrite only one file:
git fetch
git checkout origin/master <filepath>
If you want to overwrite all changed files:
git fetch
git reset --hard origin/master


Get the GIT branch

jigishkp01-mac:deployments jigishkp$ git branch
* master


master branch and 'origin/master' have diverged, how to 'undiverge' branches'?

Ref: https://stackoverflow.com/questions/2452226/master-branch-and-origin-master-have-diverged-how-to-undiverge-branches

Problem1 :  On firing the command >Git status you get the following error. 

jigishkp01-mac:deployments jigishkp$ git status
On branch master
Your branch and 'origin/master' have diverged,
and have 1 and 50 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   environments/ltec-preprod-onap-ams/onap-overrides.yaml


Solution:

jigishkp01-mac:deployments jigishkp$ git rebase origin/master
Cannot rebase: You have unstaged changes.
Additionally, your index contains uncommitted changes.
Please commit or stash them.

jigishkp01-mac:deployments jigishkp$ git stash
Saved working directory and index state WIP on master: 73edb1d NSO-10479: upgrade prod with juggernaut-SR5
jigishkp01-mac:deployments jigishkp$ git status
On branch master
Your branch and 'origin/master' have diverged,
and have 1 and 50 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

nothing to commit, working tree clean

jigishkp01-mac:deployments jigishkp$ git pull --rebase
Enter passphrase for key '/Users/jigishkp/.ssh/id_rsa': 
First, rewinding head to replay your work on top of it...
jigishkp01-mac:deployments jigishkp$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
