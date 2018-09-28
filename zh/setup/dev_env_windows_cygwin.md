# Windows Cygwin 工具链

该工具链非常轻便，而且容易安装和使用。 它是目前Windows环境下用于PX4开发的最新和最好的工具。

> **提示** 这是官方唯一支持的在Windows环境下开发PX4的工具链（它已经在集成测试系统中经过测试）

该工具链支持：

* 编译/上传 PX4到Nuttx目标(Pixhawk系列飞控)
* JMAVSim/SITL 仿真会获得比其他Windows工具链更好的性能
* 类型校验，轻便安装，完整的命令行支持和许多[其他特性](#features)

这篇文章将解释怎样下载和使用该环境，并且在需要的时候怎样扩展和更新(比如，使用其他的编译器)。

<!-- Legacy Versions (**deprecated**):

* [PX4 Windows Cygwin Toolchain 0.4 Download](https://s3-us-west-2.amazonaws.com/px4-tools/PX4+Windows+Cygwin+Toolchain/PX4+Windows+Cygwin+Toolchain+0.4.msi) (18.09.2018)
* [PX4 Windows Cygwin Toolchain 0.3 Download](https://s3-us-west-2.amazonaws.com/px4-tools/PX4+Windows+Cygwin+Toolchain/PX4+Windows+Cygwin+Toolchain+0.3.msi) (25.07.2018)
* [PX4 Windows Cygwin Toolchain 0.2 Download](https://s3-us-west-2.amazonaws.com/px4-tools/PX4+Windows+Cygwin+Toolchain/PX4+Windows+Cygwin+Toolchain+0.2.msi) (09.05.2018)
* [PX4 Windows Cygwin Toolchain 0.1 Download](https://s3-us-west-2.amazonaws.com/px4-tools/PX4+Windows+Cygwin+Toolchain/PX4+Windows+Cygwin+Toolchain+0.1.msi) (23.02.2018)
-->

## 安装说明 {#installation}

1. 下载最新的MSI安装文件：[PX4 Windows Cygwin Toolchain 0.4 Download](https://s3-us-west-2.amazonaws.com/px4-tools/PX4+Windows+Cygwin+Toolchain/PX4+Windows+Cygwin+Toolchain+0.4.msi)(18.09.2018)
2. 运行它，选择你需要的安装路径，执行安装 ![jMAVSimOnWindows](../../assets/toolchain/cygwin_toolchain_installer.PNG)
3. 在安装结束后勾选*clone the PX4 repository, build and run simulation with jMAVSim*(这简化了你的开始准备工作)
    
    > **注意**如果你错过了这一步，你需要[手动克隆PX4 Firmware代码仓库](#getting_started)

## 入门指南 {#getting_started}

工具链使用专门配置的控制台(通过运行**run-console.bat**脚本)从而可以使用PX4编译命令

1. 进入到工具链的安装目录(默认**C:\PX4**)
2. 运行**run-console.bat**(双击)启动Cygwin bash控制台
3. 在控制台中运行克隆PX4 Firmware仓库命令
    
    > **注意**只需要克隆一次 如果你在安装程序最后选择了*clone the PX4 repository, build and run simulation with jMAVSim*，则可以跳过这一步。
    
    ```bash
    克隆PX4 Firmware仓库到home目录& 同时并行加载子模块
    git clone --recursive -j8 https://github.com/PX4/Firmware.git
    ```
    
    你现在可以使用控制台中的Firmware仓库代码来编译PX4

4. 举例，要运行JMAVSim:
    
    ```bash
    # 进入Firmware仓库目录
    cd Firmware 
    # 使用JMAVSim编译并运行SITL模拟器来验证 
    make posix jmavsim
    ```
    
    控制台将会显示：
    
    ![jMAVSimOnWindows](../../assets/simulation/jmavsim_windows_cygwin.PNG)

下面[ 有关如何生成 PX4 的详细说明 ](../setup/building_px4.md) (或参阅下面的部分以了解更多常规用法说明)。

## 使用说明 {#usage_instructions}

安装目录(默认：**C:\PX4**)包含了用于启动控制台窗口和包含Cygwin工具链的其他开发工具的脚本。 下面是脚本列表。

* ** 运行控制台. bat **-启动 POSIX (类似 linux) bash 控制台。
* ** 运行 eclipse **-启动基于 [ eclipse 的 c++ IDE ](http://www.eclipse.org/downloads/eclipse-packages/)。
* ** 运行 vscode **-从默认安装目录: ` C:\Program File \ Microsoft VS Code `启动 [ Visual Studio Code IDE ](https://code.visualstudio.com/) (必须单独安装),

> ** 提示 **[ 手动设置 ](#manual_setup) 部分解释了为什么需要使用脚本以及它的工作原理。

<span></span>

> ** 提示 **您可以为批处理脚本创建桌面快捷方式, 以便更方便地访问, 因为安装程序可能未能为您创建它们 (如0.2的工具链版本)。

普通工作流程包括通过双击 ** run-console. bat ** 脚本来手动运行终端命令来启动控制台窗口。 更喜欢 IDE开发环境 的开发人员可以使用相应的 ** run- XXX. bat ** 脚本启动它, 以编辑代码/运行生成。

### Windows & Git 特殊情况

#### Windows CR + LF 对比 Unix LF 行结尾

我们建议您所有的代码仓库都强制使用Unix的LF行结尾，并以此运行工具链(并且使用编辑器可以按照此格式保存您所做的修改 - 譬如 Eclipse 或者 VS Code) 源文件也可以兼容 CR+LF 行结尾并保存在本地, 但在 Cygwin (如 shell 脚本的执行) 中需要 Unix 行结尾 (否则您会收到类似 ` $ ' \r ': 未找到命令的错误. `。 幸运的是, 当您在代码仓库的根目录中执行两个命令时, git 可以为您强制转换:

    git config core.autocrlf false
    git config core.eol lf
    

如果您在多个代码仓库中使用此工具链, 还可以为您的计算机在全局范围内设置这两种配置:

    git config --global ...
    

建议不要这样做, 因为它可能会影响 Windows 计算机上的任何其他 (无关) git 使用。

#### Unix 执行权限

在 Unix 下, 每个文件的权限中都有一个标志, 它告诉操作系统是否允许执行该文件。 Cygwin 下的 * git * 支持并遵守该标识位 (尽管 Windows 具有不同的权限系统)。 这通常会导致 * git * 发现权限上的差异, 即使没有真正的区别, 这看起来是这样的:

    diff --git ...
    old mode 100644
    new mode 100755
    

我们建议通过在每个代码仓库中执行 ` git config core.fileMode false ` 来禁用此功能, 并可以使用此工具链。

## 附加信息

### 特性/问题 {#features}

以下已知正常功能 (版本 2.0):

* 使用 jMAVSim 编译和运行 SITL, 其性能明显优于虚拟机 (它生成一个本机 windows 二进制 ** px4.exe **)。
* 编译和上载 NuttX 二进制文件 (例如: px4fmu-v2 和 px4fmu-v4)
* 使用 * astyle * 进行格式检查 (支持命令: ` 设置格式 `)
* 命令行自动补全
* 绿色安装 安装程序不会影响您的系统和全局路径 (它只修改选定的安装目录, 例如 ** C:\PX4 \ ** 并使用临时本地路径)。
* 安装程序支持更新到最新版本, 同时保持您的个人更改在工具链文件夹中

补充:

* 仿真: 不支持Gazebo 和 ROS
* 仅支持 NuttX 和 JMAVSim/SITL 编译。
* [已知问题/报告您的问题](https://github.com/orgs/PX4/projects/6)

### Shell 脚本安装 {#script_setup}

还可以使用 Github 项目中的 shell 脚本安装环境。

1. 请确保安装了 [ Windows Git ](https://git-scm.com/download/win)。
2. 将代码仓库 https://github.com/PX4/windows-toolchain 克隆到要安装工具链的位置。 打开 ` Git Bash ` 并执行以下操作，打开后会自动进入默认的安装目录:

    cd /c/
    git clone https://github.com/PX4/windows-toolchain PX4
    

1. 如果要安装所有组件, 请进入到新克隆的代码仓库文件夹, 然后双击位于文件夹 `toolchain`目录中的脚本 ` install-all-components.bat`。 如果您只需要某些组件并希望占用有限的Internet 数据和磁盘空间, 则可以进入到不同的组件文件夹, 如 ` toolchain\cygwin64 `, 然后单击 ** install-XXX.bat ** 脚本以获取特定的内容。
2. 继续 [ 入门指南 ](#getting_started) (或 [ 使用说明 ](#usage_instructions)) 

### 手动安装 (对于开发人员) {#manual_setup}

本节介绍如何在从基于脚本安装目录中通过相应的脚本手动安装 Cygwin 工具链。 结果应与使用脚本或 MSI 安装程序相同。

> ** 注意 **因为工具链的更新, 因此这些指令可能无法涵盖未来所有更改的每个细节。

1. 创建 * 文件夹 *: ** C:\PX4 \ **、** C:\PX4\toolchain \ ** 和 ** C:\PX4\home \ **
2. 从 [ Cygwin 官方网站 ](https://cygwin.com/install.html) 下载 * Cygwin 安装程序 * 文件 [ official Cygwin website ](https://cygwin.com/setup-x86_64.exe)
3. 运行下载的安装程序文件
4. 在安装向导中选择安装到文件夹中: ** C:\PX4\toolchain\cygwin64 \ **
5. 选择安装默认的 Cygwin 基础包和以下附加包的最新可用版本:

* **Category:Packagename**
* Devel:cmake (3.3.2 正常工作无告警, 3.6.2有告警但能够正常工作)
* Devel:gcc-g++
* Devel:git
* Devel:make
* Devel:ninja
* Devel:patch
* Editors:xxd
* Editors:nano (除非你精通vim)
* Python:python2
* Python:python2-pip
* Python:python2-numpy
* Python:python2-jinja2
* Archive:unzip
* Utils:astyle
* Shells:bash-completion
* Web:wget
    
    > **Note** Do not select as many packages as possible which are not on this list, there are some which conflict and break the builds.
    
    <span></span>
    
    > **Note** That's what [cygwin64/install-cygwin-px4.bat](https://github.com/MaEtUgR/PX4Toolchain/blob/master/toolchain/cygwin64/install-cygwin-px4.bat) does.

1. Write up or copy the **batch scripts** [`run-console.bat`](https://github.com/MaEtUgR/PX4Toolchain/blob/master/run-console.bat) and [`setup-environment-variables.bat`](https://github.com/MaEtUgR/PX4Toolchain/blob/master/toolchain/setup-environment-variables.bat).
    
    The reason to start all the development tools through the prepared batch scripts is they preconfigure the starting program to use the local, portable Cygwin environment inside the toolchain's folder. This is done by always first calling the script [**setup-environment-variables.bat**](https://github.com/MaEtUgR/PX4Toolchain/blob/master/toolchain/setup-environment-variables.bat) and the desired application like the console after that.
    
    The script [`setup-environment-variables.bat`](https://github.com/MaEtUgR/PX4Toolchain/blob/master/toolchain/setup-environment-variables.bat) locally sets environmental variables for the workspace root directory `PX4_DIR`, all binary locations `PATH`, and the home directory of the unix environment `HOME`.

2. Add necessary **python packages** to your setup by opening the Cygwin toolchain console (double clicking **run-console.bat**) and executing
    
        pip2 install toml
        pip2 install pyserial
        pip2 install pyulog
        
    
    > **Note** That's what [cygwin64/install-cygwin-python-packages.bat](https://github.com/MaEtUgR/PX4Toolchain/blob/master/toolchain/cygwin64/install-cygwin-python-packages.bat) does.

3. Download the [**ARM GCC compiler**](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads) as zip archive of the binaries for Windows and unpack the content to the folder `C:\PX4\toolchain\gcc-arm`.
    
    > **Note** This is what the toolchain does in: [gcc-arm/install-gcc-arm.bat](https://github.com/MaEtUgR/PX4Toolchain/blob/master/toolchain/gcc-arm/install-gcc-arm.bat).

4. Install the JDK:
    
    * Download the [**Java Development Kit Installer**](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
    * Because sadly there is no portable archive containing the binaries directly you have to install it.
    * Find the binaries and move/copy them to **C:\PX4\toolchain\jdk**.
    * You can uninstall the Kit from your Windows system again, we only needed the binaries for the toolchain.
    
    > **Note** This is what the toolchain does in: [jdk/install-jdk.bat](https://github.com/MaEtUgR/PX4Toolchain/blob/master/toolchain/jdk/install-jdk.bat).

5. Download [**Apache Ant**](https://ant.apache.org/bindownload.cgi) as zip archive of the binaries for Windows and unpack the content to the folder `C:\PX4\toolchain\apache-ant`.
    
    > **Tip** Make sure you don't have an additional folder layer from the folder which is inside the downloaded archive.
    
    <span></span>
    
    > **Note** This is what the toolchain does in: [apache-ant/install-apache-ant.bat](https://github.com/MaEtUgR/PX4Toolchain/blob/master/toolchain/apache-ant/install-apache-ant.bat).

6. Download, build and add *genromfs* to the path:
    
    * Clone the source code to the folder **C:\PX4\toolchain\genromfs\genromfs-src** with 
            cd /c/toolchain/genromfs
            git clone https://github.com/chexum/genromfs.git genromfs-src

* Compile it with: 
    
        cd genromfs-src
         make all
    
    * Copy the resulting binary **genromfs.exe** one folder level out to: **C:\PX4\toolchain\genromfs**
    
    > **Note** This is what the toolchain does in: [genromfs/install-genromfs.bat](https://github.com/MaEtUgR/PX4Toolchain/blob/master/toolchain/genromfs/install-genromfs.bat).

1. Make sure all the binary folders of all the installed components are correctly listed in the `PATH` variable configured by [**setup-environment-variables.bat**](https://github.com/MaEtUgR/PX4Toolchain/blob/master/toolchain/setup-environment-variables.bat).