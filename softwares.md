> [!important]
> 所有软件都应该去官网下载，而不应贪图方便而随意使用他人链接或者去第三方网站下载。若是不得已需要使用⌈学习版⌋，则务必准备好承担相应的风险，首先没人能保证里面是否植入了木马，其次一般厂商懒得费时费力找个人用户麻烦 **但是** 不排除特殊情况。
>
> 又：对于开发人员，更应该使用包管理工具来下载软件。

> [!note]
> GUI 软件若是自带更新检测并不建议使用包管理器下载。

## Windows 软件管理

前言：Windows 软件杂乱不堪，且各版本不好管理，因此迫切需要包管理器（Package Manager）。

### [Scoop](https://scoop.sh/)

个人首推，详细用法不赘述，官方文档简单易懂，如果不想看英文，以下链接讲解也十分细致：
[Scoop 包管理器的相关技巧和知识 – 就是这个范儿](https://www.thisfaner.com/p/scoop/)

- 清理缓存

  Scoop 并不会在安装软件后自动删除安装包，这是为了方便你在安装失败时重新安装。然而，这也会导致你的硬盘上留下许多没用的安装包，占用空间。你可以通过运行以下命令清理全部缓存的安装包：

  ```shell
  scoop cache rm *
  ```

  另外，有时 Scoop 在安装新版本的软件后不会自动删除旧版本——在这可能是由于该软件当前正在被使用，在这种情况下 Scoop 只会将环境变量链接到新版本的软件，而不会把旧版本的软件删除。久而久之，这也有可能在你的硬盘上占中不少空间。你可以通过运行以下命令清理所有过时的软件版本：

  ```shell
  scoop cleanup *
  ```

- 配置代理

  首先可以通过 `scoop config` 来查看当前配置，配置文件通常位于 `~/.config/scoop/config.json`，官方建议使用命令行配置代理：

  ```shell
  scoop config proxy 127.0.0.1:7890 # 端口号根据实际情况调整
  ```

### [Chocolatey](https://chocolatey.org/)

曾经 Windows 上最好的包管理工具且软件数量最多，但是由于其较为混乱复杂的下载逻辑逐渐式微。若需要使用完整功能还要订阅 Pro 版本且价格不菲。

### [WinGet](https://github.com/microsoft/winget-cli)

微软官方的开源包管理器，并且源包括 msstore（注意不是 Mirosoft Store，但你可以粗浅认为是超集）。

常见问题：

> 尝试更新源失败：winget

解决方法：

> 请在代理规则中加入 DOMAIN-SUFFIX 为 microsoft.com 走代理的规则，或者简单点直接开全局模式。

原因解析：

> `winget` 直接读取系统环境变量，没有独立的配置文件。

### 附：[UniGetUI](https://github.com/marticliment/UniGetUI)

原为 WingetUI，这是一个 CLI 包管理器（例如 WinGet、Scoop、Chocolatey、Pip、Npm、.NET Tool、 Cargo 和 PowerShell Gallery）创建直观的 GUI 且统一规划。

## macOS 软件管理

忘了古老的 [Fink](https://github.com/fink/fink)（依赖于 Debian 的软件包管理工具 dpkg/dselect/apt-get）和 [MacPorts](https://github.com/macports/macports-ports)（依赖于 BSD 的软件包管理工具 port，macOS 系统的标准软件包管理工具毫无争议的是 [Homebrew](https://github.com/Homebrew/brew)。

### Homebrew

<https://brew.sh/zh-cn/>

下载：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

输入 `brew help` 有基础的功能介绍如下：

```bash
Example usage:
  brew search TEXT|/REGEX/
  brew info [FORMULA|CASK...]
  brew install FORMULA|CASK...
  brew update
  brew upgrade [FORMULA|CASK...]
  brew uninstall FORMULA|CASK...
  brew list [FORMULA|CASK...]
 
Troubleshooting:
  brew config
  brew doctor
  brew install --verbose --debug FORMULA|CASK

Contributing:
  brew create URL [--no-fetch]
  brew edit [FORMULA|CASK...]

Further help:
  brew commands
  brew help [COMMAND]
  man brew
  https://docs.brew.sh
```

另外还有 `brew autoremove`（清理未使用的依赖），`brew cleanup`（清理 Homebrew 缓存）。

不确定想要下载的应用是否正确？不用担心，只需先 `brew install --cask applite` 下载 Homebrew Casks 的可视化程序，可以在里面查看应用官网，甚至直接下载应用也可以。

## 解压缩工具

- [7-Zip](https://www.7-zip.org/)🖥️<br>
  不必多言，自 Bandizip7.0 新版本加入广告后剩下的唯一真神。

- [Keka](https://github.com/aonez/Keka)🍎<br>
  可以免费下载，也可以选择去 Mac App Store 支持作者。

## 视频播放器

- [PotPlayer](https://potplayer.tv)🖥️<br>
  PotPlayer 在 2019 年某次更新后开始推送弹窗广告，但据反馈并非所有人都有，笔者习惯此应用并且时至今日也未见过一次，为了保险起见可以考虑把 PotPlayer 加入防火墙禁止联网权限。

- [IINA](https://github.com/iina/iina)🍎<br>
  基于 [mpv](https://github.com/mpv-player/mpv) 的开源视频播放器，界面优雅。

- [VLC](https://www.videolan.org/vlc)/[mpv](https://mpv.io)🖥️🍎<br>
  VLC 与 mpv 都是开源软件且都有全平台客户端，相比较而言 VLC 有着完善的 GUI（<del>但是很丑</del>有皮肤插件可以美化）；mpv 界面极简，但拥有最为丰富的功能，需要用快捷键与命令行来执行操作。<br>
  除此之外还有诸如 MPC 以及其衍生产品 MPC-HC，MPC-BE 等其他本地播放器就不详解了。

## 截图工具

- <del>[Snipaste](https://www.snipaste.com/)</del>🖥️🍎<br>
  截图软件，免费版基本满足所有需求，缺点即使专业版也不支持长截图。

- [pixpin](https://pixpin.cn/)（2024/08/08 添加）🖥️🍎<br>
  应该已经是 Snipaste 上位替代了，功能相当丰富。

- [CleanShot X](https://cleanshot.com/)（2024/12/11 添加）🍎<br>
  Mac 上毋庸置疑最强的截图录屏类软件，大版本买断制。嫌太贵也可以看一眼 [iShot](https://better365.com/ishot.html)，Pro 版本价格也适中。

## Launcher 启动器

- <del>[Fluent Search](https://www.fluentsearch.net/)</del>🖥️<br>
  （2024/11/02 更新：Fluent Search 更新迭代基本停滞，请切换至其他 Launcher）<br>
  （2025/06/21 更新：已经恢复更新并不知何时发布了 1.0 正式版本，但是与此同时 PowersToys 也推出了 Command Palette 也就是第二代 PowersToys Run，哪个顺手就用哪个吧）<br>
  基于 C# 开发的全功能搜索软件（而且是 Fluent UI！），其搜索能力与功能范围可以几乎替代 [Listary](https://www.listary.com/)。同时，工作流 Workflow 也是其一大特点，基于 C# 语法能完成各种任务。

- [Raycast](https://www.raycast.com/)（2024/09/28 添加）🍎<br>
  Raycast 是一个新兴的启动器，可以高效打开文件、软件、网站并执行各种便捷操作，可以代替 Mac 自带的 Spotlight（焦点）。当然还有老牌的 [Alfred](https://www.alfredapp.com/) 和更适合国人的轻巧型启动器 [HapiGo](https://www.hapigo.com/) 可以自行尝试。
  稍微展开来说，免费版的 Raycast 就已经包含了剪贴板历史记录管理（支持剪贴板图片 OCR）、窗口布局管理（平替 Rectangle）、应用卸载（不好用）等功能，再搭配丰富的插件市场已经是顶级全能手了。

- [uTools](https://u.tools/)🖥️🍎<br>
  很猛很强大，但是过于死板与繁杂，所有插件都以独立进程在后台挂载，我不喜欢。

## 清理卸载工具

- <del>[Geek Uninstaller](https://geekuninstaller.com)（2024/03/01 添加）</del>🖥️<br>
  一款免费轻量的强制删除应用的软件，包括不彻底地清除卸载时注册表残留。

- [Uninstall Tool](https://crystalidea.com/uninstall-tool)🖥️<br>
  虽然是收费（单次买断），但不得不承认功能强大，能够在安装应用时追踪创建的新文件以及修改的注册表，能够批量彻底删除顽固应用包括清理注册表。

- [HiBit Uninstaller](https://www.hibitsoft.ir/Uninstaller.html)（2025/09/19 添加）🖥️<br>
  Geek Uninstaller 有点陈旧了，现在更推荐这款，全方位更有优势。

- [App Cleaner & Uninstaller](https://nektony.com/zh-hans/mac-app-cleaner)🍎<br>
  强烈推荐，买断制付费软件，可以完全彻底地卸载应用。同类型应用有 [腾讯柠檬清理](https://lemon.qq.com/)（免费）和 [CleanMyMac X](https://macpaw.com/cleanmymac)（更贵且有评论称会误删文件）。

- [Pearcleaner](https://github.com/alienator88/Pearcleaner)（2025/06/28 添加）🍎<br>
  新发现的开源应用，好用！还能**删除开发环境配置**。需要注意的是删除应用后再删除相关文件时是否会有过于相近的名字而误删。

## RSS 阅读器

其实作为 RSS 阅读器，桌面端有网页也足够，更重要的是在移动端的表现，不过这里也勉强统计一下，诸如 [Feedly](https://feedly.com/)、[Inoreader](https://www.inoreader.com/) 这些老牌的在桌面端只有网页的就不写了。

- [Folo](https://github.com/RSSNext/Folo)（2025/06/06 添加）🖥️🍎<br>
  （2025/12/23 更新：前一阵子将 AI 概要等功能改成收费，其余功能正常使用）
  最新最强的 RSS 订阅软件，由 RSSHub 作者 [DIYgod](https://github.com/diygod) 带头开发，推荐尝试。

- [NetNewsWire](https://github.com/Ranchero-Software/NetNewsWire)（2024/09/29 添加）🍎<br>
  开源 RSS 阅读器，或者去外区商店下载更优雅的但买断制 Reeder Classic. 或者订阅制 Reeder.（这俩是同一个开发者，后者是新应用可以支持播客与视频的跟踪）。

## 存储空间分析工具

- [Wiztree](https://diskanalyzer.com)🖥️<br>
  硬盘空间分析软件，集 [SpaceSniffer](http://www.uderzo.it/main_products/space_sniffer) 与 [TreeSize](https://www.jam-software.com/treesize_free) 大成，提供树状文件图、按文件类型统计等列表，并且直接从主文件记录（MFT）读取数据，拥有最快的扫描速度。如果只想要一个单纯的查找文件应用，那么 [Everything](https://www.voidtools.com/zh-cn/) 是最优的选择。

- [DaisyDisk](https://daisydiskapp.com/)（2025/03/19 添加）🍎<br>
  用旭日图展示磁盘空间分布，操作体验和视觉效果都非常出色，买断制价格适中。

- [OmniDiskSweeper](https://www.omnigroup.com/more)（2025/05/09 添加）🍎<br>
  免费，可以 DaisyDisk 替代品。

## 系统监视器

- [TrafficMonitor](https://github.com/zhongyang219/TrafficMonitor)🖥️<br>
  这是一个用于显示当前网速、CPU 及内存利用率的桌面悬浮窗软件，并支持任务栏显示，支持更换皮肤，很轻量。

- [LiteMonitor](https://github.com/Diorser/LiteMonitor)（2026/01/05 添加）🖥️<br>
  一款轻量、可定制的开源桌面硬件监控软件 — 实时监测 CPU、GPU、内存、磁盘、网络等系统性能。项目较新，更新频繁，可以尝试一下。

- [iStat Menus](https://bjango.com/mac/istatmenus/)🍎<br>
  大版本付费制，组件样式美观，免费的可以看开源的 [Stats](https://github.com/exelban/stats)。

- [Sniffnet](https://github.com/GyulyVGC/sniffnet)🖥️🍎<br>
  轻松监控网络流量的软件，体积小巧界面美观。

## 密码管理器

- [Bitwarden](https://bitwarden.com/)🖥️🍎<br>
  开源且免费的全平台密码管理器，官方的自托管需要支付一定费用，当然也可以选择同样开源的非官方兼容版 [Vaultwarden](https://github.com/dani-garcia/vaultwarden) 来 self-hosting。

- [1Password](https://1password.com/)🖥️🍎<br>
  全平台密码管理器，可惜闭源且收费，不过界面更符合交互逻辑，更美观一些，经常会有一年的试用资格发放可以关注一下。

## BT 下载器

- [qBittorrent](https://github.com/qbittorrent/qBittorrent)🖥️🍎<br>
  免费开源使用范围最广的 BT 下载器，我这里更推荐安装 [qBittorrent-Enhanced-Edition](https://github.com/c0re100/qBittorrent-Enhanced-Edition)。

- [Motrix](https://github.com/agalwood/Motrix)🖥️🍎<br>
  好看，不过相比于 qBittorrent 好像也只剩下好看这个优点了。

## 桌宠

- [Desktop Goose](https://samperson.itch.io/desktop-goose)🖥️🍎<br>
  在你屏幕上散步的鹅，它会抢走你的鼠标，在你的屏幕上留下泥巴痕迹，给你留言，给你发表情包。（你也可以创建一堆鹅如果你觉得太清闲）

- [BongoCat](https://github.com/ayangweb/BongoCat)🖥️🍎<br>
  一只可爱的猫猫会和你同步敲击键盘。

## 专业类

### Windows 类

- [PowerToys](https://github.com/microsoft/PowerToys)<br>
  这是由微软社区开发且开源的强化 windows 体验的软件，功能丰富，包括但不限于“始终置顶”，“颜色选择器”，“File Locksmith（用于了解哪些进程正在使用所选文件和目录）”，“PowerRename（使用正则表达式进行高级批量重命名）”等等。后台挂载占用内存也很低完全可以常驻。

- [DiskGenius](https://www.diskgenius.cn)<br>
  硬盘分区与数据恢复软件，功能强大，免费版基本满足所有需求。原汁原味的国产软件。

- [HWiNFO](https://www.hwinfo.com)<br>
  一款极为专业的系统信息和诊断的工具，适用于 Windows 的全面硬件分析、监控和报告。HWiNFO 可以将每一样硬件的具体参数全部显示，也能实时显示各个硬件（详细到超乎你的想象）的状态。有了它你就不再需要 CPU-Z & GPU-Z。它还有最大的两个优点：究极轻量，针对个人及非商业用途完全免费。

- [MSI Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards)<br>
  无敌的微星小飞机，注意在安装时将 RivaTuner Statistics Server（RTSS）的选项勾上，因为在使用小飞机时需要调用 RTSS 里的功能。简单而言，这是一款显卡超频工具，帮助你释放显卡最佳性能，对于游戏党来说不可错过。

- [ThrottleStop](https://www.techpowerup.com/download/techpowerup-throttlestop)<br>
  相对应地，CPU 也该有一款调式工具，throttlestop 能够帮助绕过原电脑对 CPU 的限制，最常见的操作就是通过 CPU 降压提高能耗比——降温，提高性能，降低噪音，提高续航。这种操作并不是没有风险的，过度降压的结果是系统蓝屏，未保存的数据丢失等。如果想要深度使用请详细搜索使用方法。其实，Intel 自身也有一款调试 CPU 的软件——[Intel® Extreme Tuning Utility (Intel® XTU)](https://www.intel.com/content/www/us/en/download/17881/intel-extreme-tuning-utility-intel-xtu.html)，当然并没有 ThrottleStop 那么简洁明了。

- [Process Lasso](https://bitsum.com/)（2024/02/17 添加）<br>
  一款智能调试进程级别的系统优化工具，主要功能是动态调整各个进程的优先级并设为合理的优先级以实现为系统减负的目的，极为强大，也可以对 Intel 大小核调度进行详细设置（垃圾 Wintel 在搞毛线都不知道）。

- [Shell](https://github.com/moudey/Shell)（2026/01/05 添加）<br>
  功能强大的 Windows 文件资源管理器右键菜单管理器。它允许您手动选择要集成到 Windows 文件资源管理器上下文菜单中的项目，创建自定义命令来访问所有您喜欢的网页、文件和文件夹，并直接从上下文菜单启动任何应用程序。

- [Dism++](https://github.com/Chuyu-Team/Dism-Multi-language)（2026/01/05 添加）<br>
  一直忘记把这个工具加进来，此工具有极强的系统管理优化功能，包括空间回收、启动项管理、驱动管理、资源管理器设置等等。

另：[图吧工具箱](http://www.tbtool.cn)<br>
确切地说这并不是一个软件，而是一个硬件检测工具合集，里面包含几乎所有主流工具（包括但不限于 CPU 工具，主板工具，内存工具，GPU 工具，磁盘工具，屏幕工具，烤机工具等），省去了你从各个官网找对应软件的工夫。然而需要注意，不少人在网上反馈图吧工具箱被 Windows Defender 查出病毒，当然，笔者亲自体验过并确信这是无毒的安全软件，个中真假，请自行甄别。

### macOS 类

- [Little Snitch](https://www.obdev.at/products/littlesnitch/index.html)/[LuLu](https://github.com/objective-see/LuLu)<br>
  由于 macOS 自带的防火墙只能防御入站流量（inbound traffic），无法审查出站流量（outbound traffic），如果有软件向服务器上传隐私数据就无法阻止，此时需要第三方防火墙。Little Snitch 支持大版本买断，功能丰富，界面简洁优雅；LuLu 开源免费，基础功能齐全。<br>
  常见的过滤规则：<https://ceadd.ca/blockyouxlist.txt>

- [SoundSource](https://rogueamoeba.com/soundsource/)（2024/11/04 添加）<br>
  macOS 上顶级的声音控制软件，可以控制每个应用的声音大小音效，以及输出设备等等等等。

- [AlDente](https://apphousekitchen.com/)（2024/11/05 添加）<br>
  买断制，电池保护工具，电池使用策略详细且有效，如果想要简单些可以看看开源的 [battery](https://github.com/actuallymentor/battery)。

## 小工具集

> [!note]
> 区别于以上的软件，以下的是极为小众且针对人群单一的小玩意儿，可以叫做 "widget"。之所以要找这些东西，当然是为了避免重复造轮子（bushi）。

### Windows 集

- [Ditto](https://ditto-cp.sourceforge.io/)<br>
  剪贴板管理软件，极简且好用。当然如果没有几百条文字复制记录的需求，Windows 自带的超级剪贴板完全可以胜任（使用 <kbd> Win </kbd> + <kbd> V </kbd> 呼出）。

- [Window Centering Helper](https://kamilszymborski.github.io)<br>
  顾名思义，适用于所有想要让打开的窗口全部居中的强迫症。

- [DriverStore Explorer](https://github.com/lostindark/DriverStoreExplorer)<br>
  将电脑中所有驱动分门别类列出来，包括win自带设备管理器无法显示的。适合在打开内核完整性报错有不兼容驱动却无法找到时使用，当然，也可以直接去 **C:\Windows\System32\drivers** 中查找驱动。

- [f.lux](https://justgetflux.com/)（2024/08/06 添加）<br>
  使计算机显示屏的颜色适应一天中的各个时间段，自动控制色温。

- [Keyviz](https://github.com/mulaRahul/keyviz)（2024/11/07 添加）<br>
  在屏幕上实时显示按下的键盘符号。

### macOS 集

- [Ice](https://github.com/jordanbaird/Ice)<br>
  **Ice 许久不更新有许多 issues，已于 2025/06/02 购入 Barbee**<br>
  菜单栏空间非常紧张，因此迫切需要优秀的管理软件来设置隐藏某些图标。Ice 上手快速方便，还有开源项目 [Dozer](https://github.com/Mortennn/Dozer)，[Hidden Bar](https://github.com/dwarvesf/hidden) 可以尝试，不过已经停更数年之久并不推荐。至于 [Bartender](https://www.macbartender.com/) 就取舍自断了，我并不推荐。

- [SketchyBar](https://github.com/FelixKratz/SketchyBar)<br>
  SketchyBar 可以彻底自定义菜单栏，包括但不限于菜单栏的形状，添加个性化图标，修改图标间隙大小等等。

- [f.lux](https://justgetflux.com/)（2024/10/02 添加）<br>
  使计算机显示屏的颜色适应一天中的各个时间段，自动控制色温。（得益于苹果对屏幕的高标准使其效果比在 Windows 上好出不少）

- [KeyCastr](https://github.com/keycastr/keycastr)（2024/11/07 添加）<br>
  在屏幕上实时显示按下的键盘符号。

- [Input Source Pro](https://inputsource.pro/)（2024/11/21 添加）<br>
  自定义设置根据应用和网站切换输入法。

- [CheatSheet](http://mediaatelier.com/LandingCheatSheet/)（2025/02/13 添加）<br>
  长按 <kbd> ⌘ </kbd> 会显示当前使用应用的所有快捷键，由 Media Atelier 制作，不过已经不再更新此软件同时从官网下架，推荐使用 Homebrew 下载。

- [Syntax Highlight](https://github.com/sbarex/SourceCodeSyntaxHighlight)（2025/06/19 添加）<br>
  macOS 最好用的功能之一便是按下空格可以快速预览文件，但是对于程序员来说各种代码文件或者配置文件无法预览多少有些麻烦，这个扩展可以出色地解决这个问题。

- [RCDefaultApp](https://rubicode.com/Software/RCDefaultApp/) （2025/07/09 添加）<br>
  可以批量设置默认应用。此软件最新更新在 2009 年，好在下载安装后基本还能正常使用。如果不介意使用命令行工具设置，还可以试试 [duti](https://github.com/moretension/duti)，不过也有个把年头不更新了。

- [Mole](https://github.com/tw93/Mole)（2025/11/24 添加）<br>
  像鼹鼠一样深入挖掘来优化你的 Mac！好用！

> 通篇未包含 IObit 和 CCleaner 两家的任何软件，因为笔者已默认其为流氓软件与 2345 之流并无明显区别，然而其中其实也不乏能使用的软件（如 CC 的 Defraggler），本着宁可错杀不可放过原则，全部舍弃。
