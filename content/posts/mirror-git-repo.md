---
title: "Mirror Git Repo"
date: 2024-12-14T23:28:54+09:00
draft: false
---

## Mirror Git Repo

```bash
git clone <repo-url>
cd <repo-name>
git branch -r \
  | grep -v '\->' \
  | sed "s,\x1B\[[0-9;]*[a-zA-Z],,g" \
  | while read remote; do \
      git branch --track "${remote#origin/}" "$remote"; \
    done
git fetch --all
git pull --all
git remote add <mirror-name> <mirror-url>
git push --mirror <mirror-name>
```

## Reference

[How to fetch all branches locally](https://stackoverflow.com/questions/10312521/how-do-i-fetch-all-git-branches)
