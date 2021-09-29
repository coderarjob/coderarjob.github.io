---
layout: post
title: Git basics - Common actions
---

These are some common Git commands and a small description of that it does.

## Getting help

### 1. Common way to get help

```bash
$ git help clone
$ git help log
```

---

## Branching and merging

### 1. To see which commits are not merged.

```bash
$ git log --no-merges issue54..origin/master
```
This will filter the log and Git will display commits that are on the `origin/master` branch but not in `issue54` branch. Another example `git log --no-merges kernel/paging..develop`, to check if there are any commits to `develop` branch that is not there in the 'kernel/paging' branch. 

```bash
$ git log --no-merges kernel/paging..develop
```
Gets a list of commits in `develop` branch that is not there in the 'kernel/paging' branch. 

### 2. See all remote-tracking branches: 

```bash
$ git branch --remote

  origin/HEAD -> origin/master
  origin/bootloader/boot_info
  origin/feature-bootloader
  origin/kernel/base/debugconsole
  origin/kernel/base/paging
  origin/kernel/base/screen
  origin/kernel/base/userland
  origin/master
```

### 3. See list of tracking branches: 

```bash
$ git branch -vv

  * master db1d865 [origin/master] Merge branch 'develop'
```
You have one local branch 'master' which tracks the remote branch 'origin/master'. 

### 4. Create tracking branches from remote-tracking branches

Checking out a local branch from a remote-tracking branch automatically creates a tracking branch.

```bash
# Method 1 (long method)
$ git checkout -b kernel/base/debugconsole origin/kernel/base/debugconsole

Branch 'kernel/base/debugconsole' set up to track remote branch 'kernel/base/debugconsole' from 'origin'.
Switched to a new branch 'kernel/base/debugconsole'

# Method 2 (shortcut A)
$ git checkout --track origin/kernel/base/debugconsole

Branch 'kernel/base/debugconsole' set up to track remote branch 'kernel/base/debugconsole' from 'origin'.
Switched to a new branch 'kernel/base/debugconsole'

# Method 3 (shortcut B)
$ git checkout --track origin/kernel/base/debugconsole

Branch 'kernel/base/debugconsole' set up to track remote branch 'kernel/base/debugconsole' from 'origin'.
Switched to a new branch 'kernel/base/debugconsole'
```

Now check the list of branches with `-vv` option. You will not find another
tracking branch.

```bash
$ git branch -vv

  * kernel/base/debugconsole 7048ff3 [origin/kernel/base/debugconsole] Shows BOOT_INFO structure contents to PK_DEBUG. Some minor changes
  master                   db1d865 [origin/master] Merge branch 'develop'
```

### 5. Creation of new branches

```bash
$ git checkout -b version2 v2.0.0.0
```
This creates a new branch `version2`, starting at the commit pointed by the `v2.0.0.0` tag. 

**Note**: This step is needed in reality, as tags are readonly branches, they always will point to a perticular branch and cannot be changed.

The general form of this command is 

```bash
$ git checkout -b <new_branch> [<start_point>]
```

`<start_point>`: The name of a commit at which to start the new branch. Defaults to **HEAD**.

This command also switches to the `<new_branch>` after creation.

---

## Commiting

### 1. Check for Whitespace errors before commiting

Before committing, we shoudl run `git diff --check`, which identifies possible whitespace errors (extra whitespaces at the end of a line or file) and lists them.

---
---
