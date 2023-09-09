# 第1章 Debian系统安装及源码打包

## 1.1 准备

接下来，我们将学习如何安装Debian系统。首先，让我们简单了解一下Debian。Debian是一款自由和开源的操作系统，其中包含了众多种类的软件。它最早于1993年发布，由Ian Murdock创立，以Debian社区开发的GNU/Linux发行版最为著名。它以其稳定性，安全性和强大的软件包管理系统著称，广泛应用在服务器和桌面系统中。

对于初学者，理解和安装操作系统可能会感觉困难，因此，为了更加友好地对初学者进行指导，我首先会使用虚拟机进行模拟教学。请放心，我将在后续的章节中详细解释如何在真实的物理机器上进行安装。我假设您当前使用的物理机操作系统是Windows。我们可以使用VMware和VirtualBox中的任何一个作为虚拟机软件。我更倾向于推荐使用VMware。虽然VirtualBox是一款开源软件，但VMware对开源系统的支持通常更为强大，驱动包也被包含在大多数主流操作系统中。

## 1.2 安装VMware

安装VMware Workstation的步骤：

下载VMware Workstation：首先，你需要下载VMware Workstation。你可以从[VMware的官方网站](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)下载试用版或购买正版。

启动安装程序：下载完毕后，找到你的下载文件，然后双击运行安装程序。

选择安装类型：启动安装程序后，你会看到一个欢迎屏幕。点击“下一步”。然后，你会被要求选择典型安装或自定义安装。对于大多数用户，典型安装足够了。选择你的安装类型，然后点击“下一步”。

许可协议：阅读并接受许可协议，然后点击“下一步”。

自定义设置：在这一步，你可以更改安装位置，选择是否要在启动时运行VMware，以及是否要加入“客户体验改进计划”。选择你的设置，然后点击“下一步”。

安装：点击“安装”开始安装过程。安装可能需要几分钟的时间。

完成安装：安装完成后，点击“完成”关闭安装向导。

输入许可证密钥：如果你购买了VMware，你需要输入你的许可证密钥。启动VMware Workstation，然后在菜单中选择“帮助”>“输入许可证密钥”。输入你的密钥，然后点击“OK”。

现在，你已经成功安装了VMware Workstation，并且可以开始创建和运行虚拟机了。

## 1.3 在VMware中安装Debian

下一步，我们将在Vmware环境中安装Debian。Debian有三种版本可供选择：稳定版（stable）、测试版（testing）和不稳定版（unstable）。作为初学者，建议我们首先选择稳定版进行学习和熟悉。等到后面需要学习并进行开源代码的编译构建时，我们可以将系统源切换至测试版，这样可以帮助我们更容易地构建并学习最新的源代码包。

以下是在VMware中安装Debian操作系统的步骤：

