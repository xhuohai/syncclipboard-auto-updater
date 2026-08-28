# SyncClipboard Desktop Bin（AUR）

这是 [SyncClipboard](https://github.com/Jeric-X/SyncClipboard) Linux 桌面版的 Arch Linux AUR 打包仓库。

本仓库使用上游发布的 `SyncClipboard_linux_x64.AppImage` 构建二进制包 `syncclipboard-desktop-bin`，并通过 GitHub Actions 定期检查上游版本，在发现新版本后自动更新 `PKGBUILD`、校验和以及 AUR 发布内容。

> SyncClipboard 本身是跨平台的剪贴板同步与历史记录管理工具。本仓库只负责 Arch Linux 软件包的构建和发布，不包含 SyncClipboard 的源代码。

## 软件包信息

| 项目 | 内容 |
| --- | --- |
| 软件包 | `syncclipboard-desktop-bin` |
| 上游项目 | [Jeric-X/SyncClipboard](https://github.com/Jeric-X/SyncClipboard) |
| 打包来源 | 上游 Linux x64 AppImage |
| 支持架构 | `x86_64` |
| 许可证 | MIT |
| 安装后的启动命令 | `SyncClipboard.Desktop.Default` |

## 安装

### 使用 AUR helper

如果已安装 `yay` 或其他 AUR helper，可以直接执行：

```bash
yay -S syncclipboard-desktop-bin
```

也可以使用 `paru`：

```bash
paru -S syncclipboard-desktop-bin
```

### 手动从 AUR 安装

```bash
git clone https://aur.archlinux.org/syncclipboard-desktop-bin.git
cd syncclipboard-desktop-bin
makepkg -si
```

安装完成后，可以从桌面应用菜单启动 SyncClipboard，也可以在终端执行：

```bash
SyncClipboard.Desktop.Default
```


## 自动更新流程

`.github/workflows/update_and_pack_syncclipboard.yml` 默认每 72 小时运行一次，也支持手动触发。流程大致如下：

1. 在 `archlinux:base-devel` 容器中检查环境；
2. 使用 `nvchecker` 检查 GitHub 上游的最新 Release；
3. 发现新版本时更新 `PKGBUILD` 中的 `pkgver`，并将发布版本重置为 `pkgrel=1`；
4. 使用 `updpkgsums` 更新源文件校验和；
5. 使用普通用户运行 `makepkg` 验证软件包可以正常构建和安装；
6. 重新生成 `.SRCINFO`；
7. 将更新后的 `PKGBUILD` 和 `.SRCINFO` 推送到 AUR；
8. 将版本检查状态提交回本仓库。

自动发布到 AUR 需要在 GitHub 仓库中配置名为 `AUR_SSH_PRIVATE_KEY` 的 Actions Secret。该密钥应当是具有目标 AUR 软件包推送权限的 SSH 私钥，不能提交到仓库或写入 `PKGBUILD`。

## 文件说明

```text
.
├── PKGBUILD                         # Arch Linux 软件包构建脚本
├── .SRCINFO                         # AUR 使用的软件包元数据
├── nvchecker.toml                   # 上游版本检查配置
├── nvchecker_oldver.json            # nvchecker 保存的已知版本状态
└── .github/workflows/
    └── update_and_pack_syncclipboard.yml    # 自动检查、构建和发布流程
```


## 软件包冲突

该软件包声明会与以下同名或相关软件包冲突：

- `syncclipboard`
- `syncclipboard-bin`
- `SyncClipboard.Desktop`
- `syncclipboard-desktop-bin`

同一系统中请根据需要保留一种 SyncClipboard Desktop 安装方式。

## 反馈与上游支持

- 软件包构建、AUR 发布或自动更新问题：请在本仓库提交 Issue。
- SyncClipboard 功能、同步协议或应用本身的问题：请前往上游项目的 [Issues](https://github.com/Jeric-X/SyncClipboard/issues) 页面反馈。

## 许可证

本打包配置遵循 MIT 许可证。上游许可证会随软件包安装到：

```text
/usr/share/licenses/syncclipboard-desktop-bin/LICENSE
```

详见仓库中的 `PKGBUILD` 及上游项目的 [LICENSE](https://github.com/Jeric-X/SyncClipboard/blob/master/LICENSE)。
