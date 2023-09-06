# BoAT Edge用户手册

## 引言

### 概述
本文介绍BoAT Edge 产品的实现——BoAT Infra Arch基础框架的功能和使用方法，BoAT Edge产品特性的展现依赖于BoAT Infra Arch基础框架灵活的配置和组合能力。  
本文的预期读者为集成BoAT Edge 产品软件框架的客户和爱好者。  
本用户手册版本号为1.0.0。

### 缩写术语
|术语   |解释                                       |
|:----- | :-------------------------------          |
|ABI    |Application Binary Interface               |
|API    |Application Programming Interface          |
|BE     |BoAT-Engine                                |
|BIA    |BoAT Infra Arch                            |
|BoAT   |Blockchain of AI Things                    |
|BPT    |BoAT-ProjectTemplate                       |
|BSL    |BoAT-SupportLayer                          |
|IoT    |Internet of Things                         |
|JSON   |JavaScript Object Notation                 |
|OS     |Operating System                           |
|RLP    |Recursive Length Prefix (Encoding/Decoding)|
|RPC    |Remote Procedure Call                      |
|RTOS   |Real Time Operating System                 |
|SDK    |Software Development Kit                   |
|WASM   |Web Assembly                               |


## 功能和架构
BoAT Infra Arch 基础框架是面向IoT设备MCU/蜂窝模组的C语言区块链应用框架客户端软件，便于移植到各类模组中，帮助基于IoT设备MCU/蜂窝模组的物联网应用连接区块链，实现数据上链等业务。BoAT Infra Arch基础框架SDK向物联网应用提供的功能包括发起链上交易、智能合约C接口代码自动生成、调用智能合约、管理区块链密钥等。

**已支持的区块链:**  
以太坊/Polygon  
PlatON  
PlatONE  
FISCO-BCOS  
HyperlEdger Fabric  
Huawei BCS (华为链)  
Chainmaker (长安链)  
VenaChain (万纳链)

**支持的Module Open SDK**  
ChinaMobile-ML307A-DCLN  
Fibocom-FG150  
Fibocom-FM650  
Fibocom-L610  
linux-default  
Mobiletek-L503C-6S  
Unisoc-8850   

**支持的Target操作系统：**  
Linux  
RTOS

**支持的Build操作系统：**  
Linux/Cygwin  

**主要特性：**  
区块链账号（钱包）参数配置  
区块链密钥对生成  
区块链账号的创建/加载/卸载  
转账交易  
智能合约调用（自动生成C调用接口）  
智能合约调用（手工构造）  


### 系统中的位置

BoAT Infra Arch基础框架SDK的各个组件以软件lib库(libbaotxxx.a)的形式，运行于IoT设备MCU/蜂窝模组的应用处理器上。SDK以C源文件形式提供，在IoT设备MCU/蜂窝模组的开发环境中编译。  

对于OpenCPU形态的IoT设备MCU/蜂窝模组，BoAT Infra Arch基础框架SDK的各个组件库被`物联网应用链接`，形成具备区块链链接能力的物联网应用程序。  

图 2‑1展示了BoAT Infra Arch基础框架(图中简称为BoAT)在OpenCPU模组中的位置。BoAT作为应用层协议，位于模组已有的协议栈上方，向物联网应用提供区块链服务。BoAT的对等层是区块链网络层。  

