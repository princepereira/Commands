## Rebasing existing branch in git with upstream ##

```
git remote add upstream https://github.com/kubernetes/kubernetes.git

git pull upstream master -f

git push origin master -f
```


## Fetching a new upstream branch (release-1.32) for the first time ##

```
git fetch upstream release-1.32

git checkout upstream/release-1.32

git checkout -b release-1.32

git push origin release-1.32
```

## Revert last commit in github ##

```
git reset --hard HEAD~1

git push origin main --force
```
