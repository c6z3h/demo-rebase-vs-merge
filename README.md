# demo-rebase-vs-merge
Based on an Atlassian article on this https://www.atlassian.com/git/tutorials/merging-vs-rebasing I figured it's important to learn after a recent scare where I tried to rebase 100 commits and many things went haywire. Luckily I had the presence of mind to `git rebase --abort` and use `git merge` instead. But here will be a practical experiment in how it works, particularly

|                                                   |   Git rebase  |   Git merge  |
|---------------------------------------------------|---------------|--------------|
|   1a. Same file (source add, before target remove)| removed line remains removed, added line is added | (same as rebase) |
|   1b. Same file (source add, after target remove) | ask me to choose either source version with 2 lines, or target version with 1 line | (same as rebase) |
|   2a. Same file (source add, before target add)   | ask me to choose between old and new added line | (same as rebase) |
|   2b. Same file (source add, after target add)    |               |              |
|   3a. Same file (source add, target unchanged)    |               |              |
|   4a. Same file (source unchanged, target remove) |               |              |
|   5a. Same file (source unchanged, target add)    |               |              |
|   6a. Same file (source unchanged, target unchanged)|               |              |
|   7a. Same file (source remove, target remove)      |               |              |
|   8a. Same file (source remove, target add)         |               |              |
|   9a. Same file (source remove, target unchanged)   |               |              |
|   10a. Different file (source add, target add)      |               |              |

## Evaluations
### Scenario 1: Add to source, remove from target
For scenario 1b, it looks much safer as the `merge / rebase conflict` reflects the exact difference in state between the two branches. Scenario 1a silently adds and removes the lines so it can be hard to figure out what went missing or is extra. However, usually the variables that are defined in the file needs to be used somewhere, thus a `lint` or `ts-check` would help us solve the problem:
- if `source` is more accurate, we have lost a variable and checking the remote source branch will tell us what was lost.
- if `target` is more accurate, we have an additional variable and just have to remove it.
