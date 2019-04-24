## MacOS 机器学习环境配置

之前都是在以前的渣渣三星商务本上缓慢而又无奈地运行一些学习程序，现在转向macOS，首先就是进行相关环境的配置。由于是Unix系统习惯了Linux的我更喜欢这种环境，同时配置起来相对于娱乐性较强的Windows也较为简单，但是还是遇到了诸多问题。下面是一些配置步骤的摘录，如果后续需要更多的环境支持，可以在此基础上不断更新添加。

### Xcode

`Xcode` 可以直接在 `App Store` 安装，自带**GIT**，`Xcode` 命令行工具在 `terminal` 下 `sudo xcode-select —install` 安装即可。

### HomeBrew

`Homebrew` 是一款Mac OS平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

这是一个从官网下载十分漫长的过程。其中出现了一些问题以及对应的解决办法如下。

- `Error downloading Command Line Tools (macOS Mojave version 10.14) for Xcode: The operation couldn’t be completed. No such file or directory`.

  解决办法：

  `Xcode命令行工具` 与 `Xcode` 的版本一定要相同。因为我在安装命令行工具的时候没有更新原来的 `Xcode` 导致两者版本不同。更新之后发现是无法获取源的问题，去官网（<https://developer.apple.com/download/more/>）下载对应安装包默认安装即可。

- `error: RPC failed; curl 18 transfer closed with outstanding read data remaining
  fatal: The remote end hung up unexpectedly
  fatal: early EOF `

  `fatal: index-pack failed`

  解决办法：

  git 有两种拉代码的方式，一个是 HTTP，另一个是 ssh。git 的 HTTP 底层是通过 curl 的。HTTP 底层基于 TCP，而 TCP 协议的实现是有缓冲区的。 所以这个报错大致意思就是说，连接已经关闭，但是此时有未处理完的数据。键入 `*git config --global http.postBuffer 524288000// 524288000 的单位代表 B，524288000B 也就是 500MB` 并不管用。最终还是fq解决...

安装好之后就可以看到brew的相关信息。

```
MacBookforEagle-Pro:/ eagleying$ brew help
Example usage:
  brew search [TEXT|/REGEX/]
  brew info [FORMULA...]
  brew install FORMULA...
  brew update
  brew upgrade [FORMULA...]
  brew uninstall FORMULA...
  brew list [FORMULA...]

Troubleshooting:
  brew config
  brew doctor
  brew install --verbose --debug FORMULA

Contributing:
  brew create [URL [--no-fetch]]
  brew edit [FORMULA...]

Further help:
  brew commands
  brew help [COMMAND]
  man brew
  https://docs.brew.sh

```

### python3

由于自带的python27官方已经申明不再维护与更新，重点就转向3，目前较为稳定的版本应该是36，对于机器学习库的兼容性也比较强。通过brew安装python3

```
brew install python3
```

结果

```
Python has been installed as
  /usr/local/bin/python3

Unversioned symlinks `python`, `python-config`, `pip` etc. pointing to
`python3`, `python3-config`, `pip3` etc., respectively, have been installed into
  /usr/local/opt/python/libexec/bin

If you need Homebrew's Python 2.7 run
  brew install python@2

You can install Python packages with
  pip3 install <package>
They will install into the site-package directory
  /usr/local/lib/python3.7/site-packages

See: https://docs.brew.sh/Homebrew-and-Python
```

检查对应的pip版本

```
MacBookforEagle-Pro:/ eagleying$ pip3 -V
pip 19.0.3 from /usr/local/lib/python3.7/site-packages/pip (python 3.7)
```

已经是最新的，记得之后不断更新。

### 创建虚拟环境

单独的虚拟环境可以让每一个Python项目单独使用一个环境，而不会影响Python系统环境，也不会影响其他项目的环境。virtualenv可以用来管理互不干扰的独立python虚拟环境

```
sudo pip3 install virtualenv virtualenvwrapper
```

virtualenvwrapper是virtualenv的扩展包，可以更方便的新增、删除、复制、切换虚拟环境。编辑文件 `~/.bash-profile`

```
export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3
export WORKON_HOME='~/.virtualenvs'
source /usr/local/bin/virtualenvwrapper.sh
```

执行配置文件

```
source ~/.bash_profile
```

创建虚拟环境

```
mkvirtualenv env1
```

出现

```
(env1) MacBookforEagle-Pro:/ eagleying$
```

创建成功，在虚拟环境中安装包不用sudo。

#### 启动虚拟环境

```
MacBookforEagle-Pro:~ eagleying$ cd ~/.virtualenvs
MacBookforEagle-Pro:.virtualenvs eagleying$ cd wps
MacBookforEagle-Pro:wps eagleying$ source ./bin/activate
```

#### 退出虚拟环境

```
deactivate
```

#### 删除虚拟环境

```
rmvirtualenv env1
```

### 配置机器学习库

有了以上的基础就可以尽情的安装各种依赖库，通过虚拟环境管理环境种类即可。但是亲测虚拟环境运行的时候电脑发热严重，以后最好是向服务器转移。
