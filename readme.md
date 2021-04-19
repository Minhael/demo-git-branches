# demo-git-branches

Demostration for branching of Git.

# Cheat sheet
```bash
git checkout -b [new local branch] <remote branch>
git rebase -i <to remote branch>
git merge <from remote branch>
git branch --set-upstream-to=<remote>/<branch> <local branch>
git reset --hard HEAD~1
git add .
```

# Detached HEAD

**BACKUP YOUR SOURCES BEFORE DOING ANY THINGS.**

Git is pointing to a commit that is not belong to any of the branches. It is very dangerous to work on a detached HEAD. You may lose all your changes if you move to another branch.

Create a new branch from the detached HEAD may help. However, it is recommended to checkout a branch and apply the changes back:
```bash
git branch tmp
git checkout master
git merge tmp
```

# Merging branches

1. Create _rc_ branch for release candidate.
    ```bash
    git checkout -b rc master
    ```
1. Merge _NewFileB_ into _rc_
    ```bash
    # Optional: Drop `feat: add file c` commit.
    git checkout NewFileB
    git reset --hard HEAD~1

    git checkout rc

    # Optional: Note the difference between with and without fast forward
    git merge NewFileB
    git reset --hard HEAD~1     

    git merge --no-ff NewFileB
    ```
1. Merge _MoreA_ into _rc_ with interactive rebase & fast-forward.
    ```bash
    git checkout MoreA
    git rebase --interactive rc

    # Edit and mark commits as below, read rebase comments for more details
    # pick 8eb6db9 feat: add more a
    # fixup 806c993 fix: add 3 more

    git checkout rc
    git merge MoreA
    ```
1. Merge _AllAtoC_ into _rc_ with rebase and resolving conflicts.
    ```bash
    git checkout AllAtoC
    git rebase rc

    # Resolve the conflicts in fileA.txt with your IDE or text editor

    git add fileA.txt
    git rebase --continue
    git checkout rc
    git merge AllAtoC
    ```
1. Merge _BreakingChange_ into _rc_ with merge and conflicts.
    ```bash
    git checkout rc
    git merge BreakingChange

    # Resolve the conflicts in fileA.txt with your IDE or text editor

    git add fileA.txt
    git merge --continue
    ```
1. Release _rc_ to _master_ with rebase squash.
    ```bash
    git checkout master
    git merge rc
    ```
1. Clean up  

    Most of the Git hosting software/service are providing function to purge merged branches. Otherwise, just delete the branches one by one.

# Upstream branch

The default remote branch that current branch is tracking. If branch name in fetching, pulling, pushing, rebasing is omitted, default branch is used.

By default, your upstream branch is the remote branch you checked out. To change it:
```bash
git branch --set-upstream-to=<remote>/<branch> <local branch>
```

# References

* [Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/setting-up-a-repository)
* [SO about Deteched HEAD](https://stackoverflow.com/questions/10228760/how-do-i-fix-a-git-detached-head)