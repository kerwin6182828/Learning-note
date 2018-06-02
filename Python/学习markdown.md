![][py2x] [![GitHub forks][forks]][network] [![GitHub stars][stars]][stargazers] [![GitHub license][license]][lic_file]
> 免责声明：本项目旨在学习Scrapy爬虫框架和MongoDB数据库，不可使用于商业和个人其他意图。若使用不当，均由个人承担。
<img src="https://github.com/xiyouMc/PornHubBot/blob/master/img/WebHubCode2.png?raw=true" width = "700" height = "400" alt="图片名称" align=center />

 
  
   
    
     
     
## 简介

* 项目主要是爬取全球最大成人网站PornHub的视频标题、时长、mp4链接、封面URL和具体的PornHub链接
* 项目爬的是PornHub.com，结构简单，速度飞快
* 爬取PornHub视频的速度可以达到500万/天以上。具体视个人网络情况,因为我是家庭网络，所以相对慢一点。
* 10个线程同时请求，可达到如上速度。若个人网络环境更好，可启动更多线程来请求，具体配置方法见    [启动前配置]


## 环境、架构

开发语言: Python2.7

开发环境: MacOS系统、4G内存

数据库: MongoDB

* 主要使用 scrapy 爬虫框架
* 从Cookie池和UA池中随机抽取一个加入到Spider
* start_requests 根据 PorbHub 的分类，启动了5个Request，同时对五个分类进行爬取。
* 并支持分页爬取数据，并加入到待爬队列。

## 使用说明

### 启动前配置

* 安装MongoDB，并启动，不需要配置
* 安装Python的依赖模块：Scrapy, pymongo, requests 或 `pip install -r requirements.txt`
* 根据自己需要修改 Scrapy 中关于 间隔时间、启动Requests线程数等得配置

### 启动

* python PornHub/quickstart.py

## 运行截图
![](https://github.com/xiyouMc/PornHubBot/blob/master/img/running.png?raw=true)
![](https://github.com/xiyouMc/PornHubBot/blob/master/img/mongodb.png?raw=true)

## 数据库说明

数据库中保存数据的表是 PhRes。以下是字段说明:

#### PhRes 表：

	video_title:视频的标题,并作为唯一标识.
	link_url:视频调转到PornHub的链接
	image_url:视频的封面链接
	video_duration:视频的时长，以 s 为单位
	quality_480p: 视频480p的 mp4 下载地址

## 自定义

#### 抓取特定类别的视频

如需抓取特定类别的视频，编辑文件 ./PornHub/PornHub/pornhub_type.py

注释掉/删掉不需要的类别，只保留需要的类别。
该文件中已有“类别=日本”和“类别=亚洲”的示例。
若需要其它的类别，请打开pornhub网站的相应类别，
类别ID就在网址里。


[py2x]: https://img.shields.io/badge/python-2.x-brightgreen.svg
[issues_img]: https://img.shields.io/github/issues/xiyouMc/WebHubBot.svg
[issues]: https://github.com/xiyouMc/WebHubBot/issues

[forks]: https://img.shields.io/github/forks/xiyouMc/WebHubBot.svg
[network]: https://github.com/xiyouMc/WebHubBot/network

[stars]: https://img.shields.io/github/stars/xiyouMc/WebHubBot.svg
[stargazers]: https://github.com/xiyouMc/WebHubBot/stargazers

[license]: https://img.shields.io/badge/license-MIT-blue.svg
[lic_file]: https://raw.githubusercontent.com/xiyouMc/WebHubBot/master/LICENSE






















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





