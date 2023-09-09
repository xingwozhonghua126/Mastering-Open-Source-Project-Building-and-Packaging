# 第2章 Fedora系统安装及源码打包

## 1.1 Fedora简介

Fedora是一个由全球社区成员维护的、基于Linux的免费开源操作系统。它由Fedora项目组团队开发并由Red Hat赞助。Fedora被广泛用于各种环境中，包括桌面系统、服务器、内核开发、云基础架构等。

Fedora对开源软件的贡献显著，许多新的开源软件首次在Fedora中得到广泛使用。Fedora项目致力于创建一个最新、稳定且强大的操作系统，它包含并支持广泛的免费开源软件。这种对开源的支持并包含在其基础理念中，即"Freedom, Friends, Features, First"，代表"自由、朋友、特性、先行"。

Fedora以其尖端的特性，清晰的方向以及开放的开发模式而广受赞誉。它每六个月就会有一个新版本发布，以保持其技术的最新性。

## 1.2 在VMware中安装Fedora

第一步：下载Fedora ISO文件：请访问[https://getfedora.org/](https://getfedora.org/)，选择您需要的 Fedora 版本，然后下载对应的 ISO 镜像文件。

第二步：打开VMware：开启 VMware Workstation 或 VMware Player，然后点击“创建新虚拟机”。

第三步：选择安装方式：选择“典型”安装，然后点击“下一步”。

第四步：选择安装介质：选择“使用 ISO 镜像文件安装操作系统”选项，然后浏览到下载的 Fedora ISO 文件，点击“下一步”。

第五步：设置虚拟机操作系统：选择“Linux”作为“操作系统类型”，然后在选择“版本”时，选择与你下载的Fedora版本尽可能相近的版本。

第六步：虚拟机名称和位置：输入你的虚拟机的名称并选择保存的位置。

第七步：指定磁盘容量：分配虚拟硬盘的大小，根据需求选择，一般20GB足以使用。 

第八步：检查配置并完成： 点击“下一步”后，检查并确认虚拟机设定。若无问题，点击“完成”。

第九步：启动Fedora安装：新建的虚拟机会出现在左侧列表中，选中并点击“开启此虚拟机”。进入后，会进入Fedora的安装界面，按照指示完成安装。

第十步：设置Fedora：安装完成后，会进入初始设置，包括创建用户账户密码、指定语言设置等。按步骤操作即可。

第十一步：完成后，您的虚拟机中就成功安装了Fedora操作系统，你可以开始尝试使用了。

## 1.3 在Fedora中安装kde和fcitx5并正确配置

以下是在Fedora中安装KDE和Fcitx5并进行配置的基本步骤：

第一步：**安装KDE**
   
首先，你需要安装KDE环境。在终端中运行以下命令：

```sh
sudo dnf groupinstall "KDE Plasma Workspaces"
```

然后，重启电脑并在登录画面选择KDE Plasma。

第二步：**安装Fcitx5**

其次，安装Fcitx5。在终端中运行以下命令：

```sh
sudo dnf install fcitx5
```

第三步：**配置Fcitx5**

安装Fcitx5后，你需要为其创建一个配置文件。在终端中运行以下命令：

```sh
vim ~/.xprofile
```

然后，将以下内容添加到`.xprofile`文件中：

```sh
export GTK_IM_MODULE=fcitx5
export QT_IM_MODULE=fcitx5
export XMODIFIERS=@im=fcitx5
fcitx5 &
```

保存并关闭文件。

第四步：**启动Fcitx5**

最后，启动Fcitx。在终端中运行以下命令：

```sh
fcitx5 &
```

注意，你可能还需要安装和配置你需要的输入法引擎，例如 fcitx5-chinese-addons 为中文输入。

## 1.4 尝试从fedora源码仓库编译fcitx5并打包

### 1.4.1 开启 Fedora 源代码仓库

为了开启 Fedora 源代码仓库，请遵循以下步骤：

第一步： 打开终端。

第二步： 输入以下命令以打开 /etc/yum.repos.d/fedora.repo 文件：

```
sudo nano /etc/yum.repos.d/fedora.repo
```

第三步：找到 [fedora-source] 部分，将 enabled 的值从 0 更改为 1。

第四步：保存并关闭文件。

第五步：为了更新 repo 列表并同步系统，请运行：

```
sudo dnf update
```
   
请注意，需要用于获取软件包源代码的命令是 dnf download --source <package>。这将下载一个 src.rpm 文件，其中包含软件包的源代码以及用于构建二进制包的脚本。

### 1.4.2 从 Fedora 源代码仓库编译并打包 fcitx5

一般情况下，如果你想从源代码仓库获取一个 Fedora 软件包的 spec 文件，并根据它来构建一个 rpm 文件，你可以按照以下的步骤来操作：

第一步： 安装必需的软件包

开启 Linux 终端并安装以下软件：
```sh
sudo dnf install rpm-build rpmdevtools
```

第二步： 创建 RPM 构建环境

在你的主目录下设置构建环境：
```sh
rpmdev-setuptree
```

第三步：下载源代码仓库

此步骤主要针对 `fcitx5` 你需要找到对应的源代码仓库进行下载。如果它在 Fedora 的官方源中，你可以使用 `dnf` 来下载对应的源代码包。假设 `fcitx5` 在 Fedora 的官方源中：
```sh
dnf download --source fcitx5
```

第四步：解压缩源码包

解压缩下载下来的 `fcitx5` 源码包，其中会包含 spec 文件：
```sh
rpm2cpio fcitx5-*.src.rpm | cpio -idmv
```
此命令将会将源码包解压到当前目录，其中包含一个 `.spec` 文件。

第五步： 将 spec 文件复制到 rpmbuild 的 SPEC 目录中：

```sh
mv fcitx5.spec ~/rpmbuild/SPECS/
```

第六步： 从 spec 文件生成二进制 rpm 文件：
你需要将你的目录切换到 spec 文件的目录下进行构建：
```sh
cd ~/rpmbuild/SPECS/
rpmbuild -ba fcitx5.spec
```
过程中可能提示你需要某些依赖包，你应该用 `dnf` 来安装这些依赖包。

第七步： 如果一切顺利，你的 RPM 文件将被构建在 `~/rpmbuild/RPMS/` 目录及其子目录中。

注意，这些步骤和细节可能因你使用的 Fedora 版本和 fcitx5 的具体情况而有所不同，需要你基于这个基本框架进行微调。

### 1.4.3 介绍一下rpm包

RPM（RPM Package Manager）是一种 Linux 下的软件包管理器。它原初被用在 Red Hat Linux 系统上，现在很多其他的类似 Fedora、CentOS 的发行版也采用了 RPM 包的格式。RPM 包文件一般都以 .rpm 作为他们的文件格式后缀。它有助于使用者更轻松地安装、升级和删除软件包。

在 Fedora 上安装 RPM 包，你可以使用 "rpm" 或 "dnf"命令行工具，一般推荐使用 "dnf" 命令，因为它可以自动解决依赖问题。

如果你想使用 "rpm" 命令行工具安装 RPM，可以按照以下步骤操作：

```bash
sudo rpm -ivh package_name.rpm
```

- -i 代表 install，表示安装
- -v 代表 verbose，表示详细模式
- -h 代表 hash，表示问题过程中的 hash 标记 (#)

在使用 "rpm" 命令安装软件包的时候需要提前确保所有的依赖已经解决。

如果你想使用 "dnf" 命令行工具安装 RPM，可以按照以下步骤操作：

```bash
sudo dnf install package_name.rpm
```

使用 "dnf" 命令安装时，它将自动解决和下载软件包的依赖问题。

### 1.4.4 介绍一下rpm包

"spec"文件通常在RPM包管理系统（如Red Hat，CentOS，Fedora等）中使用。它定义了如何构建一个RPM包，包括如何编译源代码，如何将程序文件、文档等安装到正确的位置，以及如何设置运行时依赖关系。一个简单的spec文件由以下部分组成：

- 包的定义（名称，版本，发布，摘要，许可，URL等）
- %prep章节（指定助手如何解包源文件并为构建做准备）
- %build章节（定义编译源代码的步骤）
- %install章节（定义如何安装编译后的程序到构建根目录）
- %files章节（列出包含在RPM包中的文件和目录）
- 可选的其他章节，如%check（运行单元测试），%post和%pre（安装或卸载RPM包时运行的脚本）

RPM打包系统使用这些信息来构建二进制RPM包，这些包可以在系统中安装、升级和卸载。

fedora的fcitx5源码包仓库地址：https://src.fedoraproject.org/rpms/fcitx5/tree/rawhide