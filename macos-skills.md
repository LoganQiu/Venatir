## 允许安装并运行任何来源的应用

2024.09.30

来源：<https://sysin.org/blog/macos-if-crashes-when-opening/>

这是最常见的问题，在安装应用时会跳出：

- 提示：“应用程序” 已损坏，无法打开。您应该将它移到废纸篓。
- 提示：无法打开 “应用程序”，因为无法验证开发者。macOS 无法验证此 App 不包含恶意软件。
- 提示：“应用程序” 将对您的电脑造成伤害。您应该将它移到废纸篓。

方法：

1. 允许 “任何来源” 下载的 App
   终端输入 `sudo spctl --master-disable`
   设置完成后检查：**系统设置 --> 隐私与安全性 --> 安全性 --> 任何来源** 是否勾选。

2. 移除应用的安全隔离属性
   终端执行 `sudo xattr -dr com.apple.quarantine /Applications/name.app`
   如果不知道该如何输入，将 App 直接拖拽 `sudo xattr -rd com.apple.quarantine` 后面即可。

   一般情况下，多余的扩展属性都可以清除（个别应用例外）：
   可以直接输入：`sudo xattr -cr /Applications/*`

3. 重新签名
   多数情况下，App 已经可以正常运行了，很多和谐软件因为证书问题，仍然无法打开。此时可以重新签名。
   终端执行 `codesign --force --deep --sign - /Applications/name.app`
   **前提条件**：安装 Command Line Tools for Xcode

4. 覆盖恶意软件保护
   若是上述提示第三种被判断为恶意软件，确定想要运行。
   执行：访达 --> 应用程序 --> 右击软件 --> 显示简介 --> 勾选 “覆盖恶意软件保护”。

## 使用 `defaults write` 命令

2024.10.29

来源：<https://nyk.ma/posts/best-defaults-write-commands-mac-os-x/>

很多 OS X 偏好设置可以通过一些第三方控制面板来改变。不过背后起作用的核心命令 `defaults write` 可以做一些只能由命令行调整的改动。以下列举了十个我们认为最棒的 `defaults write` 命令。即使不是技术宅的你也一定能找到一些令你称心如意的小 trick。

### 取消自动隐藏 Dock 的触发时间