![BoAT Infra Arch基础框架在系统中的位置](https://aitos-io.github.io/BoAT-X-Framework/zh-cn/images/BoAT_User_Guide_cn-F2-1-BoAT_in_system.png)
图 2-1 BoAT Infra Arch基础框架在系统中的位置

对于非OpenCPU形态的IoT设备MCU/蜂窝模组，BoAT Infra Arch基础框架库的各个组件被`模组固件链接`，并由模组厂商扩展为AT命令，供上位机上的物联网应用调用，不再赘述。  

### BoAT Infra Arch基础框架SDK架构
BoAT Infra Arch基础框架SDK如图 2‑2所示，BoAT SDK主要包含BoAT Engine、BoAT Support Layer两层。
BoAT Engine主要提供区块链应用相关业务逻辑，包含：IoT设备钱包应用API(Wallet APIs)、区块链客户端接口协议(Protocols)、区块链网络接口(Networks)。
BoAT Support Layer主要为IoT应用提供系统底层抽象通用接口，包含：BoAT系统无关通用组件(BoAT Common Components)、操作系统API接口抽象层(Openrating System Abstraction Layer)、系统设备驱动/特性功能抽象层(Driver Abstraction Layer)。`远程过程调用接口、公共组件、硬件依赖组件以及工具组件。`

![BoAT Infra Arch基础框架SDK架构](https://github.com/phengao/hello-world/blob/master/boatsdk.png)  
图 2-2 BoAT Infra Arch基础框架SDK架构

基于BoAT Infra Arch基础框架SDK的物联网应用程序，通过调用BoAT Engine应用接口实现区块链访问业务（如图中黄色箭头所示），同时也可以通过BoAT Support Layer提供的系统抽象接口访问底层系统API（如图中橙色箭头所示），使得物联网应用程序能够在BIA基础框架支持的模组平台之间实现跨平台应用。在切换模组平台时，仅需要更改编译配置并完成编译生成固件，即可下载到对应模组中运行。  

### BoAT Infra Arch基础框架SDK代码结构
BoAT Infra Arch基础框架SDK代码由多个源码仓构成，主要的仓库如下：  
**BoAT-Engine**仓库，提供区块链应用接口实现的源码和编译目录。  
**BoAT-SupportLayer**仓库，提供平台抽象相关源码和编译目录。  
**BoAT-ProjectTemplate**仓库，提供构建BoAT Infra Arch基础框架应用程序编译目录的脚本仓库。

#### BoAT-Engine
BoAT Engine代码结构如下：
```
<BoAT-Engine>
|
+---demo                  | Demo application
+---include               | Header files for application to include
+---network               | Configuration and management of blockchain networks
|   \---chainmaker        |     network api of chainmaker
|   \---cita              |     network api of cita
|   \---ethereum          |     network api of ethereum
|   \---fiscobcos         |     network api of fiscobcos
|   \---hlfabric          |     network api of hlfabric
|   \---hwbcs             |     network api of hwbcs
|   \---platon            |     network api of platon
|   \---platone           |     network api of platone
|   \---quorum            |     network api of quorum
|   \---venachain         |     network api of venachain
+---protocol              | Blockchain client protocol implementation
|   \---boatchainmaker_v1 |     protocol api of chainmaker
|   \---boatchainmaker_v2 |     protocol api of chainmaker
|   \---boatcita          |     protocol api of cita
|   \---boatethereum      |     protocol api of ethereum
|   \---boatfiscobcos     |     protocol api of fiscobcos
|   \---boathlfabric      |     protocol api of hlfabric
|   \---boathwbcs         |     protocol api of hwbcs
|   \---boatplaton        |     protocol api of platon
|   \---boatplatone       |     protocol api of platone
|   \---boatquorum        |     protocol api of quorum
|   \---boatvenachain     |     protocol api of venachain
|   +---common            |     Universal soft algorithms implementation for blockchains
|   |   \---web3intf      |     web3 api implementation
+---tests                 | Test cases
\---tools                 | Tools for generating C interface from contract ABI
\---wallet                | wallet API implementation for each blockchain
```
在BoAT Engine中：  
wallete目录，包含各个区块链对应的Wallet APIs，是SDK提供给物联网应用调用的接口，具体包括SDK公共接口和针对不同区块链协议的钱包和交易接口。  
protocol目录，实现各区块链Portocols，区块链客户端接口协议主要实现针对不同区块链的交易接口协议，并通过RPC接口与区块链节点进行交互。  
network目录，Networks区块链网络接口主要实现对不同区块链网络节点的配置和信息管理，使Wallet APIs能够通过网络信息于区块链节点建立链接。  
tools目录，包含工具组件，提供了根据Solidity或者WASM C++的智能合约ABI接口，生成C语言合约调用接口的Python工具。  
在应用中，BoAT-Engine以静态库的形式为应用程序提供区块链应用API接口，编译BoAT-Engine生成的静态库为libboatengine.a

访问 **![BoAT-Engine仓库](https://github.com/aitos-io/BoAT-Engine)**

#### BoAT-SupportLayer
BoAT Support Layer代码结构如下：
```
<BoAT-SupportLayer>
|
+---common            | Universal soft algorithms implementation
|   \---http2intf     |     Universal soft algorithms implementation
|   \---rpc           |     rpc interface
|   \---storage       |     interface for storing data or files
|   \---utilities     |     Utility APIs
+---include           | Header files for application to include
+---keypair           | Interface for generating and managing key pairs
+---keystore          | Store a series of Secret Keys, Key Pairs, or Certificates
|   +---SE            |     Keystore Interface Implemented by Security Chip
|   \---soft          |     Keystore Interface Implemented by Software
+---platform          | The implementation of the abstraction layer for the Different Platforms
|   +---Chin...DCLN   |     The implementation of the abstraction layer for the ChinaMobile-ML307A-DCLN
|   +---Fibocom-L610  |     The implementation of the abstraction layer for the Fibocom-L610
|   +---include       |     Header files for platform internal use
|   +---linux-default |     The implementation of the abstraction layer for the Linux-default platform
|       \---src       |         The source code of implementation for linux-deafult
|           \---osal  |             The implementation of the operating system abstraction layer for the linux-default
|           \---dal   |             The implementation of the driver abstraction layer for the linux-default
+---tests             | Test cases
+---third-party       |     Third party libraries
    \---cJSON         |     JSON parsing interface for C language 
    \---crypto        |     Including various software encryption algorithms
    \---nghttp2       |     HTTP/2 library implemented by C language
    \---protobuf-c    |     protobuf interface
    \---protos        |     protos interface
    +---rlp           |     RLP encoder
```
在BoAT Support Layer中：  
BoAT Common Components组件包含物联网应用所需的各类系统无关通用软件，相关目录包括：  
common：  
远程过程调用（RPC）接口实现针对不同通信承载的接口方式。  
还包含了部分通用协议的调用接口（http2intf）  

third-party：  
使用第三方软件提供的RLP编码、JSON编解码、字符串处理等公共功能。 

keypair:   
提供keypair应用相关接口，创建密钥对、获取公钥、删除密钥对等功能。  

keystore:    
提供密钥存储相关接口。

OSAL组件:  
提供通用系统API调用API接口，将不同平台（platform）的系统API接口转换转换为统一抽象接口供给应用层使用，包含：semaphor、mutex、timer、task/thread、queue等。

DAL组件:  
提供通用系统API调用API接口，将不同平台（platform）的系统API接口转换转换为统一抽象接口供给应用层使用，包含：i2c、uart、virtualAT、http、ssl、storage等。例如密码学加速器、安全存储、随机数等，需要根据具体硬件进行移植。SDK亦提供一套全软件的默认实现。

OSAL、DAL组件按照平台组织，在platform目录下包含BIA支持的平台，每个平台目录下包含了各自OSAL和DAL的具体实现。如上图中platform目录下linux-default目录下包含osal和dal目录。  
在应用中，BoAT-SupportLayer以静态库的形式为应用程序提供平台抽象API接口，编译BoAT-SupportLayer生成的静态库为libboatvendor.a

访问 **![BoAT-SupportLayer仓库](https://github.com/aitos-io/BoAT-SupportLayer)**

#### BoAT-ProjectTemplate
```
<BoAT-ProjectTemplate>
|
|---BoATLibs.conf   | The configuration of using BoAT libs
|---config.py       | Used for creating a compilation directory for 
                    | applications based on the BoAT Infra Arch infrastructure.
```

BoATLibs.conf，用于配置在项目中所使用的BoAT Infra Arch基础框架SDK提供的静态库（libboatvendor.a/libboatengine.a）。  
config.py，按照配置文件构建基于BoAT Infra Arch基础架构SDK的应用程序编译目录。  
访问 **![BoAT-ProjectTemplate仓库](https://github.com/aitos-io/BoAT-ProjectTemplate)**  

## BoAT Infra Arch基础框架SDK编译  

### 软件依赖
BoAT Infra Arch基础框架SDK编译依赖于以下软件:

|依赖软件      |要求                                      |Build环境|Target环境|
| :------------| :----------------------------------------| :-------| :--------|
|Host OS       |Linux，或者Windows上的Cygwin              |Required |          |
|Target OS     |Linux                                     |         |Required  |
|Compiler      |gcc，需要支持c99 (9.3.0 is tested)        |Required |          |
|Cross-compiler|arm-oe-linux-gnueabi-gcc (4.9.2 is tested)|Required |          |
|Make          |GNU Make (4.3 is tested)                  |Required |          |
|Python        |Python 2.7 (Python 3 is also compatible)  |Required |          |
|curl          |libcurl及其开发文件(7.55.1 is tested)     |Required on Linux default |Required on Linux default |

在编译和使用SDK之前，需要确保这些软件已经安装。在Ubuntu下，可以使用apt install命令安装相应的包。在Cygwin下，使用Cygwin自带的Setup程序进行安装。  

- Ubuntu安装
   ````
   sudo apt install python
   download curl-7.55.1.tar.xz : https://curl.se/download/curl-7.55.1.tar.xz
   tar -xvf ./curl-7.55.1.tar.xz 
   cd curl-7.55.1
   ./configure -enable-smtp -enable-pop3
   make
   sudo make install
   sudo apt-get install libcurl4-openssl-dev
   ````
- Cygwi安装  
   执行setup-x86_64.exe，安装make、gcc、python、libcurl等工具，如下图：
   ![image](https://user-images.githubusercontent.com/81662688/130744353-3e6ad68c-7945-44e6-93ac-c8a468e8e0aa.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744453-08a1dff2-08d8-46d8-9732-de814b077ba7.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744464-409165ad-a743-401c-8ed2-68060cd4d9e1.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744485-e3d5a4ed-bd9a-4a44-b9fa-81066fe2165b.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744499-06029343-9f65-4f33-a094-01de4c8ae25e.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744525-e4614f6a-6f33-4dae-9d6c-2d0b5bb994ec.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744541-7645fe99-9c21-44e0-a3cc-fe84b102d0cf.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744556-163fb5e4-0260-42d8-b8c1-4c78052bd7d1.png)
   
   在Windows下，SDK不支持在Cygwin以外的环境下编译。如果必须在Cygwin以外运行（例如以Windows为Build环境的交叉编译器），请参照[以Windows为编译环境](#以windows为编译环境)章节对编译文件进行调整。

   在RTOS上移植SDK时，应对libcurl依赖进行移植或将RPC方法重写。  


### 编译准备
#### BoAT Infra Arch基础框架SDK源码路径
SDK源码的保存路径中，自根目录起，各级目录名称应由英文字母、数字、下划线或减号组成，不应出现空格、中文以及加号、@、括号等特殊符号。  

例如，以下为适合的路径：  
/home/developer/project-blockchain  
C:\Users\developer\Documents\project  

以下为不适合的路径：  
/home/developer/project+blockchain/，路径中带有‘+’号。  
C:\Documents and Settings\developer\project，路径中带有‘ ’空格。  

如果无法避免在路径中出现上述不适字符，请使用以下方法规避：  
- Linux：在一个没有不适字符的路径中，建立一个指向SDK目录的符号链接：ln -s \<BoAT-ProjectTemplate\> boatiotsdk，在该符号链接的路径下进行编译。  
- Windows：使用SUBST Z: \<BoAT-ProjectTemplate\>命令虚拟一个盘符Z:（也可以是其他未使用的盘符），在Z:盘下进行编译。

#### 构建编译目录  
BoAT Infra Arch基础框架SDK所包含的BoAT-Engine、BoAT-SupportLayer仓库的源码不能直接下载编译，需要先下载BoAT-ProjectTemplate编译模板仓库到本地，在本地执行配置脚本完成BoAT-Engine和BoAT-SupportLayer**源码下载**、**区块链选择**、**目标平台选择**、**生成Makefile**、**构建编译目录**，以下依序说明整个过程。  

+ **下载BoAT-ProjectTemplate项目开发模板**  
   BoAT-ProjectTemplate编译模板可以从github下载：  
   打开 linux/Windows 操作系统的终端，执行以下指令将BoAT-ProjectTemplate 仓库克隆到本地：  
   ```
   git clone https://github.com/aitos-io/BoAT-ProjectTemplate.git
   ```
   执行成功后，当前路径下将创建BoAT-ProjectTemplate目录，存放从github克隆的BoAT-ProjectTemplate开发模板。   克隆后BoAT-ProjectTemplate/目录的内容如下：  
   ```
   <BoAT-ProjectTemplate>
   |-- BoATLibs.conf
   |-- config.py
   |-- README.md
   |-- README_cn.md
   ```
   也可以将 BoAT-ProjectTemplate 仓库克隆为开发项目目录，例如:构建 boatDevelop 开发目录  
   ```
   git clone https://github.com/aitos-io/BoAT-ProjectTemplate.git boatDevelop
   ```
   将 BoAT-Engine 仓库克隆 到 boatDevelop 目录，并在此目录中开发  
   本文以\<BoAT-ProjectTemplate\>目录作为开发模板根目录。
  
+ **配置 BoATLibs.conf**  
   \<BoAT-ProjectTemplate/\>目录下BoATLibs.conf文件用于配置当前项目中所使用的开源仓库，后续配置脚本 config.py, 通过读取BoATLibs.conf文件的配置信息，克隆相应的开源仓库到本地。  
   BoATLibs.conf 文件默认包含 BoAT-SupportLayer 仓库，内容如下：
   ```
   BoAT-SupportLayer
   ```
   在编译BoAT-Engine中间件时，需将“BoAT-Engine”添加到BoATLibs.conf文件，修改后的内容如下：
   ```
   BoAT-SupportLayer
   BoAT-Engine
   ```
   BoAT-Egine是BoAT Infra Arch基础架构下基于BoAT-SupportLayer开发的跨平台物联网区块链应用中间件，因此在BoATLibs.conf文件中必须包含BoAT-SupportLayer基础库。  
   
+ **运行 config.py 脚本**  
   运行 config.py 脚本，根据提示输入选项，完成区块链和目标平台选择，构建编译目录。  
   在\<BoAT-ProjectTemplate/\>目录下执行脚本，过程中会有数次交互，详细过程如下。  
   执行脚本：
   ```
   python3 config.py
   ```
   显式提示如下：
   ```
   We will clone the BoAT- SupportLayer' repository, which may take several minutes
   
   Input the branch name or null:
   ```
   直接回车，将clone开源仓库BoAT- SupportLayer'的main分支
   ```
   branch name is []
   
   git clone https://github.com/aitos-io/BoAT-SupportLayer.git
   
   Cloning into 'BoAT-SupportLayer'...
   remote: Enumerating objects: 2837, done.
   remote: Counting objects: 100% (611/611), done.
   remote: Compressing objects: 100% (281/281), done.
   remote: Total 2837 (delta 384), reused 521 (delta 317), pack-reused 2226
   Receiving objects: 100% (2837/2837), 2.41 MiB | 1.86 MiB/s, done.
   Resolving deltas: 100% (1769/1769), done.
   git cmd succ
   
   We will clone the BoAT-Engine repository, which may take several minutes
   
   Input the branch name or null:
   ```
   输入回车，将`clone`开源仓库`BoAT-Engine`的`main`分支。  
   ```
   branch name is []
   
   git clone https://github.com/aitos-io/BoAT-Engine.git
   
   Cloning into 'BoAT-Engine'...
   remote: Enumerating objects: 860, done.
   remote: Counting objects: 100% (860/860), done.
   remote: Compressing objects: 100% (400/400), done.
   remote: Total 860 (delta 551), reused 752 (delta 455), pack-reused 0
   Receiving objects: 100% (860/860), 513.51 KiB | 365.00 KiB/s, done.
   Resolving deltas: 100% (551/551), done.
   git cmd succ
   
   overwrite the Makefile?(Y/n):
   ```
   输入回车，被判定为Y：Yes；如果你不希望重写Makefile，则可以输入n，编译配置将立即结束。  
   ```
   Yes
   
    Select blockchain list as below:
    [1] ETHEREUM          : 
    [2] PLATON            : 
    [3] PLATONE           : 
    [4] FISCOBCOS         : 
    [5] HLFABRIC          : 
    [6] HWBCS             : 
    [7] CHAINMAKER_V1     : 
    [8] CHAINMAKER_V2     : 
    [9] VENACHAIN         : 
    [a] QUORUM            : 
    [b] CITA              : 
    [0] All block chains
    Example:
     Select blockchain list as below:
     input:1a
     Blockchain selected:
      [1] ETHEREUM
      [a] QUORUM
   
   input:
   ```
   输入应用程序中选择的区块链，输入`9`，选择`VENACHAIN`区块链。  
   ```
   input:9
   Blockchain selected:
    [9] VENACHAIN
   
   Select the platform list as below:
   [1] linux-default             : Default linux platform
   [2] Fibocom-L610              : Fibocom's LTE Cat.1 module
   [3] create a new platform
   ```
   输入1，选择应用目标平台为linux-default，选择后脚本将自动完成Makefile的生成。  
   ```
   1
   platform is : linux-default
   
   include BoAT-SupportLayer.conf
   
   include BoAT-Engine.conf
   
   ./BoAT-SupportLayer/demo/ False
   ./BoAT-Engine/demo/ True
   Configuration completed
   ```
   脚本执行完后，完成构建BoAT-Engine编译目录。  
   此时BoAT-Engine相关源码已经下载到本地，同时完成BoAT-Engine的编译配置，按照选择的platform 生成Makefile。  
   执行完配置后，开发目录将包含以下内容：
   ```
   <BoAT-ProjectTemplate>
   |-- <BoAT-Engine>
   |-- <BoAT-SupportLayer>
   |-- BoATLibs.conf
   |-- config.py
   |-- Makfile
   |-- README.md
   |-- README_en.md
   ```
   脚本执行完成后增加目录\<BoAT-Engine\>和\<BoAT-SupportLayer\>以及Makefile文件。  
   
#### 引用的外部环境变量
根据需要，修改SDK以下文件中的环境变量：  
\<BoAT-ProjectTemplate\>/BoAT-SupportLayer/platform/\<platform_name\>/external.env：配置外部编译环境依赖、硬件相关的头文件搜索路径。
在Host编译时，如果gcc和binutils已经安装在系统中，通常不需要修改这些环境变量配置。  

在交叉编译时，如果交叉编译环境需要配置特定的INCLUDE路径，需要在上述文件中添加路径。  

#### BoAT Infra Arch基础框架SDK配置
- 使能/禁能区块链协议  
   执行**BoAT-ProjectTemplate**中config.py脚本，如果BoATLibs.conf文件中包含BoAT-Engine仓库，则会在配置过程中要求选择区块链，提示如下：
   ```
    Select blockchain list as below:
    [1] ETHEREUM          : 
    [2] PLATON            : 
    [3] PLATONE           : 
    [4] FISCOBCOS         : 
    [5] HLFABRIC          : 
    [6] HWBCS             : 
    [7] CHAINMAKER_V1     : 
    [8] CHAINMAKER_V2     : 
    [9] VENACHAIN         : 
    [a] QUORUM            : 
    [b] CITA              : 
    [0] All block chains
    Example:
     Select blockchain list as below:
     input:1a
     Blockchain selected:
      [1] ETHEREUM
      [a] QUORUM
   ```
   根据需要，选择一个或多个区块链编号，使能相应的区块链协议，未选择的区块链将被禁能。以上提示中给出了选择区块链示例，当输入“1a”时，表示选择使能[1]、[a]对应的区块链，即“ETHEREUM”和“QUORUM”,其余剩下的区块链将禁能，详见[构建编译目录](#构建编译目录)。  
   
- 日志打印级别调整  
根据需要，调整路径\<BoAT-ProjectTemplate\>/BoAT-SupportLayer/platform/\<platform_name\>/src/log/boatlog.h中`BOAT_LOG_LEVEL`的值，来调整日志的打印级别。

### 合约C接口代码自动生成
智能合约是区块链上的可执行代码，在区块链虚拟机（例如EVM和WASM）上执行，并以远程过程调用（RPC）方式被客户端调用。  
不同的虚拟机和合约编程语言，有不同的应用程序二进制接口（ABI）。客户端在通过RPC调用合约函数时，必须遵照相应的ABI组装接口。  
SDK中BoAT-Engine仓库tools/目录下提供以下工具，用于根据合约ABI，生成相应的C接口代码，使得C代码中，可以像调用一般C函数一样，通过生成的接口代码调用链上智能合约：  
|转换工具                                             |用途                                        |
|:----------------------------------------------------|:-------------------------------------------|
|\<BoAT-ProjectTemplate\>/BoAT-Engine/tools/eth2c.py               |根据以太坊Solidity的ABI，生成C调用代码      |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/tools/fiscobcos2c.py         |根据FISCO-BCOS Solidity的ABI，生成C调用代码 |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/tools/platoneSolidity2c.py   |根据PlatONE Solidity的ABI，生成C调用代码    |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/tools/platoneWASM2c.py       |根据PlatONE WASM的ABI，生成C调用代码        |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/tools/venachainSolidity2c.py |根据Venachain Solidity的ABI，生成C调用代码  |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/tools/venachainWASM2c.py     |根据Venachain WASM的ABI，生成C调用代码      |

由于合约编程语言一般支持面向对象，而C语言不支持面向对象，无法使用统一范式传递对象，因此只有参数类型与C语言内置类型一致的合约函数，可以通过工具转换为C调用代码。具体的支持合约函数输入类型详见[合约调用（自动生成）](#合约调用（自动生成）) 章节。

在进行调用前，首先需要编译合约，将合约编译中生成的ABI接口描述JSON文件，拷贝至SDK相应目录中：

|合约ABI存放路径                                        |用途                                                 |
|:------------------------------------------------------|:----------------------------------------------------|
|\<BoAT-ProjectTemplate\>/demo/demo_ethereum/demo_contract           |将以太坊的ABI JSON文件拷贝至该目录下                 |
|\<BoAT-ProjectTemplate\>/demo/demo_fiscobcos/demo_contract          |将FISCO-BCOS的ABI JSON文件拷贝至该目录下             |
|\<BoAT-ProjectTemplate\>/demo/demo_platone/demo_contract/Solidity   |将PlatONE（Solidity）的ABI JSON文件拷贝至该目录下    |
|\<BoAT-ProjectTemplate\>/demo/demo_platone/demo_contract/WSAM       |将PlatONE（WASM）的ABI JSON文件拷贝至该目录下        |
|\<BoAT-ProjectTemplate\>/demo/demo_venachain/demo_contract/Solidity |将Venachain（Solidity）的ABI JSON文件拷贝至该目录下  |
|\<BoAT-ProjectTemplate\>/demo/demo_venachain/demo_contract/WSAM     |将Venachain（WASM）的ABI JSON文件拷贝至该目录下      |

***注：ABI的JSON文件必须以“.json”为文件名后缀。***

在编译Demo过程中，自动生成工具将根据合约ABI JSON文件，生成相应的C接口调用代码。如果编译中自动生成C接口失败，则需要从<BoAT-ProjectTemplate>/contract的相应目录中，删除无法支持的ABI JSON文件（或者删除其中无法支持的接口），手工编写C代码，进行ABI接口组装，详见 [转账调用](#转账调用) 章节。  

### Host编译
Host编译指编译环境与目标环境一致，例如，在x86上编译x86程序。通常有两种使用Host编译的场景: 
1. 在软件调测阶段，在PC机上对软件功能进行测试。
2. 目标软件本身运行于基于x86/x86-64处理器的设备上，例如某些边缘网关。


#### 以Linux为编译环境
基于Linux发行版（例如Ubuntu）进行Host编译，一般无需特别配置编译环境，只需确保依赖软件都已安装。

编译遵照如下步骤:
1. 在符合[SDK源码路径](#boat-iot-framework-sdk源码路径)要求的路径中构建bia架构项目开发模板[编译目录](#构建编译目录)。  
2. 可选：将要调用的智能合约的ABI JSON文件放在\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_\<protocol\>/demo_contract的对应目录中（参见[合约C接口代码自动生成](#合约c接口代码自动生成)章节）。  
3. 在\<BoAT-ProjectTemplate\>目录下，执行以下命令:  
```
$make all
```
4. 编译完成后，生成的库文件在./lib中。应用应当包含BoAT-Engine和BoAT-SupportLayer中include目录下的头文件，并链接./lib下的libboatengine.a和libboatvendor.a静态库，实现访问区块链的功能。参见[头文件和库](#头文件和库)章节。

#### 以Cygwin为编译环境
在Windows上，SDK不支持在Cygwin以外的环境进行编译，也不支持使用gcc以外的编译器进行编译。

编译步骤与在Linux下相同。


### 交叉编译
交叉编译中，一般需要根据具体编译环境，对编译配置文件等进行配置。

#### 以Linux为编译环境
##### 独立的交叉编译环境
独立的编译环境是指arm-oe-linux-gnueabi-gcc（或类似交叉编译器）已经安装在Linux系统中，可以独立调用。

SDK要求系统中至少应该设置以下环境变量，使之指向交叉编译环境：


|环境变量  |说明                      |
|:------- |:------------------------ |
|CC       |指向交叉编译器gcc可执行文件 |
|AR       |指向交叉编译器ar可执行文件  |


当环境中没有定义CC和AR环境变量时，GNU make会默认CC=cc，AR=ar。通常，Linux系统中会安装host的gcc及bintuils编译环境，因此，未定义上述环境变量时，将会执行host编译。

在配置交叉编译环境时，通常需要执行特定shell脚本，对上述环境变量进行设置，使之指向交叉编译环境。对于bash shell，通常会执行类似如下的命令：
```
$source cross_compiler_config.sh     
```
或者
```  
$. cross_compiler_config.sh  
```
如上示例中的`cross_compiler_config.sh`并非本SDK中的脚本，而是交叉编译环境中的配置脚本，其具体名称和位置请参考交叉编译环境相关的说明。 
	
示例中的`source`或`.`是必需的，这使得该脚本在当前shell的上下文中执行，因而该脚本中对环境变量的修改仅能在当前shell中生效。


可以执行以下命令查看当前shell中的环境变量设置：  
```
$export
```

若环境变量CC和AR已设置，可以执行以下命令查看当前CC和AR的版本，以便确认是否已经指向了期望的交叉编译环境:  
```
${CC} -v  
${AR} -v
```

以上配置完成后，遵照[以Linux为编译环境](#以linux为编译环境)章节的步骤进行编译。


##### 与模组开发环境整合的交叉编译环境
有些OpenCPU模组在其所提供的开发环境中，已经整合了配套的交叉编译环境，使得客户无需另行在Linux系统中安装交叉编译器。这尤其便于在一台host电脑上，开发多个不同型号模组上的应用软件，而无需反复切换交叉编译环境。


###### 模组开发环境以GNU make为编译工具
若模组开发环境以GNU make为编译工具（各级源码目录内有Makefile），可以对BoAT Infra Arch基础框架SDK调整编译配置，将其纳入整合的模组开发环境中编译。

通常，模组开发环境中会提供客户代码的Example，并在编译体系中包含对客户示例代码Example的编译配置。首先将\<BoAT-ProjectTemplate\>目录（以下例子中以boatiotsdk代替目录名）复制到模组开发环境中的客户代码Example目录，然后修改针对客户代码Example的Makefile，增加一个编译BoAT Infra Arch基础框架SDK的target。

例如:  
假设原有编译环境中，客户代码Example的Makefile如下:  
```
.PHNOY: all  
all:  
    $(CC) $(CFLAGS) example.c -o example.o 

clean:  
	-rm -rf example.o
```
调整为:
```
.PHNOY: all boatiotsdkall boatiotsdkclean  
all: boatiotsdkall  
    $(CC) $(CFLAGS) example.c -o example.o 

clean: boatiotsdkclean    
	-rm -rf example.o  

boatiotsdkall:
	make -C boatiotsdk boatlibs

boatiotsdkclean:
	make -C boatiotsdk clean

```
其中，boatiotsdk为SDK所在目录，make后的-C参数指定进入boatiotsdk目录后按Makefile执行编译。


***注：Makefile中，target下的命令，必须一个Tab（ASCII码0x09）开头，而不能以空格开头。***

以上步骤仅仅是用于执行对SDK库的编译。SDK库编译完成后，还需将编译生成的lib库整合入模组开发环境。详见[头文件和库](#头文件和库)章节。

###### 模组开发环境采用非GNU Make编译工程
由于BoAT Infra Arch基础框架SDK以GNU make为编译工具，若模组开发环境采用非GNU Make的编译工具（例如Ninja、ant等），或者使用了编译工程的自动生成工具（如automake、CMake），则不能直接在模组开发环境中编译SDK。

在此类模组开发环境中编译SDK，需要将模组开发环境中的gcc、binutils编译工具释出，配置[独立的交叉编译环境](#独立的交叉编译环境)章节所述环境变量，使之可以在系统中调用，等同于独立交叉编译环境，然后编译SDK。


#### 以Windows为编译环境
在Windows下，SDK不支持在Cygwin以外的环境下编译。如果以Windows为编译环境的交叉编译器只能在Cygwin以外运行，应对编译环境和编译配置文件进行调整。  
在Cygwin以外进行交叉编译时，仍然需要安装Cygwin，并调整Makefile。

###### 安装Cygwin

SDK编译工程依赖于一些Cygwin工具，需要安装的工具如下:  

|所需工具    |用途                                                                                                                     |
|:--------  |:----------------------------------------------------------------------------------------------------------------------- |
|find       |需要Cygwin的find.exe用于递归搜索要编译的子目录。Windows自带有另一个同名但功能完全不同的FIND.EXE，不能使用。                     |
|rm         |用于删除指定目录和文件。Windows的cmd shell内置的RMDIR/RD和DEL命令分别只能用于删除目录（树）和文件，语法上与Cygwin的rm.exe不兼容。|
|mkdir      |用于创建一级或多级目录。Windows的cmd shell内置的MKDIR/MD命令具有相同功能，但语法不兼容                                         |
|GNU make   | 可以在Cygwin中安装make，也可以基于Windows上的编译器（例如Microsoft Visual Studio）自行编译GNU make。后者不依赖于Cygwin。      |

安装Cygwin之后，需要配置其路径。由于SDK编译所依赖的部分Cygwin工具与Windows自带工具同名，因此必须确保编译中引用的相关工具指向Cygwin的版本。

首先，在执行编译的cmd shell中，执行如下命令增加Cygwin的搜索路径：  
```
set PATH=%PATH%;\<Path_to_Cygwin\>\bin  
```
其中<Path_to_Cygwin>是Cygwin安装目录的绝对路径，如：C:\Cygwin64  


***注：上述命令可以编写在一个bat批处理文件中，或者直接加入Windows系统环境变量中，方便调用。如果直接加入Windows系统环境变量，不得将Cygwin置于%SystemRoot%\System32路径之前，否则在其他场景中调用Windows的FIND命令时，将错误地调用Cygwin的find版本，这将影响其他场景中使用Windows自带命令。***

然后，修改`<BoAT-ProjectTemplate>/BoAT-SupportLaer/platform/<platform_name>/external.env`，为依赖工具加上路径:
```
# Commands
CYGWIN_BASE := C:\cygwin64  # Modify to actual path to Cygwin
BOAT_RM := $(CYGWIN_BASE)\bin\rm -rf
BOAT_MKDIR := $(CYGWIN_BASE)\bin\mkdir
BOAT_FIND := $(CYGWIN_BASE)\bin\find

```

以Windows10为例，将搜索路径加入Windows环境变量的方法为:  
a)	在Windows徽标菜单上点右键，选择“系统”  
b)	在“关于”页中点击“系统信息”  
c)	在“系统”页中点击“高级系统设置”  
d)	在“系统属性”页中点击“环境变量”  
e)	在“环境变量”页中点击“系统变量”中的“Path”，并点击“编辑”  
f)	在“编辑环境变量”页中点击“新建”，新增Cygwin的安装目录下的bin路径，并确保该项新增路径位于%SystemRoot%\system32这条路径之后的任意位置  


###### 其他调整
在Cygwin以外进行交叉编译时，除上节所述外，还需要进行下列调整：

1. 尝试make，如果提示路径错误，则将Makefile中相应的路径分隔符，从“/”修改为“\\”。不要一开始就把所有的“/”都改为“\\”，因为部分源自Linux的工具的Windows版本，可以识别“/”作为路径分隔符。
2. 配置[独立的交叉编译环境](#独立的交叉编译环境)章节所述环境变量，使之指向正确的交叉编译环境。在这些环境变量中，路径应以“\\”为分隔符

### 编译和运行Demo
#### 准备
BoAT Infra Arch基础框架SDK中BoAT-Engine库提供基于以太坊、PlatON、PlatONE、FISCO-BCOS、HyperlEdger Fabric、HW-BCS、Venachain和Chainmaker的Demo。在运行这些Demo之前，需要首先安装相应的区块链节点软件（或者有已知节点），并部署Demo所需的智能合约。

Demo所使用的智能合约及其ABI JSON文件放置在：  

|Demo智能合约                                                  |合约ABI JSON文件                                              |用途           |
|:----------------------------------------------------------- |:------------------------------------------------------------ |:------------ |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_ethereum/demo_contract/StoreRead.sol   |\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_ethereum/demo_contract/StoreRead.json   |以太坊演示     |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_platone/demo_contract/WASM/my_contract.cpp    |\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_platone/demo_contract/WASM/my_contract.cpp.abi.json    |PlatONE演示    |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_fiscobcos/demo_contract/HelloWorld.sol |\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_fiscobcos/demo_contract/HelloWorld.json |FISCO-BCOS演示 |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_venachain/demo_contract/WASM/mycontract.cpp    |\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_venachain/demo_contract/WASM/mycontract.cpp.abi.json    |Venachain演示   |

在运行以太坊的Demo之前，需要安装以太坊节点模拟器ganache，以及以太坊智能合约编译部署工具truffle。  
ganache及truffle工具可以访问该网站：https://truffleframework.com  
ganache有命令行界面的ganache-cli版本，以及图形界面的Ganache版本。命令行界面的ganache-cli和图形界面的Ganache 1.x版本不会存盘，如果ganache-cli或Ganache   1.x的进程被终止，部署的合约会丢失，下次启动需要使用命令truffle migrate --reset重新部署合约，重新部署的合约地址可能会变化。图形界面的Ganache   2.x版本可以建Workspace保存状态，关闭下次重新打开该Workspace后，部署过的合约仍然在无需重新部署。  
除了使用ganache模拟器，还可以使用Ropsten等以太坊测试网络（需要申请免费的测试token）。  

在运行PlatON的Demo之前，需要安装PlatON节点。  
PlatON源码及工具可以访问该网站：https://platon.network/

在运行PlatONE的Demo之前，需要安装PlatONE节点，以及智能合约编译和部署工具。  
PlatONE源码及工具可以访问该网站：https://platone.wxblockchain.com

在运行Venachain的Demo之前，需要安装Venachain节点，以及智能合约编译和部署工具。  
Venachain源码及工具可以访问该网站：https://venachain-docs.readthedocs.io/zh/latest/documents/quick/env.html

在运行FISCO-BCOS的Demo之前，需要安装FISCO-BCOS节点和合约部署。  
FISCO-BCOS源码及安装部署步骤可以访问该网站：https://fisco-bcos-documentation.readthedocs.io

在完成节点（或模拟器）部署后，需要分别遵照相关网站的说明，部署Demo智能合约。智能合约部署成功后，将生成合约地址。

调用智能合约的Demo C代码放置在: 

|Demo C代码                                                  |用途                    |
|:---------------------------------------------------------- |:--------------------- |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_ethereum/demo_ethereum_storeread.c    |以太坊合约演示用例       |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_ethereum/demo_ethereum_transfer.c     |以太坊转账演示用例       |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_platon/demo_platon_transfer.c         |PLATON转账演示用例      |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_platone/demo_platone_mycontract.c     |PLATONE合约演示用例     |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_fiscobcos/demo_fiscobcos_helloworld.c |FISCO-BCOS合约演示用例  |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_fabric/demo_fabric_abac.c             |FABRIC合约演示用例      |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_hw_bcs/demo_hw_bcs.c                  |HW-BCS合约演示用例      |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_chainmaker/demo_chainmaker.c |CHAINMAKER合约演示用例 |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_venachain/demo_venachain_mycontract.c |Venachain合约演示用例 |

编译Demo之前，需要修改Demo的C代码中以下部分：
- 对于ETHEREUM、PLATON、FISCO-BCOS、PLATONE、Venachain:
  1.	搜索`demoUrl`，将节点URL（含端口）填写为实际部署的节点或模拟器的IP地址和RPC端口
  2.	如果demo需使用原生私钥，则搜索`native_demoKey`，并将客户端私钥设置为：  
        -	对于ETHEREUM，设置为ganache生成的任意一个账户的私钥  
        - 对于PlatON，无需修改Demo中的私钥
        - 对于PlatONE，无需修改Demo中的私钥
        - 对于Venachain，无需修改Demo中的私钥
        - 对于FISCO-BCOS，设置为<FISCO-BCOS_ROOT>/console/accounts下私钥对应的原生格式私钥
  3.	如果demo需使用PKCS格式私钥，则搜索`pkcs_demoKey`，并将客户端私钥设置为：  
        - 对于以太坊，设置为ganache生成的任意一个账户的私钥对应的PKCS格式私钥
        - 对于PlatONE，无需修改Demo中的私钥
        - 对于FISCO-BCOS，设置为<FISCO-BCOS_ROOT>/console/accounts下私钥
  4.	搜索`demoRecipientAddress`，修改为Demo合约的部署地址。
- 对于FABRIC:  
  1. 搜索`fabric_client_demokey`，设置客户端使用的私钥
  2. 搜索`fabric_client_democert`，设置客户端私钥对应的证书
  3. 如果demo启用TLS，则搜索`fabric_org1_tlsCert`、`fabric_org2_tlsCert`、`fabric_order_tlsCert`，设置CA证书链
  4. 搜索`fabric_demo_endorser_peer0Org1_url`、`fabric_demo_endorser_peer1Org1_url`、`fabric_demo_endorser_peer0Org2_url`、`fabric_demo_endorser_peer1Org2_url`，设置背书节点、排序节点的url地址
  5. 如果demo启用TLS，则搜索`fabric_demo_endorser_peer0Org1_hostName`、`fabric_demo_endorser_peer1Org1_hostName`、`fabric_demo_endorser_peer0Org2_hostName`、`fabric_demo_endorser_peer1Org2_hostName`，设置节点的主机名称
- 对于HW-BCS:  
  1. 搜索`hw_bcs_client_demokey`，设置客户端使用的私钥
  2. 搜索`hw_bcs_client_democert`，设置客户端私钥对应的证书
  3. 如果demo启用TLS，则搜索`hw_bcs_org1_tlsCert`、`hw_bcs_org2_tlsCert`，设置CA证书链
  4. 搜索`hw_bcs_demo_endorser_peer0Org1_url`、`hw_bcs_demo_endorser_peer0Org2_url`、`hw_bcs_demo_order_url`，设置背书节点、排序节点的url地址
  5. 如果demo启用TLS，则搜索`hw_bcs_demo_endorser_peer0Org1_hostName`、`hw_bcs_demo_endorser_peer0Org2_hostName`、`hw_bcs_demo_order_hostName`，设置节点的主机名称
- 对于CHAINMAKER：
  1. 搜索 `chainmaker_user_key`  ，设置客户端使用的私钥
  2. 搜索`chainmaker_user_cert`，设置客户端私钥对应的证书
  3. 如果demo启用TLS，则搜索`chainmaker_tls_ca_cert`，设置CA证书
  4. 搜索`chainmaker_node_url`， 设置url的地址
  5. 如果demo启用TLS，则搜索`chainmaker_host_name`，设置节点的主机名称

#### 编译Demo
在\<BoAT-ProjectTemplate\>目录下执行以下命令编译SDK的调用Demo：
```
$make demo
```
生成的Demo程序分别位于\<BoAT-ProjectTemplate\>/build/BoAT-Engine/demo/demo_\<protocol\>/<demo_name>路径下，< protocol>可以为`ethereum`、`platon`、`fisco-bcos`、`platone`、`venachain`、`fabric`、`hwbcs`或`chainmaker`。



### 编译中的常见问题
1. 编译中提示类似“Makefile: 120: *** 缺失分隔符。 停止”的信息。  
该问题一般是因为Makefile中的target下的命令不是以Tab（ASCII码0x09）开头引起。注意按Tab键时，文本编辑器可能将Tab字符替换为若干空格。应将文本编辑器设置为不要用空格替代Tab。

2. 编译中提示“curl/curl.h”找不到  
该问题是因为系统中未安装curl及其开发文件引起。对于在Linux发行版上做Host编译而言，注意只安装curl包不够，还需要安装其开发文件包。开发文件包在不同的Linux发行版中有不同的名称，通常会命名为类似curl-devel，或者libcurl。具体请参照所使用的Linux发行版的软件包管理工具。    

   如果curl采用源码编译，且未安装到系统目录，则应在external.env中指定其搜索路径，并在链接时指定curl库所在路径。  

   在交叉编译中，尤其要注意搜索路径和库应指向交叉编译环境中的头文件和库，而不应指向执行编译的Host上的路径。

3. 交叉编译链接时提示字节序、位宽或ELF格式不匹配  
该问题通常是因为交叉编译中，部分库引用了Host的库，而Obj文件则是由交叉编译产生，或者，部分库为32位，另一部分为64位。应仔细核查所有库的路径，避免Host与Target的库混合链接，或者不同位宽的库混合链接。

   可以使用如下命令查看库文件是ARM版本还是x86版本，以及位宽：  
   ```
   $file \<lib或obj文件名\>
   ```
4. 编译中提示“openssl/evp.h”找不到
   该问题是因为系统中未安装OpenSSL及其开发文件引起。
   在 Ubuntu 系统中执行以下指令：
   ```
   sudo apt install libssl-dev
   ```
   提供openssl/evp.h头文件支持

5. 编译中提示可执行文件找不到，或者参数错误  
常见提示:  
'make'不是内部或外部命令，也不是可运行的程序或批处理文件。  
mkdir… 命令语法不正确。  
FIND: 参数格式不正确  

   该问题一般是因为在Windows下进行编译，但未安装Cygwin，或者未在Makefile中正确配置BOAT_RM、BOAT_MKDIR、BOAT_FIND的路径。请参照[以Windows为编译环境](#以windows为编译环境)章节安装Cygwin和配置Makefile。


## 编程模型

### 头文件和库
BoAT Infra Arch基础框架SDK编译完成后，应用可以通过SDK头文件和库，发起区块链交易或调用智能合约。

在SDK编译完成后，以下文件是应用在编译链接时需要的：
- \<BoAT-ProjectTemplate\>/BoAT-SupportLayer/include/BoAT-SupportLayer.conf文件中的所有头文件路径中包含的头文件
- \<BoAT-ProjectTemplate\>/BoAT-Engine/include/BoAT-Engine.conf文件中的所有头文件路径中包含的头文件
- \<BoAT-ProjectTemplate\>/lib下的所有库文件
- 如果使用了根据合约ABI JSON文件自动生成C接口代码，还应包含生成的智能合约C接口代码文件

1. 在应用中引用SDK头文件

- 在应用的头文件搜索路径中，增加上述BoAT-SupportLayer.conf和BoAT-Enigne.conf文件中的所有头文件路径，或者将这些头文件路径下所有头文件拷贝至应用头文件目录中。
   - **BoAT-SupportLayer/include/BoAT-SupportLayer.conf**
      ```
      BOAT_INCLUDE :=   -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/include \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/platform/include \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/keystore \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/keystore/SE/NXP/sscom/se_process \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/keystore/SE/NXP/sscom/smCom \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/keystore/SE/NXP/sscom/smCom/T1oI2C \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/platform/$(PLATFORM_TARGET)/src/log \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/platform/$(PLATFORM_TARGET)/src/dal \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/platform/$(PLATFORM_TARGET)/src/dal/http \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/platform/$(PLATFORM_TARGET)/src/osal \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/keystore/SE/NXP \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/keystore/soft \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/common/http2intf \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/third-party/protos \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/third-party/nghttp2/include \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/third-party/protobuf-c/include \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/platform/$(PLATFORM_TARGET)/src/inet \
                        -I$(BOAT_BASE_DIR)/BoAT-SupportLayer/tests \
                        -I$(BOAT_BASE_DIR)/demo\
		                $(BOAT_INCLUDE)
      ...
      ```
   - **BoAT-Engine/include/BoAT-Engine.conf**
      ```
	  BOAT_INCLUDE +=   -I$(BOAT_BASE_DIR)/BoAT-Engine/include \
                        -I$(BOAT_BASE_DIR)/BoAT-Engine/include/network \
                        -I$(BOAT_BASE_DIR)/BoAT-Engine/include/protocolapi \
                        -I$(BOAT_BASE_DIR)/BoAT-Engine/protocol/common/web3intf \
                        -I$(BOAT_BASE_DIR)/BoAT-Engine/protocol \
      ...
      ```
- 在应用相关C代码中，添加以下头文件:
  ```
  #include "boatiotsdk.h" //SDK的入口头文件
  #include "boatconfig.h" //SDK的配置头文件
  #include "boatEngine.h" //SDK的区块链应用头文件
  #include "boatosal.h"   //SDK的OSAL平台抽象接口头文件
  #include "boatdal.h"    //SDK的DAL驱动抽象接口头文件
  ```
  如果使用了根据合约ABI JSON文件自动生成C接口代码，还需包含生成的智能合约C接口代码的头文件，并将生成的`*.c`文件加入应用的编译脚本。


2. 在应用中链接SDK库文件

- 在应用的链接库中，依次增加\<BoAT-ProjectTemplate\>/lib中两个静态库：  
  `libboatengine.a`
  `libboatvendor.a`

- 在应用的链接库中，增加curl的动态库：  
  `-lcurl`

对于交叉编译，应当保证开发环境中的curl版本与目标板运行环境中的一致。


### SDK初始化和销毁
在调用SDK之前，必须调用BoatIotSdkInit()对SDK的全局资源进行初始化:
   ```
   BOAT_RESULT BoatIotSdkInit(void);
   ```
在使用结束后，应调用BoatIotSdkDeInit()释放资源:
   ```
   void BoatIotSdkDeInit(void);
   ```
### 密钥对创建/获取/删除

SDK支持两类密钥对：一次性密钥对和持久性密钥对。一次性密钥对在使用时临时创建，仅在内存中存在，关机后失效。持久性密钥对在创建时会做持久性保存，关机再重新开机后，可以加载之前已经创建的持久性密钥对。  
密钥对用于生成区块链钱包地址，SDK支持在同一个IoT设备中创建6个密钥对，其中1个一次性密钥对和5个持久性密钥对。密钥对创建后存储在系统中，每个密钥对对应一个存储索引号，IoT设备通过索引号获取存储在系统中的密钥对，根据应用需要将不同的密钥对应用在不同的场景。  
密钥对除了用于涉密应用外，也是构成区块链钱包的组成部分。  
***注：<BoAT-ProjectTemplate>/BoAT-SupportLayer/common/storage中的持久化实现方法仅供参考，在商业化产品中，建议根据实际硬件能力，考虑更安全的持久化方法。***

#### 创建密钥对
IoT设备通过密钥对创建函数生成密钥对，创建密钥对需要预先配置密钥对的属性，包括模式、类型、算法、格式等，创建密钥对生成私钥和公钥，并将密钥对存储在系统加密文件中，创建成功后将得到密钥对在系统中的存储索引号用于加载和删除密钥对。  
密钥对创建函数如下：
```
BOAT_RESULT BoatKeypairCreate(BoatKeypairPriKeyCtx_config *keypairConfig, 
                              BCHAR *keypairName, 
                              BoatStoreType storeType)
```
**参数:**

|参数名称              |参数描述                                                          |
|:---------------------|:-----------------------------------------------------------------|
|keypairConfig         |A pointer to a keypair configuration structure. See boatkeypair.h for more details.  |
|keypairName           |A string of keypaire name.<br>If the given \<keypairName\> is NULL, the SDK will generate an internal name for the current keypair.|
|storeType             |an one-time or persistent keypair type|


**返回值:**  
```
This function returns the non-negative index of the loaded keypair.
It returns -1 if keypair creation fails.
```
示例(生成一次性密钥对):
```
    BUINT8 keypairIndex = 255;
    BoatKeypairPriKeyCtx_config keypair_config = {0};

	/* keypair_config value assignment */
    keypair_config.prikey_genMode = BOAT_KEYPAIR_PRIKEY_GENMODE_INTERNAL_GENERATION;
    keypair_config.prikey_type    = BOAT_KEYPAIR_PRIKEY_TYPE_SECP256K1;


	/* create ethereum keypair */
    keypairIndex = BoatKeypairCreate( &keypair_config, keypairName,BOAT_STORE_TYPE_RAM);

```

#### 获取密钥对
完成创建密钥对后，IoT应用程序可以通过返回的密钥对索引号获取密钥对详细内容，包阔密钥对的：索引号、名称、格式、类型和公钥。
IoT设备应用程序可以通过BoATKeypair_GetKeypairByIndex函数加载密钥对,获取密钥对的基本信息，函数定义如下：
```
BOAT_RESULT BoATKeypair_GetKeypairByIndex(BoatKeypairPriKeyCtx *priKeyCtx, 
                                          BUINT8 index)
```

**参数:**

|参数名称              |参数描述                                                          |
|:---------------------|:-----------------------------------------------------------------|
|priKeyCtx             | A BoatKeypairPriKeyCtx structure pointer used to store key pair information read from the system. |
|index                 | The keypair index want to read.  |

**返回值:**  
```
This function returns 0 if the keypair was successfully obtained.
It returns -1 if obtaining the key pair fails.
```

#### 删除密钥对
IoT设备应用程序可以通过BoATIotKeypairDelete函数删除index对用的可加载密钥对。
```
BOAT_RESULT BoATIotKeypairDelete(BUINT8 index)
```
**参数:**

|参数名称              |参数描述                                                          |
|:---------------------|:-----------------------------------------------------------------|
|index                 | The index of the loadable keypair.  |

**返回值:** 
``` 
This function returns 0 if kepair deletion successful.
It returns -1 if keypair deletion fails.
```
### 区块链网络创建/获取/删除
不同的区块链其网络构成和应用方式尽皆不同，不同的区块链根据其网络特性创建其网络应用信息，存储在系统中，区块链网络是构成区块链钱包的组成部分。  
#### 区块链网络创建
区块链网路创建，将区块链应用所需的网络信息，存储到系统中，并通过返回的索引号，供IoT应用程序获取网络信息，网络信息的存储类型分为两类：一次性网络和持久性网络。
创建网络函数按照区块链分别实现，以下以Ethereum区块链为例说明，其他区块链网络相关函数，参见 BoAT-Engine/network/network_Xxx.c,Xxx为不同区块链名称。
以太坊区块链网络创建函数如下：
```
BOAT_RESULT BoATEthNetworkCreate(BoatEthNetworkConfig *networkConfig, 
                                 BoatStoreType storeType)
```
**参数：**
|参数名称              |参数描述                                                          |
|:---------------------|:-----------------------------------------------------------------|
|networkConfig         |A pointer to a Etherum network configuration structure. See network_ethereum.h for more details.  |
|storeType             |An one-time or persistent keypair type|

**返回值:**  
```
This function returns the non-negative index of the loaded Ethereum network.
It returns -1 if Ethereum network creation fails.
```
示例：（生成一次性网络信息）
```
    BUINT8 networkIndex = 255;
    BoatEthNetworkConfig network_config = {0};

    network_config.chain_id             = 1;
    network_config.eip155_compatibility = BOAT_FALSE;
    strncpy(network_config.node_url_str, demoUrl, BOAT_ETH_NODE_URL_MAX_LEN - 1);

	/* create ethereum network */
    networkIndex = BoATEthNetworkCreate( &network_config, BOAT_STORE_TYPE_RAM);
```

#### 获取网络信息
IoT应用程序通过区块链创建网络函数返回的索引号获取区块链网络信息。
```
BOAT_RESULT BoATEth_GetNetworkByIndex(BoatEthNetworkData *networkData, 
                                      BUINT8 index)
```
**参数：**
|参数名称              |参数描述                                                          |
|:---------------------|:-----------------------------------------------------------------|
|networkConfig         |A pointer to a Etherum network configuration structure. See network_ethereum.h for more details.  |
|storeType             |An one-time or persistent keypair type|

**返回值:**  
```
This function returns 0 if the network was successfully obtained.
It returns -1 if obtaining the network fails.
```

#### 删除网络信息
IoT设备应用程序可以通过各个区块链提供的网络信息删除接口函数，删除指定的区块链网络信息。
以太坊区块链删除网络信息函数如下：
```
BOAT_RESULT BoATEthNetworkDelete(BUINT8 index)
```
**参数:**

|参数名称              |参数描述                                                          |
|:---------------------|:-----------------------------------------------------------------|
|index                 | The index of the loadable Ethereum network.  |

**返回值:** 
``` 
This function returns 0 if network deletion successful.
It returns -1 if network deletion fails.
```

### 区块链钱包初始化/反初始化
钱包是区块链账号的属性集合，这些属性包括私钥、区块链节点URL等关键属性。在发起交易或调用智能合约前，必须初始化钱包。

#### 初始化钱包
SDK按照不同区块链创建各自的钱包。  
钱包按照使用时创建原则，通过密钥对和区块链网络信息索引号，分别获取密钥对和区块链网络信息，根据区块链对钱包的要求，通过各区块链钱包地址算法计算出钱包地址信息，存储在内存中，作为访问区块链的账号。  
创建钱包时，应保证总在同一个线程中调用钱包创建函数BoatXxxWalletInit()，其中Xxx是不同区块链的名称，列入BoatEthWalletInit()。  
在创建钱包时，需要根据具体区块链协议，传入钱包配置参数。例如以太坊Ethereum创建钱包的函数描述如下：
```
BoatEthWallet *BoatEthWalletInit(BUINT8 walletIndex, 
                                 BUINT8 networkIndex)
```
参数:

|参数名称              |参数描述                                                          |
|:---------------------|:---------------------------------------------|
|walletIndex           | The index of the loadable keypair.           |
|networkIndex          | The index of the loadable Ethereum network.  |

**返回值:**  
Returns 0 if the wallet initialization is successful.  
Returns -1 if the wallet initialization is fails.  

#### 反初始化钱包
SDK中各个区块链都设计了各自的区块链钱包反初始化函数，反初始化钱包将把已初始化的钱包从内存中卸载。卸载不会删除已经持久化的密钥对和区块链网络，但在再次初始化之前，该钱包将不能使用。
以太坊Ethereum区块链反初始化钱包函数如下：
```
void BoatEthWalletDeInit(BoatEthWallet *wallet_ptr)
```
参数:

|参数名称       |参数描述                   |
|:--------------|:--------------------------|
|wallet_ptr     |A Ethereum blockchain wallet structure pointer, pointing to the initialized Ethereum blockchain wallet structure variable.|

**返回值:**  
Returns 0 if the wallet deinitialization is successful.  
Returns -1 if the wallet deinitialization is fails.  


### 转账调用
从本账户向其他账户进行token转账（并非所有区块链协议都支持转账）。

以以太坊为例:
```
BOAT_RESULT BoatEthTransfer(BoatEthTx *tx_ptr,
                            BCHAR *value_hex_str);
```

参数:

|参数名称         |参数描述                                                                |
|:----------------|:-----------------------------------------------------------------------|
|tx_ptr           |Transaction pointer.                                                    |
|value_hex_str    |A string representing the value (Unit: wei) to transfer, in HEX format like "0x89AB3C".<br>Note that decimal value is not accepted. If a decimal value such as "1234" is specified, it's treated as "0x1234".|

**返回值:**  
This function returns BOAT_SUCCESS if transfer is successful.
Otherwise it returns one of the error codes.

### 合约调用（自动生成）

#### 自动生成合约的限制
由于合约编程语言一般支持面向对象，而C语言不支持面向对象，无法使用统一范式传递对象，因此只有参数类型与C语言内置类型一致的合约函数，可以通过工具转换为C调用代码。

对于Solidity编写的合约，工具支持以下参数类型：
  - address
  - bool
  - uint8
  - uint16
  - uint32
  - uint64
  - uint128
  - uint256
  - int8
  - int16
  - int32
  - int64
  - int128
  - int256
  - bytes1
  - bytes2
  - bytes3
  - bytes4
  - bytes5
  - bytes6
  - bytes7
  - bytes8
  - bytes9
  - bytes10
  - bytes11
  - bytes12
  - bytes13
  - bytes14
  - bytes15
  - bytes16 
  - bytes17
  - bytes18
  - bytes19
  - bytes20
  - bytes21
  - bytes22
  - bytes23
  - bytes24
  - bytes25
  - bytes26
  - bytes27
  - bytes28
  - bytes29
  - bytes30
  - bytes31
  - bytes32
  - bytes
  - string
  - T[N] : N > 0，N是整数，T是上面已支持的任意类型.
  - T[]  : T是上面已支持的任意类型，除了T[N].

对于C++编写的WASM合约，工具支持以下参数类型:
  - string
  - uint8
  - uint16
  - uint32
  - uint64
  - int8
  - int16
  - int32
  - int64
  - void

若合约函数中包含不支持的参数类型，自动转换工具将无法完成转换，必须手工编写合约。

#### 自动生成的合约调用接口
成功生成的合约调用接口为如下形式的C函数:
```
BCHAR * <合约ABI JSON文件名 >_< 合约函数名>(<钱包类型> *tx_ptr, …);
```
例如:
```
BCHAR * StoreRead_saveList(BoatEthTx *tx_ptr, Bbytes32 newEvent);
```
调用接口的第一个参数总为已初始化的交易对象的指针。

如果调用成功，调用接口的返回值是一个JSON格式的字符串。

***注：该字符串的内存在SDK内部分配，应用程序必须将该字符串复制到应用分配的内存中再做后续使用，不得修改返回的字符串的内容，或将该字符串的指针保存使用。***

改变区块链状态的合约函数，是指任何会改变区块链账本中的持久化存储信息的函数。例如，会对合约中的成员变量进行写入修改的函数，都是改变区块链状态的合约函数。如果一个合约函数只修改函数内的局部变量，而不会修改合约成员变量，那么该合约函数是不改变区块链状态的合约函数。

有一些合约编程语言中，两类合约函数在原型上有明确的修饰符区别，而另一些编程语言，没有明显的标识。在ABI中，这两类函数会有不同的标识。

例如，以如下以太坊Solidity合约为例:
```
contract StoreRead {
    
    bytes32[] eventList;
    
    function saveList(bytes32 newEvent) public {
        eventList.push(newEvent);
    }
    
    function readListLength() public view returns (uint32 length_) {
        length_ = uint32(eventList.length);
    }

    function readListByIndex(uint32 index) public view returns (bytes32 event_) {
        if (eventList.length > index) {
            event_ = eventList[index];
        }
    }
}
```
合约编译后，三个函数对应的ABI描述如下:
```
"abi": [
    {
      "constant": false,
      "inputs": [
        {
          "name": "newEvent",
          "type": "bytes32"
        }
      ],
      "name": "saveList",
      "outputs": [],
      "payable": false,
      "stateMutability": "nonpayable",
      "type": "function",
      "signature": "0xe648ba32"
    },
    {
      "constant": true,
      "inputs": [],
      "name": "readListLength",
      "outputs": [
        {
          "name": "length_",
          "type": "uint32"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function",
      "signature": "0xd0a80818"
    },
    {
      "constant": true,
      "inputs": [
        {
          "name": "index",
          "type": "uint32"
        }
      ],
      "name": "readListByIndex",
      "outputs": [
        {
          "name": "event_",
          "type": "bytes32"
        }
      ],
      "payable": false,
      "stateMutability": "view",
      "type": "function",
      "signature": "0xaa1a7122"
    },
]
```
上述合约中，eventList是合约的成员变量。saveList()会改变eventList的值，是改变区块链状态的合约函数；readListLength()和readListByIndex()函数有view修饰符，只读取eventList的值，是不改变区块链状态的合约函数。

特别注意，尽管改变区块链状态和不改变区块链状态的合约函数在原型上非常相似，但两者的调用原理有很大差别。通过该工具生成的C接口函数也是同样情况。


对区块链状态的任何改变，都需要通过区块链交易实施并在全网达成共识。改变区块链状态的合约函数是异步调用，在调用时，只是发起该次区块链交易，而在区块链网络将该交易打包出块前，该合约不会被执行。拿StoreRead的demo举例，相关调用代码如下所示：    
```
  BCHAR *result_str;
  result_str = StoreRead_saveList(&tx_ctx, (BUINT8*)"HelloWorld");
```
通过StoreRead_saveList函数获得的返回值result_str仅仅是用于标识该次交易的哈希值，而不是该StoreRead合约saveList函数的返回值。所以，在设计智能合约时，改变区块链状态的对外（public）接口函数，其试图返回的信息，应当保存在合约成员变量中。如果想获得经过更改后区块链的值，参考以下代码，其中hash为上一步中获得的交易的哈希值。
```
    BUINT32 list_len;
    if (BOAT_SUCCESS == BoatEthGetTransactionReceipt(hash))
    {
        list_len = StoreRead_readListLength(&tx_ctx);
        result_str = StoreRead_readListByIndex(&tx_ctx, list_len - 1);
    }
```
当BoatEthGetTransactionReceipt(hash)的返回值为BOAT_SUCCESS时，说明相应的改变区块链状态的合约函数调用已被成功上链。假设该合约只会由用户一人改变状态，然后通过StoreRead_readListLength()获得eventList数组的长度list_len，再通过StoreRead_readListByIndex()函数的调用获得StoreRead_saveList()上传上去的值"HelloWorld"。
而不改变区块链状态的合约函数，只需区块链节点读取其数据库内的已有信息，无需交易和共识。因此对该类函数的调用是同步调用，其返回值是就是相应合约函数的返回值。StoreRead合约的readListLength和readListByIndex两个函数就是此类合约函数。

***注：该处代码只能算伪代码，为了方便理解，进行了一定的简化。实际使用过程中，还需要对返回值进行相应的转换，具体细节可以参考demo_ethereum_storeread.c。***



#### 交易初始化
调用自动生成的合约接口，首先应初始化一个交易对象，然后调用生成的合约接口。
即使调用的合约函数是不改变区块链状态的，也需要先初始化一个交易对象。

以以太坊为例（不同的区块链协议参数有所差异）:
```
BOAT_RESULT BoatEthTxInit(BoatEthWallet *wallet_ptr,
                          BoatEthTx *tx_ptr,
                          BBOOL is_sync_tx,
                          BCHAR *gasprice_str,
                          BCHAR *gaslimit_str,
                          BCHAR *recipient_str)
```
参数:

|参数名称          |参数描述                                                                                        |
|:----------------|:-----------------------------------------------------------------------------------------------|
|wallet_ptr       |The wallet pointer that this transaction is combined with.                                      |
|tx_ptr           |Pointer a transaction object.                                                                   |
|is_sync_tx       |For a stateful transaction, specify BOAT_TRUE to wait until the transaction is mined.<br>Specifiy BOAT_FALSE to allow multiple transactions to be sent continuously in a short time.<br>For a state-less contract call, this option is ignored.|
|gasprice_str         |A HEX string representing the gas price (unit: wei) to be used in the transaction.<br>Set \<gasprice\> = NULL to obtain gas price from network.<br>BoatEthTxSetGasPrice() can later be called to modify the gas price at any time before the transaction is executed.|
|gaslimit_str         |A HEX string representing the gas limit to be used in the transaction.<br>BoatEthTxSetGasLimit() can later be called to modify the gas limit at any time before the transaction is executed.|
|recipient_str    |A HEX string representing the recipient address, in HEX format like"0x19c91A4649654265823512a457D2c16981bB64F5".<br>BoatEthTxSetRecipient() can later be called to modify the recipient at any time before the transaction is executed.|

**返回值:**  
This function returns BOAT_SUCCESS if initialization is successful.
Otherwise it returns one of the error codes.

示例:
```
BoatEthTx tx_ctx;
BOAT_RESULT result;
result = BoatEthTxInit(
                       wallet_ptr,
                       &tx_ctx,
                       BOAT_TRUE,
                       NULL,
                       "0x333333",
                       "0x9e7f3ae22cf97939a2e4cd68dd33bb29268a1ec9");
```
#### 调用合约接口
完成交易初始化后，可以调用自动生成的合约接口：

例如:
```
BCHAR *result_str;
result_str = StoreRead_saveList(&tx_ctx, (BUINT8*)"HelloWorld");
```
#### 自动生成合约的常见问题
**Q：编译时报如下错误**
```
    for abi_item in self.abi_object['abi']:
TypeError: list indices must be integers, not str
```
A：产生该问题的原因是使用了错误的输入文件。自动生成合约需要的JSON文件为编译器生成的完整JSON文件，如果仅仅拷贝ABI部分的内容自己创建一个JSON的话，需要在最外层增加ABI。具体格式可以参考：  
https://github.com/aitos-io/BoAT-X-Framework/issues/355

### 手动构造合约调用
如果自动生成工具无法生成C调用接口，则需要手工构造交易报文。另外，因为Fabric/hwbcs的调用本身就非常便捷，不需要自动生成合约调用接口的工具，所以需要手动调用合约。

手工构造交易需要遵循具体区块链协议的ABI接口。

**例1：以太坊交易构造**
- **步骤1** 调用BoatEthTxInit()进行交易初始化
- **步骤2** 设置交易参数
  - 设置交易参数nonce：  
    ```
    BOAT_RESULT BoatEthTxSetNonce(BoatEthTx *tx_ptr,
    BUINT64 nonce);
    nonce通常设置为BOAT_ETH_NONCE_AUTO，从网络获取nonce值。
    ```
  - 需要的话，设置交易参数value（初始化的交易对象中，value默认为0）:  
    ```
    BOAT_RESULT BoatEthTxSetValue(BoatEthTx *tx_ptr,
    BoatFieldMax32B *value_ptr);
    ```
- **步骤3** 对于改变区块链状态的合约调用（交易），设置交易参数data:
  ```
  BOAT_RESULT BoatEthTxSetData(BoatEthTx *tx_ptr,
  BoatFieldVariable *data_ptr);
  ```
  其中，data_ptr的格式遵循以太坊ABI，包括合约函数原型的Keccak-256哈希的前4字节作为Function Selector，随后依次排列各个参数：
  https://solidity.readthedocs.io/en/develop/abi-spec.html  
- **步骤4** 发送交易
  - 对于改变区块链状态的合约调用，调用如下合约函数：
    ```
    BOAT_RESULT BoatEthTxSend(BoatEthTx *tx_ptr);
    ```
  - 对于不改变区块链状态的合约调用，调用State-less合约函数：
    ```
    BCHAR * BoatEthCallContractFunc(BoatEthTx *tx_ptr,
                                    BCHAR *func_proto_str,
                                    BUINT8 *func_param_ptr,
                                    BUINT32 func_param_len);
    ```
    其中，func_param_ptr的格式遵循与步骤3相同的规则。

**例2：PlatONE交易构造**
- **步骤1** 调用BoatPlatONETxInit()进行交易初始化，其中交易类型字段根据实际交易类型进行设置。
- **步骤2** 设置交易参数
  - 设置交易参数nonce：
    ```
    BOAT_RESULT BoatPlatoneTxSetNonce(BoatEthTx *tx_ptr, BUINT64 nonce); 
    ```
    nonce通常设置为BOAT_PLATONE_NONCE_AUTO，从网络获取nonce值。
  - 需要的话，设置交易参数value（初始化的交易对象中，value默认为0）:
    ```
    BOAT_RESULT BoatPlatoneTxSetValue(BoatEthTx *tx_ptr,
    BoatFieldMax32B *value_ptr);
    ```
- **步骤3** 对于改变区块链状态的合约调用（交易），设置交易参数data:
  ```
  BOAT_RESULT BoatPlatoneTxSetData(BoatEthTx *tx_ptr,
  BoatFieldVariable *data_ptr);
  ```
  其中，data_ptr按RLP编码，依次编入如下字段:
  ```
  {
    TransactionType (Fixed unsigned 64bit, BigEndian),
    FunctionName,
    FunctionArgument1,
    FunctionArgument2,
  …
  }
  ```
  RLP编码按以下方法:
  - a) 调用RlpInitListObject()初始化顶层LIST对象
  - b) 调用RlpInitStringObject()初始化第一个编码字段对象
  - c) 调用RlpEncoderAppendObjectToList()将第一个编码字段对象加入顶层LIST对象
  - d) 重复b和c，直至所有编码对象都加入顶层LIST对象
  - e) 调用RlpEncode()对顶层LIST对象及其子对象进行RLP编码
  - f) 调用RlpGetEncodedStream()获取编码后的码流
  - g) 完成合约调用后，调用RlpRecursiveDeleteObject()销毁顶层LIST对象及其所有子对象

- **步骤4** 发送交易
  - 对于改变区块链状态的合约调用，调用如下合约函数：
    ```
    BOAT_RESULT BoatPlatoneTxSend(BoatEthTx *tx_ptr);
    ```

  - 对于不改变区块链状态的合约调用，调用State-less合约函数
    ```
    BCHAR * BoatPlatoneCallContractFunc(BoatPlatoneTx *tx_ptr,
                                        BUINT8 *rlp_param_ptr,
                                        BUINT32 rlp_param_len);
    ```
    其中，rlp_param_ptr的格式遵循与步骤3相同的规则。
  - 具体调用方法，可参照SDK所附的Demo的自动生成代码，这些代码位于\<BoAT-ProjectTemplate\>/build/demo/demo_\<protocol\>/demo_contract下。

**例3：HyperlEdger Fabric交易构造**
- **步骤1** 调用BoatHlfabricTxInit()进行交易初始化，其中参数根据实际使用进行设置。
- **步骤2** 如果节点自动查询功能没有开启，调用BoatHlfabricWalletSetNetworkInfo()进行网络参数设置。
- **步骤3** 调用BoatHlfabricTxSetTimestamp()设置时间戳，实时时间通过硬件相应功能获取。
- **步骤4** 如果节点自动查询功能开启，调用BoatHlfabricDiscoverySubmit()获取当前联盟链中所有节点信息。
- **步骤5** 设置交易参数。  
    使用demo_fabric_abac.c中的代码举例：  
    ```
    result = BoatHlfabricTxSetArgs(&tx_ptr, "invoke", "a", "b", "10", NULL);
    ```
    Fabric所有的函数调用输入数据均为string类型。拿上述代码来说，"invoke"是abac链码中的函数名。"a","b","10"均为该函数的相应的三个输入，不论链码中的相应变量是什么类型，均以string的形视作为输入。这也是不需要自动生成合约调用接口工具的原因。  
- **步骤6** 发送交易。
  - 对于改变区块链状态的合约调用，调用BoatHlfabricTxSubmit函数：
    ```
    BOAT_RESULT BoatHlfabricTxSubmit(BoatHlfabricTx *tx_ptr);
    ```

  - 对于不改变区块链状态的合约调用，调用BoatHlfabricTxEvaluate合约函数：
    ```
    BOAT_RESULT BoatHlfabricTxEvaluate(BoatHlfabricTx *tx_ptr);
    ```
    当返回的结果为BOAT_SUCCESS时，说明调用成功。

**例4：HW-BCS交易构造**
- **步骤1** 调用BoatHwbcsTxInit()进行交易初始化，其中参数根据实际使用进行设置。
- **步骤2** 调用BoatHwbcsWalletSetNetworkInfo()进行网络参数设置。
- **步骤3** 调用BoatHwbcsTxSetTimestamp()设置时间戳，实时时间通过硬件相应功能获取。
- **步骤4** 设置交易参数。  
    使用demo_hw_bcs.c中的代码举例：  
    ```
    result = BoatHwbcsTxSetArgs(&tx_ptr, "initMarble", "a","1", NULL, NULL);
    ```
    hwbcs所有的函数调用输入数据均为string类型。拿上述代码来说，"initMarble"是hw链码中的函数名。"a","1"均为该函数的相应的两个输入，不论链码中的相应变量是什么类型，均以string的形视作为输入。这也是不需要自动生成合约调用接口工具的原因。  
- **步骤5** 发送交易。
  - 对于改变区块链状态的合约调用，调用BoatHwbcsTxSubmit函数：
    ```
    BOAT_RESULT BoatHwbcsTxSubmit(BoatHwbcsTx *tx_ptr);
    ```

  - 对于不改变区块链状态的合约调用，调用BoatHwbcsTxEvaluate合约函数：
    ```
    BOAT_RESULT BoatHwbcsTxEvaluate(BoatHwbcsTx *tx_ptr);
    ```
    当返回的结果为BOAT_SUCCESS时，说明调用成功。

**例5：CHAINMAKER交易构造**

* **步骤1** 调用BoatChainmakerTxInit()进行交易初始化，其中参数根据实际使用进行设置。

* **步骤2** 调用BoatChainmakerAddTxParam()  设置交易参数。

  代码示例：

  ```
  BoatChainmakerAddTxParam(&tx_ptr, 6, "time", "6543235", "file_hash", "ab3456df5799b87c77e7f85", "file_name", "name005", NULL);
  ```

* **步骤3** 调用BoatChainmakerContractInvoke() 发起交易。

* **步骤4** 调用BoatChainmakerContractQuery() 查询交易。

## SDK往RTOS移植的建议
若将SDK移植到RTOS上，一般应遵循以下几点:
1. 解除对curl的依赖  
    curl是一个Linux下的通信协议库，在SDK中用于支持http/https通信。区块链节点通常采用http/https协议进行通信。

    对于采用RTOS的模组，应当在\<BoAT-ProjectTemplate\>/BoAT-SupportLayer/platform/\<platform_name\>/src/rpc中增加对模组的http/https的接口的调用封装，并修改\<BoAT-ProjectTemplate\>/BoAT-SupportLayer/platform/\<platform_name\>/scripts/gen.py，关闭RPC_USE_LIBCURL并设置新增的RPC USE OPTION


2. 解除对文件系统的依赖  

    SDK中使用文件作为钱包的持久化保存方法。若RTOS不支持文件系统，应当修改\<BoAT-ProjectTemplate\>/BoAT-SupportLayer/platform/\<platform_name\>/src/dal/boatstorage.c中文件操作相关的`BoatGetStorageSize`, `BoatWriteStorage`, `BoatReadStorage`, `BoatRemoveFile`四个函数，将读/写文件修改为系统支持的持久化方法。


3. 内存裁剪  

    若目标系统内存较为紧张，以致无法装入时，可以尝试对内存进行裁剪。可以裁剪的点包括：

    a)	根据实际需要，在执行config.py是只选择需要的区块链，参见[选择区块链](#BoAT Infra Arch基础框架SDK配置)
    b)	根据实际情况，减小<BoAT-ProjectTemplate>/BoAT-Engine/include/network/network_<protocol\>.h中，节点URL字符串的存储空间BOAT_XXX_NODE_URL_MAX_LEN  
    c)	根据实际需要，减小<BoAT-ProjectTemplate>/BoAT-SupportLayer/include/keypair.h中，支持的密钥对数量BOAT_MAX_KEYPAIR_NUM  
	d)  根据实际情况，减小<BoAT-ProjectTemplate>/BoAT-Engine/include/boatengine.h中，支持的网络数量BOAT_MAX_NETWORK_NUM
    e)	根据实际情况，减小<BoAT-ProjectTemplate>/BoAT-SupportLayer/include/boatrlp.h中，一个LIST中支持的最大成员个数MAX_RLP_LIST_DESC_NUM  
    f)	根据实际情况，减小<BoAT-ProjectTemplate>/BoAT-Engine/protocol/common/web3intf/web3intf.h中，web3数据缓冲区的自增步长WEB3_STRING_BUF_STEP_SIZE


    如果经过上述裁剪，内存仍然过大无法装入，可以尝试：  
    a)	根据实际需要，采用针对具体交易参数的简易RLP编码方法，替代<BoAT-ProjectTemplate>/sdk/rlp中递归的通用RLP编码方法  
    b)	根据实际需要，裁剪实际不会用到的API  