第一步：下载Debian ISO
转到Debian官方网站(https://www.debian.org)，找到最新的Debian ISO文件并下载。须注意，一般需要选择"amd64"版本。

第二步：设置VMware
打开VMware并点击创建新的虚拟机。

第三步：创建新的虚拟机
现在，你需要设置新的虚拟机。选择“创建新的虚拟机”，在“选择安装媒体“界面选择你下载的Debian ISO文件。在"客户机操作系统”部分，选择"Linux"，在版本中选择"Debian"。

第四步：配置虚拟机
设置虚拟机的硬盘空间、内存和处理器数量等资源。这些设置应根据你计划在虚拟机中运行的应用程序和你的主机硬件资源进行相应的配置。

第五步：安装Debian
启动虚拟机，它将从Debian ISO文件启动。选择语言、位置和键盘布局等选项，然后进行分区设置。对于简单设置，你可以选择guided partitioning。

在安装Debian系统过程中，当你到达"选择和安装软件"的步骤时，系统会让你选择要安装哪些软件。在软件选择界面，你可以看到有多种选择，包括Debian桌面环境，Web服务器，打印服务器，SSH服务器，标准系统工具等等。

在这些选项中，选中"Debian桌面环境"和"KDE"。如果你没有看到"KDE"，请确认你至少选中了"Debian桌面环境"，这通常会包装多种桌面环境供你选择。在完成选择后，继续安装过程。系统会自动为你安装KDE桌面环境。

第六步：设置系统
设置root用户密码，创建并设置新的用户。然后配置包管理器（apt）。如果你不确定，你可以使用默认设置。

第七步：完成安装
安装任务完成后，重启你的虚拟机。在此之后，你已经可以开始使用你的Debian系统了。如果需要的话，你可以在VMware的工具菜单中安装VMware Tools，这将使得虚拟机更顺畅地运行。

总的来说，虽然安装过程可能会有所不同，但基本上，这就是在VMware中安装Debian的步骤。

## 1.4 在Debian中安装fcitx5

Debian系统中安装fcitx5的步骤如下：

打开终端。

确保你的系统是最新的，你可以通过下面的命令更新你的系统：

```
sudo apt-get update
sudo apt-get upgrade
```

安装fcitx5。用以下的命令安装fcitx5以及其相关的语言包（这里以中文为例）：

```
sudo apt-get install fcitx5 fcitx5-config-qt fcitx5-chinese-addons im-config
```

在你的会话（Session）开始程序中添加fcitx5。这个步骤取决于你的桌面环境，可能在"Startup Application settings"，"Session and Startup settings"等位置。

运行im-config选择fcitx5，配置输入法环境变量。

重启你的系统。检查fcitx5是否已经在系统托盘中出现。

配置fcitx5。点击系统托盘中的fcitx5图标，选择"Configure"。在弹出的配置界面中，你能添加，并配置你需要的输入法。

## 1.5 尝试编译从代码编译fcitx5并打包

### 1.5.1 打开debian系统源码仓库

在Debian中打开系统源码仓库的步骤如下：

打开Terminal并键入以下命令以开始编辑Debian的源列表：

```
sudo nano /etc/apt/sources.list
```

找到文件中的每个deb行，并添加一个以deb-src开始的相似行进行源码下载。例如，如果你的文件中的一个行是：

```
deb http://deb.debian.org/debian/ buster main
```

那么你需要添加以下行：

```
deb-src http://deb.debian.org/debian/ buster main
```

需要注意的是，这个链接以及分发版本（比如buster）可能会根据你的Debian版本以及你所在的地理位置而有所不同。

按下Ctrl + O保存对文件的更改，按下Ctrl + X退出编辑器。

在Terminal键入以下命令，更新你的系统以反映对源列表的更改：

```
sudo apt-get update
```

现在，你应该可以使用apt-get source命令来下载Debian软件包的源码了。例如，如果你想下载fcitx5的源码，只需键入以下命令：

```
apt-get source fcitx5
```

### 1.5.2 从Debian源码仓库编译及打包fcitx5

为了从Debian源码仓库编译fcitx5，你可以按照以下步骤进行操作：

首先，你需要安装所需的工具和库。在终端中，运行以下命令：

```
sudo apt-get update
sudo apt-get install build-essential
sudo apt-get build-dep fcitx5
```

其次，从Debian源码仓库中获取最新的fcitx5源码：

```
apt-get source fcitx5
```

进入源码文件夹：

```
cd fcitx5*
```

创建一个build目录来编译源码：

```
mkdir build
cd build
```

在build目录中，调用CMake来配置源码编译：

```
cmake ..
```

执行make命令来编译源码：

```
make
```

当然有时候我的编译的代码可能会和debian的代码有些差异。因为debian其中patch的原因。下面我用使用fcitx5源码尝试进行打包。

安装打包所需要的工具：

```
sudo apt-get install devscripts build-essential debhelper
```

从源码创建 Debian 包：

```
cd fcitx5
debuild -us -uc
```

这将在上层目录中创建 .deb 包文件，你可以使用 dpkg -i 命令来安装它。

Debian的deb包是Debian、Ubuntu以及其它基于Debian的Linux分发版中使用的软件包格式。deb包含了一款软件的所有文件，以及这款软件运行所需要的依赖关系等元信息。一旦安装，包管理器（如 APT）会处理这些依赖关系，并确保所有必要的包一起安装。deb包的名字通常采取此格式：“软件名_版本号_架构.deb”。

其中[debian系统fcitx5源码包仓库地址](https://salsa.debian.org/input-method-team/fcitx5)。[fcitx5的主仓库地址](https://github.com/fcitx/fcitx5)。对比后我们发现debian系统fcitx5仓库多了一个debian文件夹，其中debian文件夹是debian系统的打包配置文件。

## 1.6 源码包中的debian文件夹

Debian源代码包含一个名为"debian"的文件夹，它包含所有与构建Debian包相关的必要信息和工具。下面是这个文件夹里一些重要文件的简单介绍：

control: 这是一个必需的文件，包含了包的元数据，比如包名称，版本，依赖关系等等。

rules: 该文件主要是一个Makefile，它定义了一套规则以构建二进制包。

changelog: 这个文件记录的是软件包的版本历史，换句话说，它描述了每个版本的变动情况。

compat: 这个文件只包含一个数字，这是用来说明软件包所需要的debhelper（一个用于打包的辅助工具）的版本。

copyright: 这个文件包含关于软件包获取和使用的法律信息。

patches: 在这个文件夹中包含了对源代码的修改。这些修改可以是对错误的修复，或对源代码中功能的改进。

注意，debian文件夹中可能存在的其他文件和文件夹，就取决于特定的包和特定的打包需要了。例如，有时可能需要postinst、postrm、preinst和prerm等脚本，这些脚本在包安装或卸载之前或之后运行。

通过学习和比较Debian源码包的配置以及build文件夹中的内容，我们可以更深入地理解Debian包的配置规则。看到优质的开源项目时，你也可以试图创建Debian文件夹，以此来丰富Debian社区的生态环境。