# Git Commands
## Git branch, stage, commit, push: 
```Shell
git checkout branchName
git fetch
git pull origin branchName
git pull
git checkout -b newBranch
git diff --compact-summary
git diff --summary
git diff --name-only
git restore application/standalone/Tomcat\ Server\ Config/context.xml
git status
git add .
git commit -a -m "comment"
git push
git push --set-upstream origin newBranch
```
## Showing all git commits on a file: 
```Shell
git log --all -- path
git log --follow -- filename
```
## Check out a remote Git branch: 
```Shell
git checkout -b test origin/test
```