## BoAT的扩展AT命令建议

### 创建密钥对 AT^BCKEYPAIR
|Command                                                                         |Response(s)                                              |
|:-----------------------------------------------------------------------------  |:------------------------------------------------------- |
|Write Command:<br>^BCKEYPAIR=\<store_type\>,\<keypair_name\>[,\<kepair_config\>] |^BCKEYPAIR: \<wallet_index\><br>OK<br>                      |
|Test Command:<br>^BCKEYPAIR=?                                                      |+BCKEYPAIR: (list of supported \<protocol_type\>s)<br>OK<br>|

功能：
创建密钥对，与BoatKeypairCreate()对应。

参数：
\<store_type\>: integer type
- 1: persistent
- 0: one-time

\<keypair_name\>: string type; 
a string of keypair name

\<keypair_config\>: string type;
A JSON string representing the keypair configuration of \<protocol_type\>. The exact format is manufacture specific.

\<keypair_index\>: integer type; 
the index of the created keypair

### 创建区块链网络 AT^BCNETWORK
|Command                                                                         |Response(s)                                              |
|:-----------------------------------------------------------------------------  |:------------------------------------------------------- |
|Write Command:<br>^BCNETWORK=\<protocol_type\>,\store_type\>[,\<network_config\>] |^BCNETWORK: \<wallet_index\><br>OK<br>                      |
|Test Command:<br>^BCNETWORK=?                                                      |+BCNETWORK: (list of supported \<protocol_type\>s)<br>OK<br>|

