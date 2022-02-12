---
layout: post
title: Installation of YouCompleteMe plugin.
---

[YouCompleteMe GitHub](https://github.com/ycm-core/YouCompleteMe)

## Installation

YCM is two part. 
* One part is the VIM plugin.
* Another is a compiled library.

### Minimum requirements

**Note:**
Not all VIM or NeoVim version may be supported.
Before you begin, please check the GitHub page for the requiremnets. 

At the time of writing, here are the minimum requirements.

|Editor | Current Min|
|-------|------------|
|VIM    | 8.1.2269   |
|NeoVim | 0.5        |


**Note:**
* Some features are not available in Neovim, and Neovim is not officially supported.
* VIM must be built with python3 support. So, this means, getting vim from linux repo will not
  work. [Instructions to build VIM with python3
  support](https://blog.siddharthkannan.in/vim/2019/08/31/compiling-vim-with-python/)


|Compiler | Current Min   |
|---------|---------------|
|GCC 	  |  8            |
|Clang 	  |  7            |
|MSVC 	  | 15.7 (VS 2017)|

### The VIM plugin using Vundle.

[How to install Vundle](../2022-02-08-Vundle.md)

{% highlight vimscript%}
Plugin 'ycm-core/YouCompleteMe'
{% endhighlight %}

### Build the YCM library.

{% highlight bash%}
$ apt install build-essential cmake vim-nox python3-dev
$ cd ~/.vim/bundle/YouCompleteMe
$ ./install.py --clangd-completer --java-completer --verbose
{% endhighlight %}

## Troubleshooting

1. If you are getting syntax errors or reference to functions missing, check if the VIM/NeoVim
   version is supported by the latest YCM.
