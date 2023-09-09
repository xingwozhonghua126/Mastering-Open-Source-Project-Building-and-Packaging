# 第3章 Archlinux系统安装及源码打包

## 1.1 Archlinux简介

Arch Linux是一款为高级用户设计的自由开放源代码的Linux发行版。它的设计理念是“Keep It Simple, Stupid”（KISS原则），主张用户控制自己的操作系统。Arch Linux提供了一个最小化的基础系统，让用户可以从这个基础系统上，自由、快捷、灵活地建立出适合自己的系统环境。

Arch Linux并不像一些其他Linux发行版那样，提供一个在安装过程中就已经配置好的图形界面。在Arch Linux中，所有的配置工作都需要用户自行进行，这就要求用户对Linux系统有较深入的了解。

在Arch Linux下，软件安装和升级非常简便，主要通过其自家的软件包管理器pacman来完成。用户可以通过它轻松地对系统进行全面升级或安装、卸载软件包。

Arch Linux用户社区非常活跃，提供了大量的文档资料和论坛讨论供用户参考学习，如Arch维基等。

## 1.2 在VMware中安装Archlinux

在 VMware 中安装 Archlinux，你可以遵循以下步骤：

1. 首先，你需要下载最新版本的 Arch Linux ISO，你可以在 https://archlinux.org/download/ 找到它。

2. 打开 VMware，然后在 VMware 中创建一个新的虚拟机。你可以点击 "File" -> "New Virtual Machine..."，然后选择 "Custom (advanced)"。

3. 接着，你需要设置你的虚拟机的各种配置，比如处理器数量、内存大小、硬盘大小等，这取决于你的需要。对于操作系统类型，选择 “Linux”，版本选择 "Other Linux 3.x kernel 64-bit"（如果有的话，也可以选择更接近 Arch Linux 的选项）。

4. 然后，在创建 VM 过程的 "CD/DVD (IDE)" 部分，选择 "Use ISO image file" 然后定位到你先前下载的 Arch Linux ISO 文件。

5. 完成虚拟机的创建，然后运行虚拟机，你会看到 Arch Linux 的启动菜单。选择 "Boot Arch Linux (x86_64)"，然后按回车键。

6. 然后你需要设置磁盘分区，你可以使用 cfdisk 或 fdisk 等工具。比如，你可以键入 "cfdisk" 来启动磁盘分区器。然后创建一个主分区，然后选择 write 来写入分区表。

7. 创建文件系统。例如，如果你的硬盘是 /dev/sda，且你创建了一个 sda1 的分区，就可以使用 "mkfs.ext4 /dev/sda1" 来格式化你的硬盘。

8. 挂载分区。例如你可以键入 "mount /dev/sda1 /mnt" 来挂载你的硬盘。

9. 安装基础系统。你可以键入 "pacstrap /mnt base linux linux-firmware vim" 来安装基础系统。

10. 配置新系统。使用 "genfstab -U /mnt >> /mnt/etc/fstab" 来生成 fstab 文件。

11. 你可以使用 "arch-chroot /mnt" 切换到新系统。接着，给新系统设置语言、时间、主机名字等。

12. 安装一个引导器。例如你可以安装 GRUB，键入 "pacman -S grub" 安装，"grub-install --target=i386-pc /dev/sda" 安装到硬盘，"grub-mkconfig -o /boot/grub/grub.cfg" 生成配置。

以上大致是在 VMware 中安装 Arch Linux 的过程，但是根据你的实际需要，你可能还需要配置网络、安装更多的软件等。至于每个步骤的详细操作，建议你查阅 Arch Linux 的官方维基文档。

## 1.3 Archlinux安装KDE及Fcitx5

在Arch Linux中安装KDE和Fcitx5需要按照以下步骤进行：

首先，如果你还没有安装图形界面，你需要安装xorg。在终端中运行以下命令：
```
sudo pacman -S xorg
```

1. **安装KDE：**

在Arch Linux上安装KDE Plasma桌面环境，你需要运行以下命令：

```
sudo pacman -S plasma
```

你也可能需要安装一些常用KDE应用程序，这可以通过安装kde-applications软件包来实现：

```
sudo pacman -S kde-applications
```

安装完成后，通过启用display manager，如SDDM，来启动KDE桌面环境。首先安装SDDM：

```
sudo pacman -S sddm
```

然后启用SDDM：

```
sudo systemctl enable sddm
```

你现在可以重启电脑，应该可以看到KDE桌面环境了。

2. **安装Fcitx5：**

为了在Arch Linux上安装Fcitx5，你可以运行以下命令：

```
sudo pacman -S fcitx5
```

然后安装你需要的输入法，如fcitx5-chinese-addons（对于中文输入）：

```
sudo pacman -S fcitx5-chinese-addons
```

然后，你需要配置你的环境以使用Fcitx5。在你的 bashrc 或 zshrc 文件中添加以下行（如果你的shell是bash或zsh）：

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

重新启动你的终端或者重新启动你的桌面环境，现在你应该可以用Fcitx5输入了。

牢记，确保你的系统是最新的，可以通过以下命令更新系统：

```
sudo pacman -Syu
```

## 1.3 Archlinux 上 Fcitx5 的编译和打包

Fcitx5 是一款优秀的输入法框架，并且 Arch Linux 是一款注重用户自定义和灵活性的 Linux 发行版。在 PACMAN（Arch Linux 的包管理器）中没有预打包的 Fcitx5 的话，可以自己从源码编译并打包 Fcitx5。Arch Linux 的 AUR（Arch User Repository）中有很多社区维护的 PKGBUILD，用以编译打包一些未收录在官方仓库中的软件。

PKGBUILD 是一个 shell脚本，定义了一组变量和函数，使得 makepkg 脚本能够生成一个Arch Linux可以使用的包文件(.pkg.tar.xz)。在给定的链接中，PKGBUILD 文件包含了如何打包 Fcitx5。

以下是一些基本步骤介绍如何编译和打包 Fcitx5。

1. 先安装所需要的依赖。在PKGBUILD文件中，`depends`部分列出了运行时依赖，`makedepends`部分列出了编译时依赖。确认所有依赖已安装。

2. 克隆 git 仓库。在终端中，使用 `git clone` 命令克隆仓库。

    ```
    git clone https://gitlab.archlinux.org/archlinux/packaging/packages/fcitx5.git
    ```

3. 进入仓库目录。使用 `cd` 命令，例如：

    ```
    cd fcitx5
    ```

4. 生成包文件。在仓库目录中使用 makepkg 命令编译打包源码：

    ```
    makepkg -si
    ```

   `-si` 选项表示在生成 pkg.tar.xz 文件后立即安装编译出来的包。

5. 如果没有问题，Fcitx5 就成功安装到系统中了。

以上就是在 Arch Linux 上编译和打包 Fcitx5 的基本步骤，具体问题还需要根据实际情况解决。注意保持系统的更新，并了解包管理和编译的相关知识。祝你编译顺利！