功能：
创建区块链网络，与BoatXxxNetworkCreate()对应。

参数：
\<protocol_type\>: integer type
- 1: Ethereum
- 2: HLFABRIC
- 3: PLATON
- 4: PLATONE
- 5: FISCOBCOS
- 6: HWBCS
- 7: Chainmaker
- 8: Venachain

\<store_type\>: integer type; 
- 1: persistent
- 0: one-time

\<network_config\>: string type;
A JSON string representing the network configuration of \<protocol_type\>. The exact format is manufacture specific.

\<network_index\>: integer type; 
the index of the created network

### 初始化钱包 AT^BCWALT
|Command                                                                         |Response(s)                                              |
|:-----------------------------------------------------------------------------  |:------------------------------------------------------- |
|Write Command:<br>^BCWALT=\<protocol_type\>,\<keypair_index\>[,\<network_index\>] |^BCWALT: <br>OK<br>                      |
|Test Command:<br>^BCWALT=?                                                      |+BCWALT: (list of supported \<protocol_type\>s)<br>OK<br>|

功能：
初始化钱包，与BoatXxxWalletInit()对应。

参数：
\<protocol_type\>: integer type
- 1: Ethereum
- 2: HLFABRIC
- 3: PLATON
- 4: PLATONE
- 5: FISCOBCOS
- 6: HWBCS
- 7: Chainmaker
- 8: Venachain

