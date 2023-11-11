# demo-rebase-vs-merge
Based on an Atlassian article on this https://www.atlassian.com/git/tutorials/merging-vs-rebasing I figured it's important to learn after a recent scare where I tried to rebase 100 commits and many things went haywire. Luckily I had the presence of mind to `git rebase --abort` and use `git merge` instead. But here will be a practical experiment in how it works, particularly

|                                                   |   Git rebase  |   Git merge  |
|---------------------------------------------------|---------------|--------------|
|   Same file (source add, target remove)           |               |              |
|   Same file (source add, target add)              |               |              |
|   Same file (source add, target unchanged)        |               |              |
|   Same file (source unchanged, target remove)     |               |              |
|   Same file (source unchanged, target add)        |               |              |
|   Same file (source unchanged, target unchanged)  |               |              |
|   Same file (source remove, target remove)        |               |              |
|   Same file (source remove, target add)           |               |              |
|   Same file (source remove, target unchanged)     |               |              |
|   Different file (source add, target add)         |               |              |
