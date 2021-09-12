---
layout: post
title: Git basics
---

Source: [Pro Git](https://git-scm.com/book/en/v2) for PDF, EPUB and Kindle
files.

## Remote branches

**Tracking branches** are local branches that have a direct relationship to a 
remote branch. If you’re on a tracking branch and type git pull, Git
automatically knows which server to fetch from and which branch to merge in.
The remote branch it tracks is called an **upstream branch**.

**Remote-tracking branches** are downloaded copies of branches from remote git
servers. You can checkout to a remote-tracking branch, but you will be in a
'detached HEAD' state. Any commits will get lost!

```
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

HEAD is now at 7048ff3 Shows BOOT_INFO structure contents to PK_DEBUG.  Some minor changes
$                                                                     [7048ff3]

```

_Note: The terminologies, 'tracking branch' and 'remote-tracking branch' mean
different things._

-----

`git-clone`

When you clone a repository, it generally automatically creates a master branch
that tracks origin/master. However you can set up other tracking branches if
you wish.

Clonning a repository into a newly created directory, creates remote-tracking 
branches (not the same as "tracking branches") for each branch in the cloned 
repository. However you do not have local, editable copies of them 
(That is you do not have a local branch that tracks a remote branch).

If you want to contribute on a remote branch, you will need to create a local
tracking branch.

```
$ git clone git@github.com:coderarjob/meghaos-x86.git
$ cd meghaos-x86
```

Clone brings down all the remote branches and all the assiciated histories.
At this point you can be offline as all the remote data is available locally.

```
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

```
# To see the list of tracking branches.
$ git branch -vv

* master db1d865 [origin/master] Merge branch 'develop'
```

You have one local branch 'master' which tracks the remote branch 
'origin/master'. 

Say you cant to work on 'origin/kernel/base/userland' branch, you will need to
create a tracking branch.

### Create local tracking branches

Checking out a local branch from a remote-tracking branch automatically creates
a tracking branch.

```
$ git checkout -b kernel/base/debugconsole origin/kernel/base/debugconsole

Branch 'kernel/base/debugconsole' set up to track remote branch 'kernel/base/debugconsole' from 'origin'.
Switched to a new branch 'kernel/base/debugconsole'
```

This is very common operation, so there are shortcuts available.

#### Shortcut 1

```
$ git checkout --track origin/kernel/base/debugconsole

Branch 'kernel/base/debugconsole' set up to track remote branch 'kernel/base/debugconsole' from 'origin'.
Switched to a new branch 'kernel/base/debugconsole'
```

#### Shortcut 2

This operation is so common, that there's even a shotcut for that shortcut!

If a branch name you are trying to checkout **a)** does not exit and 
**b)** exactly matches a name on only one remote, Git will create a tracking branch.

```
$ git checkout kernel/base/debugconsole

Branch 'kernel/base/debugconsole' set up to track remote branch 'kernel/base/debugconsole' from 'origin'.
Switched to a new branch 'kernel/base/debugconsole'
```

Now check the list of branches with `-vv` option. You will not find another
tracking branch.

```
$ git branch -vv

* kernel/base/debugconsole 7048ff3 [origin/kernel/base/debugconsole] Shows BOOT_INFO structure contents to PK_DEBUG. Some minor changes
  master                   db1d865 [origin/master] Merge branch 'develop'
```

### Setup a local branch as a tracking branch

If you already have a local branch and want to set it to a tracking branch, you
use the `-u` or `-set-upstream-to` option.

```
$ git checkout debugging
$ git branch -u origin/kernel/base/debugconsole
Branch 'debugging' set up to track remote branch 'kernel/base/debugconsole' from 'origin'.
```

This option can also be used to change the upstream branch.