\<keypair_index\>: integer type; 
the index of the created keypair

\<network_index\>: integer type; 
the index of the created network


### 反初始化钱包AT^BUWALT
|Command**                               |Response(s)                                            |
|:-------------------------------------- |:----------------------------------------------------- |
|Write Command:<br>^BUWALT=<wallet_tpye> |<br>OK<br>                                             |
|Test Command:<br>^BUWALT=?              |+BUWALT: (list of loadeded \<wallet_index\>s)<br>OK<br>|

功能：
反初始化钱包，与BoatWalletUnload()对应。

参数:
\<wallet_tpye\>: integer type; wallet type to unload, previously protocol_type parameter entered by AT^BCNETWORK earlier

### 删除密钥对 AT^BDKEYPAIR
|Command                                  |Response(s)   |
|:--------------------------------------- |:------------ |
|Write Command:<br>^BDKEYPAIR=\<keypair_index\>|<br>OK<br>    |

功能：
删除已创建的持久化密钥对，与BoatKeypairDelete()对应。

参数:
\<keypair_index\>: integer type; the index of the stored keypair

### 删除密钥对 AT^BDKEYPAIR
|Command                                  |Response(s)   |
|:--------------------------------------- |:------------ |
|Write Command:<br>^BDKEYPAIR=\<network_index\>|<br>OK<br>    |

