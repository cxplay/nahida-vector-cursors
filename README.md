# 纳西妲概念矢量指针

![banner](./assets/og-banner.webp)

一套以纳西妲的概念元素为灵感设计的指针资源, 以 Windows 为目标平台适配.

> 相关讨论: [直接使用矢量资源的指针方案的可能性 · Issue \#11 · SamToki/Sam-Toki-Mouse-Cursors][pre-discussion]

> 以下为 Windows 矢量指针资源预览 (原始资源为垂直翻转).

|                           正常选择                           |                           文本选择                           |                           链接选择                           |                              忙                              |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="./windows/arrow.svg" alt="正常选择" title="正常选择" style="transform: scaleY(-1); cursor: default;"> | <img src="./windows/ibeam.svg" alt="文本选择" title="文本选择" style="transform: scaleY(-1); cursor: text;"> | <img src="./windows/link.svg" alt="链接选择" title="链接选择" style="transform: scaleY(-1); cursor: pointer;"> | <img src="./windows/wait.svg" alt="忙" title="忙" style="transform: scaleY(-1); cursor: wait;"> |

|                         垂直调整大小                         |                         水平调整大小                         |                      沿对角线调整大小1                       |                      沿对角线调整大小2                       |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="./windows/ns.svg" alt="垂直调整大小" title="垂直调整大小" style="transform: scaleY(-1); cursor: n-resize;"> | <img src="./windows/ew.svg" alt="水平调整大小" title="水平调整大小" style="transform: scaleY(-1); cursor: ew-resize;"> | <img src="./windows/nwse.svg" alt="沿对角线调整大小1" title="沿对角线调整大小1" style="transform: scaleY(-1); cursor: nw-resize;"> | <img src="./windows/nesw.svg" alt="沿对角线调整大小2" title="沿对角线调整大小2" style="transform: scaleY(-1); cursor: ne-resize;"> |

| 移动 | 不可用 | 精确选择 | 备用选择 | 笔 |
|:-:   |:-:     |:-:       |:-: |:-:       |
|<img src="./windows/move.svg" alt="移动" title="移动" style="transform: scaleY(-1); cursor: move;">|<img src="./windows/unavail.svg" alt="不可用" title="不可用" style="transform: scaleY(-1); cursor: no-drop;">|<img src="./windows/cross.svg" alt="精确选择" title="精确选择" style="transform: scaleY(-1); cursor: crosshair;">|<img src="./windows/up.svg" alt="备用选择" title="备用选择" style="transform: scaleY(-1);">|<img src="./windows/pen.svg" alt="笔" title="笔" style="transform: scaleY(-1);">|

| 在后台工作 | 帮助选择 | 位置选择 | 个人选择 |
|:-:           |:-:           |:-:                |:-:                |
|<img src="./windows/busy.svg" alt="在后台工作" title="在后台工作" style="transform: scaleY(-1); cursor: progress;">|<img src="./windows/helpsel.svg" alt="帮助选择" title="帮助选择" style="transform: scaleY(-1); cursor: help;">|<img src="./windows/pin.svg" alt="位置选择" title="位置选择" style="transform: scaleY(-1);">|<img src="./windows/person.svg" alt="个人选择" title="个人选择" style="transform: scaleY(-1);">|

## 安装方式

最新版 Windows 中, 当用户在设置中的 "辅助功能" - "鼠标指针与触控" 将鼠标指针 "大小" 滑块调整到 2 倍及以上时, Windows 会开始切换到矢量指针资源, 通过替换这套矢量资源我们可以做到自定义矢量指针.

这套矢量指针资源位于 `C:\Windows\Cursors` 目录之下, 使用标准 SVG 格式保存, 权限属性的所有者为系统用户 `TrustedInstaller`, 这使得常规管理员用户无法通过常规手段修改这些资源文件, 所以需要通过提权工具来获取 `TrustedInstaller` 权限进行操作.

> [!NOTE]
> 虽然可以强行将这些待替换的资源文件的所有者修改为 `Administrator` 用户组, 但无法保证系统文件权限被破坏的情况下一定不会对未来的系统运行造成影响.

### 获取资源

打开 PowerShell 运行以下命令获取最新指针资源:

```powershell
Invoke-WebRequest https://github.com/cxplay/nahida-vector-cursors/releases/latest/download/windows.zip -OutFile windows.zip; `
Expand-Archive -DestinationPath . windows.zip; `
Remove-Item windows.zip
```

最新的指针资源将会解压到 windows 文件夹中.

### 提权会话

在本例中, 我们将会使用开源提权工具 [NanaRun][1] 进行权限提升.

打开 PowerShell 运行以下命令获取 NanaRun:

> [!CAUTION]
> 由于提权工具存在较高滥用风险, 请确保反病毒软件不会影响提权工具的获取和运行, 并且确保运行的提权工具是从可信的来源获取的.

```powershell
Invoke-WebRequest https://github.com/M2Team/NanaRun/releases/download/1.0.92.0/NanaRun_1.0_Preview3_1.0.92.0.zip -OutFile nanarun.zip; `
Expand-Archive nanarun.zip; `
Remove-Item nanarun.zip
```

使用 NanaRun 以 `TrustedInstaller` 身份运行一个新的 PowerShell 会话:

```powershell
.\nanarun\x64\MinSudo.exe --NoLogo ---TrustedInstaller powershell
```

运行以下 PowerShell 命令获取当前用户身份:

```powershell
whoami
```

如果终端输出以下结果, 那么说明我们的提权操作已经成功了:

```powershell
nt authority\system
```

### 替换指针

在提权会话下, 运行以下命令将已解压的指针资源复制到 `C:\Windows\Cursors` 目录, 替换同名资源:

```powershell
Copy-Item .\windows\* C:\Windows\Cursors
```

### 刷新指针

打开 Windows 设置中的 “辅助功能” - “鼠标指针与触控”, 在 "鼠标指针样式" 选项中切换一次指针样式刷新资源, 并确保指针 "大小" 滑块已经设置为 2 倍或以上缩放尺寸.

## 版权许可

项目部分指针设计来自 [SamToki/Sam-Toki-Mouse-Cursors][2], 本项目设计资产在 [CC BY-NC-SA 4.0](./LICENCE.txt) 许可下重新发行.

[1]: https://github.com/M2Team/NanaRun
[2]: https://github.com/SamToki/Sam-Toki-Mouse-Cursors
[pre-discussion]: https://github.com/SamToki/Sam-Toki-Mouse-Cursors/issues/11
