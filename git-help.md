# Git Help

### I accidentally made a commit before switching off the master branch.

`git reset --hard HEAD^`  


### I made commits to master but did not push

```text
git checkout existingbranch
git merge master         # Bring the commits here
git checkout master
git reset --keep HEAD~3  # Move master back by 3 commits.
git checkout existingbranch
```

[https://stackoverflow.com/questions/1628563/move-the-most-recent-commits-to-a-new-branch-with-git](https://stackoverflow.com/questions/1628563/move-the-most-recent-commits-to-a-new-branch-with-git)

### I checked a \(large data\) file into the repo and now I can't push to github

```bash
#for folders:
git filter-branch --tree-filter 'rm -rf path/path' HEAD

#for files:
git filter-brach --tree-filter 'rm file' HEAD
```

> [https://dalibornasevic.com/posts/2-permanently-remove-files-and-folders-from-git-repo](https://dalibornasevic.com/posts/2-permanently-remove-files-and-folders-from-git-repo)