功能：
删除已创建的持久化区块链网络，与BoatXxxNetworkDelete()对应。

参数:
\<network_index\>: integer type; the index of the stored network

### 合约函数调用AT^BCALLFUNC
|Command                                                    |Response(s)   |
|:--------------------------------------------------------- |:------------ |
|Write Command:<br>^BCALLFUNC=\<tx_object\>|<br>OK<br>    |

功能：
发起合约调用，与根据合约ABI JSON文件生成的C调用接口对应。

参数:
\<tx_object\>: string type;
A JSON string representing the transaction object as per the generated C contract interface. The exact format is manufacture specific.


### 转账AT^BTRANS
|Command                                                           |Response(s)   |
|:---------------------------------------------------------------- |:------------ |
|Write Command:<br>^BTRANS=\<recipient\>,\<value\>|<br>OK<br>    |

功能：  
发起指定Wallet的转账（并非所有区块链都支持转账）。例如，对以太坊，与BoatEthTransfer()对应。

参数:  
\<recipient\>: string type;  
A HEX string representing the recipient (payee) address.

\<value\>: integer type; the value to transfer, represented by the smallest token unit of the protocol (e.g. for Ethereum, the unit is wei or 10-18 ether).
## 参考资料
[1].BoAT Infra Arch基础框架SDK Reference Manual, AITOS.20.70.100IF, V1.0.0
