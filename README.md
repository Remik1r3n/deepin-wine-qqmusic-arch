在Archlinux及衍生发行版上运行QQ音乐
========

<p align="center">
  </a>
  <a href="https://github.com/Lapis-Apple/deepin-wine-qqmusic-arch/releases">
    <img src="https://img.shields.io/github/downloads/Lapis-Apple/deepin-wine-qqmusic-arch/total?logo=github&style=flat-square" alt="GitHub Release">
  </a>
  <a href="https://github.com/Lapis-Apple/deepin-wine-qqmusic-arch/issues">
    <img src="https://img.shields.io/github/issues/Lapis-Apple/deepin-wine-qqmusic-arch?logo=github&style=flat-square" alt="GitHub Issues">
  </a>
</p>

Deepin 打包的 QQ音乐 容器移植到 Archlinux，不依赖 `deepin-wine5`，包含定制的注册表配置。

<!-- TOC -->

- [安装](#安装)
    - [从AUR安装](#从aur安装)
    - [用安装包安装](#用安装包安装)
    - [本地打包安装](#本地打包安装)
- [设置](#设置)
- [兼容性记录](#兼容性记录)
- [切换到 `deepin-wine`](#切换到-deepin-wine)
    - [自动切换(推荐)](#自动切换)
- [卸载](#卸载)
- [常见问题及解决](#常见问题及解决)
    - [窗口阴影不总是跟随窗口](#不能记住密码)
    - [网络连接状态改变后不能重连](#网络连接状态改变后不能重连)
    - [高分辨率屏幕支持](#高分辨率屏幕支持)
    - [GNOME 桌面上的悬浮窗口问题](#gnome-桌面上的悬浮窗口问题)
    - [使用其他字体](#使用其他字体)
- [感谢](#感谢)

<!-- /TOC -->

## 安装

`deepin-wine-qqmusic` 依赖`Multilib`仓库中的 `wine`，`wine-gecko` 和 `wine-mono`，Archlinux默认没有开启 `Multilib`仓库，需要编辑`/etc/pacman.conf`，取消对应行前面的注释([Archlinux wiki](https://wiki.archlinux.org/index.php/Official_repositories#multilib)):

```diff
# If you want to run 32 bit applications on your x86_64 system,
# enable the multilib repositories as required here.

#[multilib-testing]
#Include = /etc/pacman.d/mirrorlist

-#[multilib]
-#Include = /etc/pacman.d/mirrorlist
+[multilib]
+Include = /etc/pacman.d/mirrorlist
```

以下三种安装方式效果相同，选择一种即可

### 从AUR安装

已添加到 AUR [deepin-wine-qqmusic](https://aur.archlinux.org/packages/deepin-wine-qqmusic/)，可使用 `yay` 或 `yaourt` 安装：

```shell
yay -S deepin-wine-qqmusic
```

### 用安装包安装

在 [GitHub Release](https://github.com/Lapis-Apple/deepin-wine-qqmusic-arch/releases) 页面下载后缀为 `.pkg.tar.xz` 或 `.pkg.tar.zst` 的安装包，使用`pacman`安装：

```bash
sudo pacman -U #下载的包名
```

`.md5` 文件用于校验包完整性：

```bash
md5sum -c *.md5
```

### 本地打包安装

```shell
 git clone https://github.com/Lapis-Apple/deepin-wine-qqmusic-arch.git

 cd deepin-wine-qqmusic-arch
  
 makepkg -si
```

用上述三种安装方式之一安装完成后，运行应用菜单中创建的 QQ音乐 快捷方式即可。

## 设置

dpi，目录映射等可以在 `winecfg` 进行设置，打开 `winecfg` 的命令为：

```bash
/opt/apps/com.qq.music.deepin/files/run.sh winecfg
```

## 兼容性记录

暂未整理 :(

## 切换到 `deepin-wine`

> 根据 [deepin-wine-wechat-arch#15](https://github.com/countstarlight/deepin-wine-wechat-arch/issues/15#issuecomment-515455845)，[deepin-wine-wechat-arch#27](https://github.com/countstarlight/deepin-wine-wechat-arch/issues/27)，由 [@feileb](https://github.com/feileb), [@violetbobo](https://github.com/violetbobo), [@HE7086](https://github.com/HE7086)提供的方法

原版 `wine` 在 [DDE(Deepin Desktop Environment)](https://www.deepin.org/dde/) 上，有托盘图标无法响应鼠标事件([deepin-wine-tim-arch#21](https://github.com/countstarlight/deepin-wine-tim-arch/issues/21))的问题，以及窗口阴影可能不正常的问题，可以选择切换到 `deepin-wine5`。

### 自动切换(推荐)

```bash
/opt/apps/com.qq.music.deepin/files/run.sh -d
```

这会安装需要的依赖并自动切换到 `deepin-wine5`。

> 该命令会切换到 AUR 仓库：[deepin-wine5](https://aur.archlinux.org/packages/deepin-wine5)


切换回 `wine`：

```bash
rm $HOME/.deepinwine/Deepin-QQMusic/deepin
```

如果要卸载自动安装的依赖：

```bash
sudo pacman -Rns deepin-wine5
```

**注意：切换到 `deepin-wine` 后，对 `wine` 的修改，如更改dpi，都改为对 `deepin-wine` 的修改**

## 卸载

无论用何种方式安装，卸载都是：

```bash
sudo pacman -Rns deepin-wine-qqmusic
```

卸载的同时会删除用户目录下的整个 `WINEPREFIX` 环境，路径为：`~/.deepinwine/Deepin-QQMusic`

QQ音乐在本地保存的数据不会被删除，如下载的音乐。

## 常见问题及解决

### 窗口阴影不总是跟随窗口

参照[切换到 `deepin-wine`](#切换到-deepin-wine) 解决

### 网络连接状态改变后不能重连

参照[切换到 `deepin-wine`](#切换到-deepin-wine) 解决

### 高分辨率屏幕支持

参照[设置](#设置)打开 `winecfg` ，在选项卡 `Graphics` 中修改dpi，如 修改为`192`

### GNOME 桌面上的悬浮窗口问题

> 根据 [deepin-wine-tim-arch#2](https://github.com/countstarlight/deepin-wine-tim-arch/issues/2)，由[EricDracula](https://github.com/EricDracula)提供的方法

安装 GNOME 插件: [TopIcons Plus](https://extensions.gnome.org/extension/1031/topicons/)

### 使用其他字体

默认使用 `Noto Sans CJK SC` 字体，可自行更改注册表更换。

## 感谢

* [Wuhan Deepin Technology Co.,Ltd.](http://www.deepin.org/)

* [@wszqkzqk](https://github.com/wszqkzqk) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/wszqkzqk/wszqkzqk-deepin-wine-tim-arch)

* [@ssfdust](https://github.com/ssfdust) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/ssfdust/wszqkzqk-deepin-wine-tim-arch)

* [@countstarlight](https://github.com/countstarlight) 的 [deepin-wine-qq-arch](https://github.com/countstarlight/deepin-wine-qq-arch)

