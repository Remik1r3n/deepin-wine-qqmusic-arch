在Archlinux及衍生发行版上运行QQ音乐
========

<p align="center">
  <a href="https://travis-ci.org/Lapis-Apple/deepin-wine-qqmusic">
    <img src="https://img.shields.io/travis/Lapis-Apple/deepin-wine-qqmusic?&logo=travis&style=flat-square" alt="Build Status">
  </a>
  <a href="https://im.qq.com/download/">
    <img src="https://img.shields.io/badge/QQ-9.4.3.27712-blue?style=flat-square&logo=tencent-qq" alt="QQ Version">
  </a>
  <a href="https://aur.archlinux.org/packages/deepin-wine-qq/">
    <img src="https://img.shields.io/aur/version/deepin-wine-qq?label=AUR&logo=arch-linux&style=flat-square" alt="AUR Version">
  </a>
  <a href="https://github.com/Lapis-Apple/deepin-wine-qqmusic/releases">
    <img src="https://img.shields.io/github/downloads/Lapis-Apple/deepin-wine-qqmusic/total?logo=github&style=flat-square" alt="GitHub Release">
  </a>
  <a href="https://github.com/Lapis-Apple/deepin-wine-qqmusic/issues">
    <img src="https://img.shields.io/github/issues/Lapis-Apple/deepin-wine-qqmusic?logo=github&style=flat-square" alt="GitHub Issues">
  </a>
</p>

Deepin 打包的 QQ音乐 容器移植到 Archlinux，可不依赖 `deepin-wine5`。

<!-- TOC -->

- [安装](#安装)
    - [从AUR安装](#从aur安装)
    - [用安装包安装](#用安装包安装)
    - [本地打包安装](#本地打包安装)
- [设置](#设置)
- [兼容性记录](#兼容性记录)
- [切换到 `deepin-wine`](#切换到-deepin-wine)
    - [自动切换(推荐)](#自动切换推荐)
    - [从 `deepin-wine 2.x` 迁移](#从-deepin-wine-2x-迁移)
- [卸载](#卸载)
- [常见问题及解决](#常见问题及解决)
    - [不能记住密码](#不能记住密码)
    - [网络连接状态改变后不能重连](#网络连接状态改变后不能重连)
    - [高分辨率屏幕支持](#高分辨率屏幕支持)
    - [GNOME 桌面上的悬浮窗口问题](#gnome-桌面上的悬浮窗口问题)
    - [使用其他字体](#使用其他字体)
- [感谢](#感谢)
- [更新日志](#更新日志)

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

**注意：由于新版QQ音乐可能需要 `wine` 还没有实现的一些win api，这会导致一些功能不可用，安装前先根据[兼容性记录](#兼容性记录)选择一个合适的版本**

以下三种安装方式效果相同，选择一种即可

### 从AUR安装

暂时还没上aur.

### 用安装包安装

还没打。

```bash
sudo pacman -U #下载的包名
```

`.md5` 文件用于校验包完整性：

```bash
md5sum -c *.md5
```

### 本地打包安装

咕咕咕。

## 设置

dpi，目录映射等可以在 `winecfg` 进行设置，打开 `winecfg` 的命令为：

```bash
/opt/apps/com.qq.im.deepin/files/run.sh winecfg
```

## 兼容性记录

暂未整理。

## 切换到 `deepin-wine`

> 根据 [deepin-wine-wechat-arch#15](https://github.com/countstarlight/deepin-wine-wechat-arch/issues/15#issuecomment-515455845)，[deepin-wine-wechat-arch#27](https://github.com/countstarlight/deepin-wine-wechat-arch/issues/27)，由 [@feileb](https://github.com/feileb), [@violetbobo](https://github.com/violetbobo), [@HE7086](https://github.com/HE7086)提供的方法

原版 `wine` 在 [DDE(Deepin Desktop Environment)](https://www.deepin.org/dde/) 上，有托盘图标无法响应鼠标事件([deepin-wine-tim-arch#21](https://github.com/countstarlight/deepin-wine-tim-arch/issues/21))的问题，且原版 `wine` 尚不能实现保存登录密码等功能，可以选择切换到 `deepin-wine5`。

### 自动切换(推荐)

```bash
/opt/apps/com.qq.im.deepin/files/run.sh -d
```

这会安装需要的依赖，移除已安装的QQ音乐目录并回退对注册表文件的修改。

> 从 `v9.4.2.27655-1` 开始，该命令会切换到 AUR 仓库：[deepin-wine5](https://aur.archlinux.org/packages/deepin-wine5)


切换回 `wine`：

```bash
rm $HOME/.deepinwine/Deepin-QQ/deepin
```

如果要卸载自动安装的依赖：

```bash
sudo pacman -Rns deepin-wine5
```

**注意：切换到 `deepin-wine` 后，对 `wine` 的修改，如更改dpi，都改为对 `deepin-wine` 的修改**

## 卸载

无论用何种方式安装，卸载都是：

```bash
sudo pacman -Rns deepin-wine-qq
```

卸载的同时会应该删除用户目录下的整个 `WINEPREFIX` 环境，路径为：`~/.deepinwine/Deepin-QQMusic`



## 常见问题及解决

### 网络连接状态改变后不能重连

参照[切换到 `deepin-wine`](#切换到-deepin-wine) 解决

### 高分辨率屏幕支持

参照[设置](#设置)打开 `winecfg` ，在选项卡 `Graphics` 中修改dpi，如 修改为`192`

### GNOME 桌面上的悬浮窗口问题

> 根据 [deepin-wine-tim-arch#2](https://github.com/countstarlight/deepin-wine-tim-arch/issues/2)，由[EricDracula](https://github.com/EricDracula)提供的方法

安装 GNOME 插件: [TopIcons Plus](https://extensions.gnome.org/extension/1031/topicons/)

### 使用其他字体

默认使用文泉驿微米黑(`wqy-microhei`)字体，可以使用Windows平台常用字体替代，直接将字体文件或字体链接文件放置到字体文件夹就会生效，不会影响系统字体

字体文件夹在：`$HOME/.deepinwine/Deepin-QQ/drive_c/windows/Fonts`

## 感谢

* [Wuhan Deepin Technology Co.,Ltd.](http://www.deepin.org/)

* [@wszqkzqk](https://github.com/wszqkzqk) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/wszqkzqk/wszqkzqk-deepin-wine-tim-arch)

* [@ssfdust](https://github.com/ssfdust) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/ssfdust/wszqkzqk-deepin-wine-tim-arch)

## 更新日志

还没写
