# Git Help

### I accidentally made a commit before switching off the master branch.

`git reset --hard HEAD^`  


### I checked a \(large data\) file into the repo and now I can't push to github

```bash
#for folders:
git filter-branch --tree-filter 'rm -rf path/path' HEAD

#for files:
git filter-brach --tree-filter 'rm file' HEAD
```

