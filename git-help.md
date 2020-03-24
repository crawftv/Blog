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
git filter-branch --tree-filter 'rm file' HEAD
```

> [https://dalibornasevic.com/posts/2-permanently-remove-files-and-folders-from-git-repo](https://dalibornasevic.com/posts/2-permanently-remove-files-and-folders-from-git-repo)

### Push a bunch of small commits together \(squash\)

```bash
git rebase -i HEAD~<NUMBER OF COMMITS TO GO BACK>
git push force #if you have already pushed commits
```

> [https://github.com/todotxt/todo.txt-android/wiki/Squash-All-Commits-Related-to-a-Single-Issue-into-a-Single-Commit](https://github.com/todotxt/todo.txt-android/wiki/Squash-All-Commits-Related-to-a-Single-Issue-into-a-Single-Commit)

### Accidentally merged the wrong pull request/commit

```bash
git reset --merge <COMMIT HASH>
```

> [https://intellipaat.com/community/21094/undo-merge-git-rollback-a-git-merge](https://intellipaat.com/community/21094/undo-merge-git-rollback-a-git-merge)

## Reset a file and lose all progress

```bash
git checkout -- filename
```

> [https://www.norbauer.com/rails-consulting/notes/git-revert-reset-a-single-file](https://www.norbauer.com/rails-consulting/notes/git-revert-reset-a-single-file)