对那些隐藏了 Dock 的用户来说，把鼠标移动到屏幕下缘呼出 Dock 有一个短延迟。你可能对此不敏感，不过 [移除该延迟](http://osxdaily.com/2012/03/27/remove-auto-hide-dock-delay-mac-os-x/) 后会让此非常明显，有一种 Mac 更快了的错觉。

```bash
defaults write com.apple.Dock autohide-delay -float 0 && killall dock
```

你可以将 `-float 0` 修改为任意你想要的触发延迟，以秒为单位。

### 加快 Mission Control 的动画效果

这是另一个让你有 Mac 变快错觉的 trick，仅仅是 [缩短了 Mission Control 动画的长度](http://osxdaily.com/2012/02/14/speed-up-misson-control-animations-mac-os-x/)。

```bash
defaults write com.apple.dock expose-animation-duration -float 0.12 && killall Dock
```

同样的，可以在 `-float` 后自定义长度，以秒为单位。

### 让 Dock 中隐藏的程序图标半透明

“隐藏窗口”一直是一个 OS X 的实用功能，不过默认设置下不易区分哪些程序是被隐藏了的。这里使用一个简单的命令开启隐藏程序的 [图标半透明](http://osxdaily.com/2010/06/22/make-hidden-application-icons-translucent-in-the-dock/)，这样就便于区别了。

```bash
defaults write com.apple.Dock showhidden -bool YES && killall Dock
```

### 在 Mail 中拷贝地址时不使用全名

不论出于何种理由，当你在 Mail.app 里想拷贝一个 email 地址时，总是会附上联系人的全名。很讨厌。不过用一个 `defaults write` 命令可以 [关掉](http://osxdaily.com/2012/05/03/stop-pasting-full-names-copy-email-address-mac-os-x-mail/) 这功能。

```bash
defaults write com.apple.mail AddressesIncludeNameOnPasteboard -bool false
```

### 在“快速查看”中开启文本选择功能

空格键一直是 OS X 最有用的功能之一。并且在快速预览中拥有 [文字选择](http://osxdaily.com/2011/11/21/select-text-in-quick-look-windows/) 功能似乎也是理所应当的。下面告诉你怎样打开它。

```bash
defaults write com.apple.finder QLEnableTextSelection -bool TRUE;killall Finder
```

### 在 Finder 中始终显示隐藏的文件和文件夹

隐藏的文件，不出意外地，在 Finder 中默认是不显示的。[显示隐藏文件](http://osxdaily.com/2009/02/25/show-hidden-files-in-os-x/) 很容易做到，对高玩来说是刚需吧。

```bash
defaults write com.apple.finder AppleShowAllFiles -bool YES && killall Finder
```

### 完全隐藏桌面图标

如果你桌面上堆了一大坨乱七八糟的文件，[隐藏所有桌面图标](http://osxdaily.com/2009/09/23/hide-all-desktop-icons-in-mac-os-x/) 会让你像通了便一样一身轻松。这些文件依然可在 `~/Desktop` 文件夹访问，你只是不让他们堆在你的萌妹子壁纸上罢了。

```bash
defaults write com.apple.finder CreateDesktop -bool false && killall Finder
```

### 在登录屏幕上显示系统信息

开启后，你可以在登录界面上查看一些 [系统的基本信息](http://osxdaily.com/2011/08/17/show-system-info-mac-os-x-lion-login-screen/)，包括 OS X 系统版本，主机名及其它。点击右上角的时间即可。适用于系统管理员和蛋疼高玩。

```bash
sudo defaults write /Library/Preferences/com.apple.loginwindow AdminHostInfo HostName
```

### 改变截图的默认保存地址

如果你经常 `⌘ + ⇧ + 3` 或 `⌘ + ⇧ + 4`，你一定明白你的桌面会很快被一堆 png 淹没。推荐解决方案是在 `~/Pictures` 或 `~/Documents` 新建一个文件夹，然后将其设为默认的 [屏幕截图保存位置](http://osxdaily.com/2011/01/26/change-the-screenshot-save-file-location-in-mac-os-x/)。

```bash
defaults write com.apple.screencapture location ~/Pictures/Screenshots
```

### 改变截图的默认格式

依然是屏幕截图。你 [可以改变](http://osxdaily.com/2010/08/16/change-the-screenshot-capture-file-format/) 默认文件格式为 JPG 或其它的格式。JPG 可以提供最佳的图片质量和压缩比。

```bash
defaults write com.apple.screencapture type jpg && killall SystemUIServer
```

### 附加：显示 ~/Library 文件夹

一个简单的命令能 [总是显示用户的~/Library 文件夹](http://osxdaily.com/2011/07/04/show-library-directory-in-mac-os-x-lion/)。这虽不是 `defaults write` 命令，不过对需要经常访问 `~/Library/` 的人而言会非常方便。

```bash
chflags nohidden ~/Library/
```

以上大多数命令应该适用于所有 OS X 版本。

### 笔者自加：关闭 Finder

```bash
defaults write com.apple.finder QuitMenuItem -bool YES && killall Finder
```

此时使用 <kbd> ⌘ </kbd> + <kbd> Q </kbd> 就可以关闭 Finder 了（即 Finder 在 <kbd> ⌘ </kbd> + <kbd> ⭾ </kbd> 切换栏里消失），随之而来的是桌面的图标消失了，当你再次打开 Finder 的时候，桌面图标就会回来。

## 删除/修改启动台（Launchpad）内的图标

2025.01.31

下载暴雪战网后会在启动台出现两个战网图标，分别是 Battle.net 和 Agent，看着十分别扭，遂想删除 Agent。

**原理**：macOS 使用 sqlite 来进行启动台以及小组件的管理。
**方法**：在终端中输入 `sqlite3 $(find /private/var/folders \( -name com.apple.dock.launchpad -a -user $USER \) 2> /dev/null)/db/db "DELETE FROM apps WHERE title='Appname';" && killall Dock`（其中的 Appname 记得改成实际应用的名字）。

在 Steam 下载的游戏中有部分在启动台会直接显示 Steam 图标，有点膈应人，遂想修改一下。

**原理**：修改对应游戏的快捷方式图标文件。
**方法**：进入对应游戏的文件夹（Finder 左侧 Applications 的路径为 `/Applications` 是没有游戏图标的，Steam 下载游戏的快捷方式在 `~/Applications`，右键查看包内容即可进入文件夹），用正确图标替换 `/Content/Resources/shortcut.icns` 即可（记得将图标重命名为 `shortcut.icns`）。也可以用游戏本身图标去替换，打开 Steam 找到下载路径，继续进入 `/steamapps/common` 找到对应游戏进入即可看到。

参考：[macOS 下修复 Steam 游戏图标](https://all2h.com/post/blog/ruan-ying-jian-zhe-teng/macosxia-geng-xin-steamyou-xi-tu-biao)

## 开启或关闭 SIP

> [!warning]
> 务必请先了解什么是 SIP 且知晓关闭后会带来什么隐患再进行操作，并且仍然强烈不建议关闭 SIP。

SIP 全称为 System Integrity Protection 即系统完整性保护，具体可见[官网介绍](https://support.apple.com/zh-cn/102149)。默认都是开启状态，若是有需求关闭后请记得及时重新开启。

要开启或关闭此特性的步骤都是一样的，只是输入命令不同。

1. 进入恢复模式

   根据芯片类型不同方式也不一样：[M 系列芯片](https://support.apple.com/zh-cn/102518?type-of-mac=mac-with-apple-silicon)，[Intel 芯片](https://support.apple.com/zh-cn/102518?type-of-mac=intel-based-mac)。

2. 打开终端

   开启：输入 `csrutil enable`
   关闭：输入 `csrutil disable`
