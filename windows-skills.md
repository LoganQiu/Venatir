## 系统字体更改

2024.01.05

> [!WARNING]
> 直接更换系统字体可能使电脑出现意料之外的状况，因此在更换前务必备份个人数据。

- 最简单的方法是使用 [MacType](https://github.com/snowie2000/MacType) 渲染字体，可以同时与以下几种方法搭配使用。

- 若想替换字体，较简单的方法是使用 [No!! MeiryoUI](https://github.com/Tatsu-syo/noMeiryoUI)，但可能替换不完全。

- 若是想直接在 Windows 上操作，那么有以下两种方法：

  - 使用注册表（<mark>Win11 已失效</mark>）
    点击 **Windows+R** 搜索 **Regedit** 以此打开注册表编辑器，然后找到以下路径：
    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts`
    找到以下键值并将其修改为你需要的字体：
    **Microsoft YaHei & Microsoft YaHei UI (TrueType)**
    **Microsoft YaHei Bold & Microsoft YaHei UI Bold (TrueType)**
    **Microsoft YaHei Light & Microsoft YaHei UI Light (TrueType)**

  - 使用高级启动
    将字体 **Fonts** 文件夹放置在 C 盘根目录中 **C:\\Fonts**，需要注意的是里面的字体文件应为 **msyh.ttc, msyhbd.ttc, msyhl.ttc**，即将想要更换的字体重命名为以上三项（因为简中 Windows 默认字体为微软雅黑），然后进入 **设置 --> 系统 --> 恢复 --> 高级启动**，进入 **Windows 恢复环境（WinRE）**，选择 **疑难解答 --> 高级选项 --> 命令提示符**，执行 `xcopy C:\Fonts C:\Windows\Fonts` 命令进行全局替换。
    相当于把微软雅黑改成其他字体，唯一缺点是别人发的用微软雅黑排版的文件会变成别的字体。*当然，一般人不会用微软雅黑写文件。*

> [!important]
> 由于字体更改自然导致 hash 验证会错误，那么在以后其他情况下需要执行
>
> ```powershell
> Dism /Online /Cleanup-Image /ScanHealth
> Dism /Online /Cleanup-Image /CheckHealth
> Dism /Online /Cleanup-image /RestoreHealth
> sfc /SCANNOW
> ```
>
> 时第三步卡在 62.3% 大概率由此导致，并且以上步骤可能会使系统字体恢复初始状态。

附：更改系统字体大小可能导致 **桌面图标间隔距离异常**，解决方法为打开注册表，进入路径 `HKEY_CURRENT_USER\Control Panel\Desktop\WindowMetrics`，将 **IconVerticalSpacing**（垂直距离）和 **IconSpacing**（水平距离）改为 -1125（不同电脑中默认值不同，但在 -1125 上下浮动）即可。

关于字体选择，主要在两个大类中挑选--无衬线字体（sans-serif）和衬线字体（serif），此处先按下不表。

## 注册表操作

2024.01.05

> [!WARNING]
> 对注册表的不正当修改可能使电脑出现意料之外的状况，因此在更换前务必备份原始注册表与个人数据。

**注册表 (Registry)** 是 Microsoft Windows 操作系统和其应用程序中的一个重要的层次型数据库，用于存储系统和应用程序的设置信息。

通常不建议对注册表进行主动更改，如果你只是看着已删除软件仍在右键菜单里呆着感到烦躁，或者单独想对右键菜单进行编辑，那么笔者建议使用以下几个轻便的小工具：

- [RightMenuMgr](https://github.com/Gredicer/Right-mouse-button-management)
- [ContextMenuManager](https://github.com/BluePointLilac/ContextMenuManager)
- [FileMenu Tools](https://www.lopesoft.com/index.php/en/download/filemenu-tools)（仅能隐藏而非删除）

如果以上方法未能解决的烦恼或者你想要对注册表进行其他操作，那么就请继续：

- Regedit（Windows 系统自带的注册表编辑器，适合在目标明确的情况下使用）
- [Registry Workshop](http://torchsoft.com)
  增强版注册表编辑器，还能记录每一次更改（提供的官网链接里还有另一个有趣的小工具请自行探寻）。
- [Wise Registry Cleaner](https://www.wisecleaner.com/wise-registry-cleaner.html)
  如果你不想手动操作注册表，那么这可能是一个不错的选择，它有注册表清理、系统优化、注册表整理等功能且可以一键执行，但是笔者未曾使用过此软件，不保证使用体验。

这是 Microsoft 官方对 shell 扩展的一些介绍：[注册 Shell 扩展处理程序](https://learn.microsoft.com/zh-cn/windows/win32/shell/reg-shell-exts)。

提供一些简单的思路：

```txt
# 右键菜单杂项
HKEY_CLASSES_ROOT\\*\shellex\ContextMenuHandlers
HKEY_CLASSES_ROOT\Directory\Background\shellex\ContextMenuHandlers
HKEY_CLASSES_ROOT\Directory\shell
HKEY_CLASSES_ROOT\Directory\shellex\ContextMenuHandlers
HKEY_CLASSES_ROOT\Folder\shell
HKEY_CLASSES_ROOT\Folder\shellex\ContextMenuHandlers
# 不同类型文件默认打开方式修改
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\
```

## 更改 User 用户名

2024.01.08

> [!TIP]
> 新电脑或重装系统的 Win11 跳过联网激活即可指定用户名，具体方法为在联网界面按下 <kbd> Shift </kbd> + <kbd> F10 </kbd> 打开命令提示符窗口，输入 `OOBE\BYPASSNRO` 即可，自动重启再次到联网界面后多了一个“我没有 Internet 连接”选项，选择后继续前进到询问“谁将使用此设备？”就可以输入你的用户名。
>
> 所以何不重装系统来无风险更改用户名呢？

既然来到了这个互联网犄角旮旯，那就默认了对此操作的迫切需求，也就不废话在何种情况下需要修改 User 用户名了。以下操作皆有风险，务必备份 **全盘文件**，推荐使用 Ghost 分区备份（当然你也可以足够信任我直接开搞）。

最后一次规劝：在使用计算机一段时间后不要执行此操作，以下方法仅对还未对计算机做出什么更改的情况下十分有效。

1. 以 **管理员身份** 运行 cmd，输入 `net user administrator /active:yes` 来开启 administrator 账户，然后输入 `whoami /user`，记下当前用户对应的 SID。
2. 按下 <kbd> Ctrl </kbd> + <kbd> Alt </kbd> + <kbd> Delete </kbd>，切换用户，选择 administrator 账户，进入 **C:\Users** 目录，将用户名文件夹重命名。可能会报错（有后台程序仍在运行时），重启计算机并以 administrator 账户登录再修改。
3. 打开注册表编辑器，在路径 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList` 下的几个项中找到你刚才记下的 SID（应该是 1001 结尾，当然可能有所不同），双击数值名称 **ProfileImagePath** 并修改数值数据成你希望的用户名。稳妥起见，必须将整个注册表中的 `\原用户名\` 全局替换为 `\新用户名\`。
4. 切换回新用户名账户，按下 <kbd> Win </kbd> + <kbd> R </kbd> 输入 `control userpasswords2` 进入用户账户界面，选择 **属性** 将 **用户名** 修改为新的（系统可能已经帮你完成更改）。进入系统 **环境变量**，将 `\原用户名\` 全部改为 `\新用户名\`。
5. 重启，以新用户名账户登录，检查基本无恙后可以 **管理员身份** 运行 cmd，输入 `net user administrator /active:no` 来禁用 administrator 账户。

至此结束所有流程，如果后续发现某些应用无法正常运行，请尝试卸载重装，或者仍然不行只剩下最后一条路——以 **管理员身份** 运行 cmd，输入 `mklink /d C:\Users\{原用户名} C:\Users\{新用户名}`，这个操作使 C 盘用户文件夹下出现一个带小箭头的旧用户名图标，这即为符号链接，关于此方面知识，请参考 [微软文档](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/mklink)。

## 应用开机自启

2024.08.25

应用自启分为 **系统启动时运行** 和 **用户登录时运行**。

1. 启动文件夹

   如果只需要 **用户登录时运行**，最简单的就是把程序或快捷方式放入：

   ```text
   # Win + R 打开运行窗口，输入 "shell:startup"
   C:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
   ```

2. 注册表添加

   ```text
   # 用户登录时运行
   HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
   # 系统启动时运行
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
   # 在64位系统下32位程序访问上述注册表路径时默认会被系统自动映射于（此处不用管）
   HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Run
   ```

   进入对应表项后右键空白区域 --> 新建字符串值 --> 输入自启动项的名字和可执行文件的绝对地址。

3. 任务计划程序

   创建任务时可以选择 **只在用户登录时运行** 和 **不管用户是否登陆都要运行**，还可以选择 **使用最高权限运行**，即以管理员权限运行且不会跳出 UAC 弹窗。同时任务计划程序还可以在高级设置中选择延迟启动，算是 Windows 系统下对于无自启动优先级的解决办法。

## 文件资源管理器预览窗格

2024.01.28

在下载 WPS 后会发现文件资源管理器中的预览窗格查看 Word，PPT 等时变成 **无法预览** 或者 **WPS 预览**，这是因为 WPS 对注册表进行了破坏（WPS 对注册表的破坏相当多，所以非必要不安装）。如果想要改回来，请执行以下操作：

1. 打开注册表进入路径 `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\PreviewHandlers`，在右侧数据中找到你需要预览的文件类型，记录下与之对应的左侧名称。
2. 进入 `HKEY_CLASSES_ROOT\XXXXX\ShellEx\{8895b1c6-b41f-4c1c-a562-0d564250836f}`，注意将 XXXXX 替换为你要预览的文件的扩展名（如.doc），发现右侧数值项被篡改，将上一步记录的名称填入即可。
3. 此时若仍然无法预览请将去设置里将默认打开软件全部设置为 Word，PowerPoint，Excel 等。

附（本人未尝试）：如果想改回 WPS 预览，请在 WPS 配置工具中选择 **高级 --> 功能定制 --> 预览窗格**（可能需要先 **高级 --> 重置修复 --> 重新注册组件**）。

2024.07.24

WPS 此错误似乎已经得到修复（？存疑），只需将默认打开方式改为 MS Office 同时进入 **WPS Office 配置工具 --> 重置修复 --> 重新注册组件** 进行尝试。同时微软官方的 PowerToys 提供了预览 .svg, .md, .txt, .cpp, .py 等各种格式文件的功能，可以下载尝试。

## 终端编码修改

2024.06.22

1. 临时修改

   执行 `chcp 65001` 即可。
   注：65001（utf-8）936（gbk）

2. 永久修改

   进入注册表路径：`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor`
   新建 --> 字符串值 --> 数值名称 `autorun` --> 数值数据 `chcp 65001`
   以上路径为修改 CMD 的默认编码，若是想修改 pwsh，建议进入 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PowerShell` 或者 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PowerShellCore` 进行尝试，我并未实践。

## UAC 弹窗关闭

2024.08.25

[用户帐户控制 (UAC)](https://learn.microsoft.com/zh-cn/windows/security/application-security/application-control/user-account-control/) 是一项 Windows 安全功能，旨在保护操作系统免受未经授权的更改。

关闭通知方法：控制面板 --> 系统和安全 --> 安全和维护 --> 更改用户账户控制设置 --> 选择不通知

> [!note]
> 请注意这并不是关闭 UAC，仅仅关闭了通知而已，若需要彻底关闭 UAC（非常不建议！并且在此操作前请仔细浏览上面链接中微软对此功能的阐述），打开组策略编辑器 --> 计算机配置 --> Windows 设置 --> 安全设置 --> 本地策略 --> 安全选项，找到“用户账户控制：以管理员批准模式运行所有管理员”，把“已启用”改为“已禁用”，重新启动计算机。

## 启用 BBR v2

2024.10.02

转载：<https://dev.moe/2829>

BBR 是 Google 开发的 TCP 拥塞控制算法。它改善了传统拥塞控制算法（如 CUBIC）一丢包就降速的缺点，让带宽利用率在高丢包环境下大幅提升。

需要注意，拥塞控制算法控制的是 **发包速度**。如 Google 服务器开启了 BBR，其上传速度将尽可能占满用户带宽；在用户的角度看来，则体验到了更快的下载速度。

### 众所周知…

许多朋友会在自己的 Linux 服务器上，用几句命令轻松开启 BBR：

```bash
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```

于是，当他们从服务器下载文件时，服务器就会尽可能用满带宽。可当他们上传文件时，速度却依旧很慢。这是因为他们上传所用的系统，仍在使用传统的拥塞控制算法。

如果用户使用的是 Linux 系统，用同样的步骤开启 BBR 即可。而对于 Windows 系统，如果是 Windows 11 22H2 以上版本，Microsoft 也 [添加了](https://datatracker.ietf.org/meeting/112/materials/slides-112-iccrg-an-update-on-rledbat-and-bbrv2-00) BBRv2 的支持。

### 在 Windows 11 下启用 BBR / BBR v2

打开 PowerShell（管理员），先查看一下当前的拥塞控制算法：

```powershell
NetTCPSetting | Select SettingName, CongestionProvider
```

然后开启 BBRv2：

```powershell
netsh int tcp set supplemental template=Internet congestionprovider=BBR2
netsh int tcp set supplemental template=InternetCustom congestionprovider=BBR2
netsh int tcp set supplemental template=Datacenter congestionprovider=BBR2
netsh int tcp set supplemental template=DatacenterCustom congestionprovider=BBR2
netsh int tcp set supplemental template=Compat congestionprovider=BBR2
```

> 此处也可以将 `BBR2` 替换为 `BBR`（BBR v1），有兴趣的朋友可以测试比较一下效果。

再次输入最开始的命令，确认已成功开启。

### 启用后…

无需重启系统，单线程上传速度从 10Mbps 提升到了 30Mbps。推流及访问网页等场景的体验也获得了提升。

> **2024-04 更新：在新版 Windows 11 下，启用 BBR v2 可能会造成本地 TCP 连接无法使用（如导致 adb 卡住无法连接等），此时可以用以下命令还原拥塞控制算法：**
>
> ```powershell
> netsh int tcp set supplemental template=Internet congestionprovider=CUBIC
> netsh int tcp set supplemental template=InternetCustom congestionprovider=CUBIC
> netsh int tcp set supplemental template=Datacenter congestionprovider=CUBIC
> netsh int tcp set supplemental template=DatacenterCustom congestionprovider=CUBIC
> netsh int tcp set supplemental template=Compat congestionprovider=NewReno
> ```
>
> 还原后无需重启，故障应立即消失。可以在下次需要加速上传时再临时开启 BBR。

## 260 字符路径长度限制

2024.10.11

由官方文档 [最大路径长度限制](https://learn.microsoft.com/zh-cn/windows/win32/fileio/maximum-file-path-limitation) 可知从 Windows 10 版本 1607 开始，许多常见的 Win32 文件和目录函数中删除了 **MAX_PATH** 限制。 要为每个应用程序启用新的长路径行为，必须满足以下两个条件：

1. 设置注册表值

   注册表项 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem LongPathsEnabled (Type: REG_DWORD)` 必须存在并设置为 `1`。然后重启系统即可。
   还可以在 `Computer Configuration > Administrative Templates > System > Filesystem > Enable Win32 long paths` 中通过 **组策略** 控制此注册表设置。

2. 应用程序清单包含 `longPathAware` 元素

   此步骤一般应由开发者完成，即应用本身需支持长路径，具体内容可见 [应用程序清单](https://learn.microsoft.com/zh-cn/windows/win32/sbscs/application-manifests)，在 XML 文件中启用 `longPathAware` 属性。

## 笔记本开机正常运行但是黑屏

2025.02.11

- 释放静电：

  关机状态下，拔掉电源适配器，移除全部外接设备（包括电源适配器），按住电源键并保持 30 秒再松开，等待几秒插电源适配器，开机观察。

- BIOS 硬重置：

  关机状态下，同时按 <kbd> win </kbd> + <kbd> V </kbd> 并保持按住的状态，按电源键开机，<kbd> win </kbd> + <kbd> V </kbd> 不要松开。等待一分钟，如果开机有显示，并提示 502 错误，就表示重置成功。之后如果原来是 UEFI 引导的系统，就可以直接进入系统。

- 执行 BIOS Recovery：

  关机状态下，同时按 <kbd> win </kbd> + <kbd> B </kbd> 并保持按住的状态，按电源键开机，计算机通电 3 ~ 5 秒后松开。可以看到 <kbd> Caps Lock </kbd> 提示灯闪烁数次。如果 BIOS 恢复程序开始执行，会进入到 UEFI 的 BIOS 恢复界面，<kbd> Caps Lock </kbd> 提示灯会闪烁。BIOS 恢复一般不超过 5 分钟，屏幕会提示 BIOS 恢复成功。

## OMEN 手动更新 BIOS

2025.02.11

> 以下内容仅在本人电脑暗影精灵 8 Plus（OMEN by HP Laptop 17-ck1xxx）实践并成功，理论对所有 OMEN 甚至 HP 都有效，对其他电脑不负任何责任。

进入 HP 官网下载最新 BIOS，安装时直接选择第一项 **Update** 容易出现错误提示：

> [!warning]
> There is not enough free disk space to complete the System BIOS and Device Firmware update operation. Delete some files to free disk space then retry again.

由于 BIOS 是安装在 EFI 系统分区中，许多电脑默认只分配 100 MB 导致空间不足，有以下两种方法解决（推荐第二种）：

1. 借助 DiskGenius 之类的磁盘管理工具扩展 EFI 系统分区。
2. 将一个容量小于等于 32GB 的 U 盘格式化为 FAT32 格式，卷名改为 HP_TOOLS；或者直接选择第二项 **Create Recovery USB flash drive**。然后插着 U 盘重新运行 BIOS 更新程序。
