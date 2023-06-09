---
title: Mac使用技巧(二)
date: 2022-03-20 18:00:00
tags: [Mac, 使用技巧, 202203]
category: 学习技巧
---

Mac 使用技巧，一篇文章写完太长，影响阅读体验，于是写成了一个系列。

该系列相对比较全面，对于Mac 0 基础的小白有着很大的提升。

同时为了便于查找，将每篇文章的主要内容都会做一个简单的目录介绍和锚点跳转。

- <a href="#target1">废纸篓的使用技巧</a>
- <a href="#target2">常用 SSH 命令</a>
- <a href="#target3">在 macOS 中锁定文件和目录</a>
- <a href="#target4">隐藏或显示 Dock 栏</a>
- <a href="#target5">加密文件/文件夹的三种方式</a>
- <a href="#target6">应用卡住</a>

<!-- more -->

### <a name="target1">废纸篓的使用技巧</a>

- 彻底删除文件

  清倒废纸篓：除了常规的进入废纸篓里点击图标外，更好用的快捷键是：【⌘ + ⇧ + ⌫】。

- 查看删除文件

  对于不想恢复，又想查看的已删除文件，点击图标，然后再按一下空格键。

- 卸载程序

  在【应用程序】文件夹里找到图标，右键单击选择【移到废纸篓】或者直接拖拽进废纸篓都行。

- 从 Dock 中移除图标

  对于固定在屏幕底部 Dock 栏上的图标，如果嫌碍事了，直接将其拖拽到废纸篓里即可移出，而相应文件或者程序也并不会被删除。

### <a name="target2">常用SSH命令</a>

- 生成密钥

  ```bash
  ssh-keygen
  ```

- ssh远程登录主机

  ```bash
  ssh user@host -p port
  #或者
  ssh host -l user -p port
  ```

  > user：登录的用户
  >
  > host：登录的主机
  >
  > 参数 -p：ssh端口
  >
  > 若是第一次登录该主机，会显示目标主机的公钥指纹，提示无法确认主机的身份，并询问是否继续；用户需要自行确认主机的真实性，这是为了防止ssh的中间人攻击。

- 无密码登录主机

  假设两台主机 A，B，A无密码登陆B：

  B 主机的 ~/.ssh 目录下新建文件 authorized_keys，并将A主机的公钥拷贝进去；该文件可以存多个公钥，一行一个；此时 A 主机可无密码登陆 B 主机。

- 使用别名登录主机

  新建文件 ~/.ssh/config，并写入如下内容（注意缩进）：

  ```markdown
  #config文件
  Host myhost
      HostName 138.xxx.xxx.xxx
      Port xxxx
      User root
      IdentityFile ~\.ssh\id_rsa
  ```

  > HostName：要登录的主机名或 IP 地址
  >
  > Port：目标主机的 ssh 端口
  >
  > User：登录的用户
  >
  > IdentityFile：用于登录的私钥
  >
  > 之后便可以用：`ssh myhost`登录该主机

### <a name="target3">在 macOS 中锁定文件和目录</a>

目的是为了有助于防止数据丢失。

- 使用 Finder 锁定文件和文件夹

  - 锁定：按住⌃键点按（或单击鼠标右键）要锁定的文件，【显示简介】-勾选【已锁定】；或者用快捷键【⌘+I】快速打开【显示简介】
  - 解锁：取消勾选即可

- 使用终端锁定文件和目录

  - 输入命令`ls -lO 【文件路径】`（例如，~/downloads/document.rtf。），将文件路径替换为项目的位置即可。如果 uchg 出现在输出中，则表示锁定已就位；如果 uchg 不存在，则该项目已解锁。

  - 锁定：`chflags uchg 【文件路径】`。如果该路径是目录，则修改的是整个目录及其目录下所有文件属性，即用户只能读。

    > 拓展：chflags 主要是用来修改 file flag 的，而 file flag 是在 BSD Unix 中的概念，跟 Linux 系统中的 attr 是差不多的一个概念，是文件的一些标志位来存放文件的某些属性。而这个文件属性是跟文件系统相关的，所以这个命令在不同的文件系统上的支持程度不一样，体现在某一些flag在一些特定的文件系统上没有。常用的几个属性：
    >
    > |      属性      | ls中显示 | chflags中使用             | 文件所有者能否修改？ | 详述                                                         |
    > | :------------: | :------: | ------------------------- | -------------------- | ------------------------------------------------------------ |
    > |      隐藏      |  hidden  | hidden                    | 能                   | 设置以后在 GUI 上看不到，ls 依然可以看到                     |
    > | 系统级只能添加 |  sappnd  | sappnd, sappend           | 否                   | 设置以后此文件不能够截断或者复写（overwrite），只能通过 append 模式添加内容 |
    > | 用户级只能添加 |  uappnd  | uappnd, uappend           | 能                   | 设置以后此文件不能够截断或者复写（overwrite），只能通过 append 模式添加内容 |
    > |   系统级只读   |   schg   | schg, schange, simmutable | 否                   | 不能够重命名、移动、删除、更改内容                           |
    > |   用户级只读   |   uchg   | uchg, uchange, uimmutable | 能                   | 不能够更改内容                                               |
    >
    > 通过`man chflags`查看更多用法。

### <a name="target4">隐藏或显示Dock栏</a>

- 隐藏/显示：【⌘ + ⌥ + D】
- 自动显示和隐藏：【系统偏好设置】-【程序坞与菜单栏】-勾选上【自动隐藏和显示 Dock 选项】

### <a name="target5">加密文件/文件夹的三种方式</a>

- 创建加密压缩文件

- 创建加密磁盘映像

  【磁盘工具】：在菜单栏，点击“文件”->“新建映像”->“空白映像”，打开创建磁盘映像功能。点击“存储”后，会在指定位置生成 dmg 格式的磁盘映像文件，同时也会自动进行挂载，在访达的左侧可以看到，此时可以将需要加密的文件拖到这里面来。

- 专业第三方加密软件

  太多了，比如 AutoCrypt for Mac（文档加密与解密工具），软件基于AES-256算法，其他的就不一一列举了。

### <a name="target6">应用卡住</a>

- 强制退出

  点击系统菜单栏最左侧苹果图标，选择“强制退出”，或使用【⌘ + ⌥ + ⎋（esc）」组合键。
  
  在弹出窗口中，可以看见已打开的应用，点击选择需要退出关闭的应用，然后点击右下角的“强制退出”即可。
  
- 通过“活动监视器”，关闭卡死进程

  在【启动台】-【其它】里找到活动监视器，或者通过【⌘ + ␣（空格）】直接搜索；
  
  在弹出窗口中，可以看到系统所有的启动进程，找到卡死的应用进程，点左上角的叉，在弹出对话框中，选择“强制关闭”，进行关闭进程后，应用会自动退出。







