# 机器学习笔记

## 简介

**作者：子实**

机器学习笔记，使用 `jupyter notebook (ipython notebook)` 编写展示。

`Github` 加载 `.ipynb` 的速度较慢，建议在 [Nbviewer](http://nbviewer.jupyter.org/github/zlotus/notes-LSJU-machine-learning/blob/master/ReadMe.ipynb?flush_cache=true) 中查看该项目。

----

## 目录

来自斯坦福网络课程《机器学习》的笔记，可以在[斯坦福大学公开课：机器学习课程](http://open.163.com/special/opencourse/machinelearning.html)观看。

根据视频内容，对每一讲的名称可能会有所更改（以更好的体现各讲的教学内容）。

- 【第1讲】 机器学习的动机与应用（主要是课程要求与应用范例，没有涉及机器学习的具体计算内容）
- 【第2讲】 [监督学习应用-线性回归](chapter02.ipynb)
- 【第3讲】 [线性回归的概率解释、局部加权回归、逻辑回归](chapter03.ipynb)
- 【第4讲】 [牛顿法、一般线性模型](chapter04.ipynb)
- 【第5讲】 [生成学习算法、高斯判别分析、朴素贝叶斯算法](chapter05.ipynb)




[fish](http://fishshell.com/) - the friendly interactive shell [![Build Status](https://travis-ci.org/fish-shell/fish-shell.png?branch=master)](https://travis-ci.org/fish-shell/fish-shell)
================================================

fish is a smart and user-friendly command line shell for OS X, Linux, and the rest of the family. fish includes features like syntax highlighting, autosuggest-as-you-type, and fancy tab completions that just work, with no configuration required.

For more on fish's design philosophy, see the [design document](http://fishshell.com/docs/2.0/design.html).

## Quick Start

fish generally works like other shells, like bash or zsh. A few important differences can be found at <http://fishshell.com/tutorial.html> by searching for magic phrase 'unlike other shells'.

Detailed user documentation is available by running `help` within fish, and also at <http://fishshell.com/docs/2.0/index.html>

## Building

fish is written in a sane subset of C++98, with a few components from C++TR1. It builds successfully with g++ 4.2 or later, and with clang. It also will build as C++11.

fish can be built using autotools or Xcode. autoconf 2.60 or later is required.

fish requires gettext for translation support.

### Autotools Build

    autoconf
    ./configure
    make [gmake on BSD]
    sudo make install

### Xcode Development Build

* Build the `base` target in Xcode
* Run the fish executable, for example, in `DerivedData/fish/Build/Products/Debug/base/bin/fish`

### Xcode Build and Install

    xcodebuild install
    sudo ditto /tmp/fish.dst /

## Build Dependencies (or, Help, it didn't build!)

If fish reports that it could not find curses, try installing a curses development package and build again.

On Debian or Ubuntu you want:

    sudo apt-get install libncurses5-dev

on RedHat, CentOS, or Amazon EC2:

    sudo yum install ncurses-devel
