---
layout: post
title: Git basics - Common actions
---

These are some common Git commands and a small description of that it does.

## Getting help

### 1. Common way to get help

{% highlight bash%}
$ git help clone
$ git help log
{% endhighlight %}

---

## Branching and merging

### 1. To see which commits are not merged.

{% highlight bash%}
$ git log --no-merges issue54..origin/master
{% endhighlight %}
This will filter the log and Git will display commits that are on the `origin/master` branch but not in `issue54` branch. Another example `git log --no-merges kernel/paging..develop`, to check if there are any commits to `develop` branch that is not there in the 'kernel/paging' branch. 

{% highlight bash%}
$ git log --no-merges kernel/paging..develop
{% endhighlight %}
Gets a list of commits in `develop` branch that is not there in the 'kernel/paging' branch. 

### 2. See all remote-tracking branches: 

{% highlight bash%}
$ git branch --remote

  origin/HEAD -> origin/master
  origin/bootloader/boot_info
  origin/feature-bootloader
  origin/kernel/base/debugconsole
  origin/kernel/base/paging
  origin/kernel/base/screen
  origin/kernel/base/userland
  origin/master
{% endhighlight %}

### 3. See list of tracking branches: 

{% highlight bash%}
$ git branch -vv

  * master db1d865 [origin/master] Merge branch 'develop'
{% endhighlight %}
You have one local branch 'master' which tracks the remote branch 'origin/master'. 

### 4. Create tracking branches from remote-tracking branches

Checking out a local branch from a remote-tracking branch automatically creates a tracking branch.

{% highlight bash%}
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
{% endhighlight %}

Now check the list of branches with `-vv` option. You will not find another
tracking branch.

{% highlight bash%}
$ git branch -vv

  * kernel/base/debugconsole 7048ff3 [origin/kernel/base/debugconsole] Shows BOOT_INFO structure contents to PK_DEBUG. Some minor changes
  master                   db1d865 [origin/master] Merge branch 'develop'
{% endhighlight %}

### 5. Creation of new branches

{% highlight bash%}
$ git checkout -b version2 v2.0.0.0
{% endhighlight %}
This creates a new branch `version2`, starting at the commit pointed by the `v2.0.0.0` tag. 

**Note**: This step is needed in reality, as tags are readonly branches, they always will point to a perticular branch and cannot be changed.

The general form of this command is 

{% highlight bash%}
$ git checkout -b <new_branch> [<start_point>]
{% endhighlight %}

`<start_point>`: The name of a commit at which to start the new branch. Defaults to **HEAD**.

This command also switches to the `<new_branch>` after creation.

### 6. Archiving a branch (Release a build)

{% highlight bash%}
$ git archive master --prefix='meghaos-v2-x86/' | gzip > meghaos.tar.gz
{% endhighlight %}

The `git archive <branch>` creates an archive of the latetst snapshot of your
code in the `branch`. 

If someone opens that tarball, they get the code under `meghaos-v2-x86`
directory.

For unique file name, we can use the `git describe` command.

{% highlight bash%}
$ git archive master --prefix='meghaos-v2-x86/' | gzip > `git describe master`.tar.gz
$ ls *.tar.gz
210619.0-7-gdb1d865.tar.gz
{% endhighlight %}

### 7. Find the common ancestor(s) between two commits

{% highlight bash%}
$ git merge-base kernel/base/paging develop
b61efcb5205287ba679f2eda8c0e7ed7ab7119b9
{% endhighlight %}

This can be used to find out what has changed in one branch fron the last
common ancestor.

{% highlight bash%}
# While on kernel/base/paging
$ git merge-base kernel/base/paging develop
b61efcb5205287ba679f2eda8c0e7ed7ab7119b9
$ git diff b61efcb52
..
..
..
{% endhighlight %}

#### There is a shortcut

{% highlight bash%}
$ git diff kernel/base/paging...develop
..
..
..
{% endhighlight %}

In the context of `git diff`, the three dots between two branch names, will
show the differences between the first branch, and the last common ancestor of
the two branches.


---

## Committing

### 1. Check for Whitespace errors before committing

Before committing, we shoudl run `git diff --check`, which identifies possible whitespace errors (extra whitespaces at the end of a line or file) and lists them.

### 2. Generating a build number

{% highlight bash%}
$ git describe master
210619.0-7-gdb1d865
{% endhighlight %}

This command generates a string consisting of a name of the most recent tag
earlier than the last commit, followed by the number of commits since that tag,
followed by the partial SHA-1 value of the last commit being described.

---
---
