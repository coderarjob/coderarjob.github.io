---
layout: post
title: Git basics - Remote branches
catagories: git
---

Source: [Pro Git](https://git-scm.com/book/en/v2) for PDF, EPUB and Kindle
files.

## Terminologies

**Tracking branches** are local branches that have a direct relationship to a 
remote branch. If you’re on a tracking branch and type git pull, Git
automatically knows which server to fetch from and which branch to merge in.
The remote branch it tracks is called an **upstream branch**.

**Remote-tracking branches** are downloaded copies of branches from remote git
servers. You can checkout to a remote-tracking branch, but you will be in a
'detached HEAD' state. Any commits will get lost!

_Note: The terminologies, 'tracking branch' and 'remote-tracking branch' mean
different things._

-----

## git-clone

When you clone a repository, it generally automatically creates a master branch
that tracks origin/master. However you can set up other tracking branches if
you wish.

Cloning a repository into a newly created directory, creates remote-tracking 
branches (not the same as "tracking branches") for each branch in the cloned 
repository. However you do not have local, editable copies of them 
(That is you do not have a local branch that tracks a remote branch).

If you want to contribute on a remote branch, you will need to create a local
tracking branch.

```bash
$ git clone git@github.com:coderarjob/meghaos-x86.git
$ cd meghaos-x86
```

Clone brings down all the remote branches and all the associated histories.
At this point you can be offline as all the remote data is available locally.

```bash
# To see the list of remote branches.
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

To see the list of tracking branches.

```bash
$ git branch -vv

* master db1d865 [origin/master] Merge branch 'develop'
```

You have one local branch 'master' which tracks the remote branch 
'origin/master'. 

Say you cant to work on 'origin/kernel/base/debugconsole' branch, you will need to
create a tracking branch first.

-----

## Create local tracking branches

Checking out a local branch from a remote-tracking branch automatically creates
a tracking branch.

```bash
$ git checkout -b kernel/base/debugconsole origin/kernel/base/debugconsole

Branch 'kernel/base/debugconsole' set up to track remote branch 'kernel/base/debugconsole' from 'origin'.
Switched to a new branch 'kernel/base/debugconsole'
```

This is very common operation, so there are shortcuts available.

### Shortcut 1

```bash
$ git checkout --track origin/kernel/base/debugconsole

Branch 'kernel/base/debugconsole' set up to track remote branch 'kernel/base/debugconsole' from 'origin'.
Switched to a new branch 'kernel/base/debugconsole'
```

### Shortcut 2

This operation is so common, that there's even a shortcut for that shortcut!

If a branch name you are trying to checkout **a)** does not exit and 
**b)** exactly matches a name on only one remote, Git will create a tracking branch.

```bash
$ git checkout kernel/base/debugconsole

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

-----

## Setup an exiting local branch as a tracking branch

If you already have a local branch and want to set it to a tracking branch, you
use the `-u` or `-set-upstream-to` option.

```bash
$ git checkout debugging
$ git branch -u origin/kernel/base/debugconsole
Branch 'debugging' set up to track remote branch 'kernel/base/debugconsole' from 'origin'.
```

This option can also be used to change the upstream branch.

-----

## Checking out a remote-tracking branch or tag.

A branch is a pointer that points to one of the latest commit. It also has a
pointer to it parent commit(s). However, 'tags' or 'remote-tracking' branches
are pointers that do not change. That means, your new commit become untracable,
as the tag or the remote-tracking branch will not move forward to your latest
commit. It can only be accessed by the exact commit hash.

This is what is meant by 'detached HEAD'. The HEAD points to the current
commit, but is not part of a branch.

### Checking out tags

A tag always points to a particular commit and can have associated data.

Say you want to fix a bug on an older version, you first need to create a new
branch for that. 

```bash
# We want to make change on the files tagged `v2.0.0.0`
# New branch is created so that we can edit the files and commit.

$ git checkout -b version2 v2.0.0.0
Switched to a new branch 'version2'
```

### Checking out remote-tracking branch

Similar message will appear, when checking out tags.

```bash
$ git checkout origin/kernel/base/debugconsole
Note: switching to 'origin/kernel/base/debugconsole'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 7048ff3 Shows BOOT_INFO structure contents to PK_DEBUG. Some minor changes
```

----
----
