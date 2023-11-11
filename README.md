# Demo Rebase VS Merge
Based on an Atlassian article on this https://www.atlassian.com/git/tutorials/merging-vs-rebasing I figured it's important to learn after a recent scare where I tried to rebase 100 commits and many things went haywire. Luckily I had the presence of mind to `git rebase --abort` and use `git merge` instead. But here will be a practical experiment in how it works, particularly

|                                                   |   Git rebase  |   Git merge  |
|---------------------------------------------------|---------------|--------------|
|   1a. Same file (source add, before target remove)| removed line remains removed, added line is added | (same as rebase) |
|   1b. Same file (source add, after target remove) | ask me to choose either source version with 2 lines, or target version with 1 line | (same as rebase) |
|   2a. Same file (source add, before target add)   | ask me to choose between old and new added line | (same as rebase) |
|   2b. Same file (source add, after target add)    | (same as 2a) | (same as 2a) |
|   3. Same file (source add, target unchanged)    | changes added | (same as rebase) |
|   4. Same file (source unchanged, target remove) | (nothing changed) | (same as rebase) |
|   5. Same file (source unchanged, target add)    | (nothing changed) | (same as rebase) |
|   6. Same file (source unchanged, target unchanged) | (nothing changed) | (same as rebase) |
|   7a. Same file (source remove, before target remove) | removed line remains removed, removed line is removed | (same as rebase) |
|   7b. Same file (source remove, after target remove) | removed line remains removed, removed line is removed | (same as rebase) |
|   8a. Same file (source remove, before target add)         |               |              |
|   8b. Same file (source remove, after target add)         |               |              |
|   9. Same file (source remove, target unchanged)   |               |              |
|   10a. Different file (source add, target add)      |               |              |

## Evaluations
### Scenario 1: Add to source, remove from target
For scenario 1b, it looks much safer as the `merge / rebase conflict` reflects the exact difference in state between the two branches. Scenario 1a silently adds and removes the lines so it can be hard to figure out what went missing or is extra. However, usually the variables that are defined in the file needs to be used somewhere, thus a `lint` or `ts-check` would help us solve the problem:
- if `source` is more accurate, we have lost a variable and checking the remote source branch will tell us what was lost.
- if `target` is more accurate, we have an additional variable and just have to remove it.


### Scenario 2: Add to source, add to target
For these scenarios, it is quite clear cut, and diffs are shown explicitly.

### Scenario 3: Add to source, unchanged target
Makes sense the additions were for some purpose. But for example, if the change was for some common function that now re-directs to a different page, and the `target-branch` didn't know of this change, it will be re-directing to the different page (which may be wrong!). The best safeguard is to have unit tests ensure all functions work, but who got time! The next best option is to do a code review before merging the new `target-branch` to `master` -- that's reasonable.

### Scenario 4, 5, 6: Source unchanged
No new commits to merge or rebase so no changes.

### Scenario 7: Remove from source, remove from target
See scenario 1.

### The rest... feels like too much time spent for now, but the bottomline is just
- write unit tests if have time (who am I kidding?)
- code review the changes, specifically in files you know that you have amended in the local branch (for example you were working on search function, don't bother checking the homepage code change, just verify that your search pages are still valid)
