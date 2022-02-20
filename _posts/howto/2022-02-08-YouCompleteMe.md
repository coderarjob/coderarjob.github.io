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

### Install the YCM plugin to VIM.

With Vundle, all you need to do is to add the below like to your `.vimrc`. See
[How to install Vundle](../Vundle)

{% highlight vimscript%}
Plugin 'ycm-core/YouCompleteMe'
{% endhighlight %}

### Build the YCM library.

{% highlight bash%}
$ # Install build requrements
$ apt install build-essential cmake vim-nox python3-dev
$ # Go to the directory where VIM plugin was installed. With Vundle it is here.
$ cd ~/.vim/bundle/YouCompleteMe
$ # I am building for Java and C language support. See YCM documentation for all the options.
$ ./install.py --clangd-completer --java-completer --verbose
{% endhighlight %}

## Troubleshooting

1. If you are getting **syntax errors** or **reference to functions missing** errors in the YCM 
   vim script files, then check if your VIM/NeoVim version is supported by the latest YCM.
   I found that VIM is better supported than NeoVim.

2. **NoExtraConfDetected: No .ycm_extra_conf.py file detected** error. I was using NeoVim 0.44,
   which is was anyways not supported. But I found a fix
   [source](https://stackoverflow.com/questions/47812854/vim-youcompleteme-error-noextraconfdetected-no-ycm-extra-conf-py-file-detecte)

   {% highlight vimscript%}
   let g:ycm_global_ycm_extra_conf = '/home/coder/.vim/bundle/YouCompleteMe/third_party/ycmd/.ycm_extra_conf.py
   Plugin 'ycm-core/YouCompleteMe'
   {% endhighlight %}

   **Note:**

   `find ~/.vim/bundle -name .ycm_extra_conf.py` shows many files with that name, I did not know
   which file to choose, so I picked one from the top. In VIM I neither got this error, not was
   there a set to set this variable.
