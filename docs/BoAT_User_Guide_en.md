# BoAT Edge User Manual

## Introduction

### Overview
This document introduces the functionality and usage of BoAT Edge product, which is the implementation of the BoAT Infra Arch framework. The features of BoAT Edge product are dependent on the flexible configuration and combination capabilities of the BoAT Infra Arch framework.  
The intended audience of this document is customers and enthusiasts who integrate the BoAT Edge product software framework.  
This user manual is version 3.1.

### Abbreviations and Terms
|Term   |Explanation                               |
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


## Functionality and Architecture
BoAT Infra Arch is a C language blockchain application framework client software for IoT device MCU/cellular modules. It is easy to port to various modules and helps IoT applications based on IoT device MCU/cellular modules connect to the blockchain and implement business operations such as data on-chain. The BoAT Infra Arch framework SDK provides functions to IoT applications including initiating on-chain transactions, generating C interface code for smart contracts, invoking smart contracts, and managing blockchain keys.

**Supported Blockchains:**  
Ethereum/Polygon  
PlatON  
PlatONE  
FISCO-BCOS  
Hyperledger Fabric  
Huawei BCS  
Chainmaker  
VenaChain

**Supported Module Open SDKs:**  
ChinaMobile-ML307A-DCLN  
Fibocom-FG150  
Fibocom-FM650  
Fibocom-L610  
linux-default  
Mobiletek-L503C-6S  
Unisoc-8850   

**Supported Target Operating Systems:**  
Linux  
RTOS

**Supported Build Operating Systems:**  
Linux/Cygwin  

**Key Features:**  
Configuration of blockchain accounts (wallets)  
Generation of blockchain key pairs  
Creation/loading/unloading of blockchain accounts  
Transfer transactions  
Smart contract invocation (automatically generates C calling interfaces)  
Smart contract invocation (manual construction)

### System Positioning

The components of the BoAT Infra Arch framework SDK run on the application processor of IoT device MCU/cellular modules in the form of software libraries (libbaotxxx.a). The SDK is provided in the form of C source files and is compiled in the development environment of IoT device MCU/cellular modules.

For IoT device MCU/cellular modules in the form of OpenCPU, the components of the BoAT Infra Arch framework SDK are linked to the IoT application to form an IoT application program with blockchain connectivity.

Figure 2-1 shows the positioning of the BoAT Infra Arch framework (referred to as BoAT in the figure) in an OpenCPU module. BoAT acts as an application layer protocol and is located above the existing protocol stack of the module, providing blockchain services to IoT applications. The peer layer of BoAT is the blockchain network layer.

![Positioning of BoAT Infra Arch framework in the system](https://aitos-io.github.io/BoAT-X-Framework/zh-cn/images/BoAT_User_Guide_cn-F2-1-BoAT_in_system.png)
Figure 2-1 Positioning of BoAT Infra Arch framework in the system

For IoT device MCU/cellular modules in non-OpenCPU form, the components of the BoAT Infra Arch framework library are linked to the module firmware and extended by module manufacturers as AT commands for IoT applications on the host computer to call, which will not be discussed further.

### BoAT Infra Arch Framework SDK Architecture
The BoAT Infra Arch framework SDK consists of two layers: BoAT Engine and BoAT Support Layer, as shown in Figure 2-2.
BoAT Engine provides blockchain application-related business logic, including IoT device wallet application APIs, blockchain client interface protocols, and blockchain network interfaces.
BoAT Support Layer provides system-level abstraction interfaces for IoT applications, including BoAT system-independent common components, operating system API abstraction layer, and system device driver/feature abstraction layer. It also includes remote procedure call interfaces, common components, hardware-dependent components, and tool components.

![BoAT Infra Arch framework SDK architecture](https://github.com/phengao/hello-world/blob/master/boatsdk.png)  
Figure 2-2 BoAT Infra Arch framework SDK architecture

IoT applications based on the BoAT Infra Arch framework SDK implement blockchain access business by calling the application interfaces of BoAT Engine (as shown by the yellow arrows in the figure). At the same time, they can also access the underlying system APIs through the system abstraction interfaces provided by BoAT Support Layer (as shown by the orange arrows in the figure), enabling IoT applications to achieve cross-platform applications on the module platforms supported by BIA. When switching module platforms, you only need to change the compilation configuration and complete the compilation to generate the firmware, which can be downloaded and run on the corresponding module.

### BoAT Infra Arch Framework SDK Code Structure
The code of the BoAT Infra Arch framework SDK consists of multiple source code repositories, including:
- BoAT-Engine repository, which provides the source code and compilation directory for implementing blockchain application interfaces.
- BoAT-SupportLayer repository, which provides platform abstraction-related source code and compilation directory.
- BoAT-ProjectTemplate repository, which provides a script repository for building the compilation directory of BoAT Infra Arch framework applications.

#### BoAT-Engine
The code structure of BoAT Engine is as follows:
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

In BoAT Engine:  
The wallete directory contains Wallet APIs corresponding to various blockchains. It provides interfaces for IoT applications to call, including SDK common interfaces and wallet and transaction interfaces for different blockchain protocols.
The protocol directory implements Portocols for different blockchains. The blockchain client interface protocol mainly implements transaction interface protocols for different blockchains and interacts with blockchain nodes through RPC interfaces.
The network directory implements the Networks blockchain network interface, which mainly configures and manages different blockchain network nodes, allowing Wallet APIs to establish connections with blockchain nodes through network information.
The tools directory contains tool components that provide Python tools to generate C language contract invocation interfaces based on Solidity or WASM C++ smart contract ABI interfaces.
In the application, BoAT-Engine provides blockchain application API interfaces to the application program in the form of a static library. The static library generated by compiling BoAT-Engine is libboatengine.a.

Access the ![BoAT-Engine repository](https://github.com/aitos-io/BoAT-Engine).

#### BoAT-SupportLayer
The BoAT Support Layer code structure is as follows:
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
In the BoAT Support Layer:
The BoAT Common Components include various system-independent general-purpose software required by IoT applications. The relevant directories include:
common: Implements interface methods for remote procedure calls (RPC) for different communication carriers. It also includes interface calls for some common protocols (http2intf).

third-party: Uses third-party software to provide common functions such as RLP encoding, JSON encoding/decoding, and string processing.

keypair: Provides application-related interfaces for key pairs, such as creating key pairs, obtaining public keys, and deleting key pairs.

keystore: Provides interfaces for key storage.

OSAL component: Provides interfaces for calling general system APIs. It converts system API interfaces of different platforms into unified abstract interfaces for application layer use, including semaphores, mutexes, timers, tasks/threads, queues, etc.

DAL component: Provides interfaces for calling general system APIs. It converts system API interfaces of different platforms into unified abstract interfaces for application layer use, including i2c, uart, virtualAT, http, ssl, storage, etc. For example, cryptographic accelerators, secure storage, random numbers, etc., need to be ported according to specific hardware. The SDK also provides a set of fully software-based default implementations.

OSAL and DAL components are organized by platform. The platform directory under the platform directory includes the specific implementations of OSAL and DAL for each platform. For example, the linux-default directory under the platform directory in the above figure includes the osal and dal directories.
In the application, BoAT-SupportLayer provides platform abstraction API interfaces to the application program in the form of a static library. The static library generated by compiling BoAT-SupportLayer is libboatvendor.a.

Access the ![BoAT-SupportLayer repository](https://github.com/aitos-io/BoAT-SupportLayer).

#### BoAT-ProjectTemplate
```
<BoAT-ProjectTemplate>
|
|---BoATLibs.conf   | The configuration of using BoAT libs
|---config.py       | Used for creating a compilation directory for 
                    | applications based on the BoAT Infra Arch infrastructure.
```

BoATLibs.conf is used to configure the static libraries (libboatvendor.a/libboatengine.a) provided by the BoAT Infra Arch SDK used in the project.  
config.py is used to create a compilation directory for applications based on the BoAT Infra Arch infrastructure SDK.   

Access the ![BoAT-ProjectTemplate repository](https://github.com/aitos-io/BoAT-ProjectTemplate).

## Compilation of BoAT Infra Arch SDK

### Software Dependencies
The compilation of BoAT Infra Arch SDK relies on the following software:

|Dependency    |Requirements                             |Build Environment|Target Environment|
| :------------| :---------------------------------------| :--------------| :----------------|
|Host OS       |Linux, or Cygwin on Windows               |Required        |                  |
|Target OS     |Linux                                    |                |Required          |
|Compiler      |gcc, needs to support c99 (9.3.0 is tested)|Required        |                  |
|Cross-compiler|arm-oe-linux-gnueabi-gcc (4.9.2 is tested)|Required        |                  |
|Make          |GNU Make (4.3 is tested)                  |Required        |                  |
|Python        |Python 2.7 (Python 3 is also compatible)   |Required        |                  |
|curl          |libcurl and its development files (7.55.1 is tested)|Required on Linux default|Required on Linux default|

Before compiling and using the SDK, make sure these software are installed. On Ubuntu, you can use the apt install command to install the corresponding packages. On Cygwin, use the Setup program that comes with Cygwin to install the required tools.

- Ubuntu Installation
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
- Cygwin Installation  
  Run setup-x86_64.exe and install make, gcc, python, libcurl, and other tools as shown in the figure below:
   ![image](https://user-images.githubusercontent.com/81662688/130744353-3e6ad68c-7945-44e6-93ac-c8a468e8e0aa.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744453-08a1dff2-08d8-46d8-9732-de814b077ba7.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744464-409165ad-a743-401c-8ed2-68060cd4d9e1.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744485-e3d5a4ed-bd9a-4a44-b9fa-81066fe2165b.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744499-06029343-9f65-4f33-a094-01de4c8ae25e.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744525-e4614f6a-6f33-4dae-9d6c-2d0b5bb994ec.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744541-7645fe99-9c21-44e0-a3cc-fe84b102d0cf.png)
   ![image](https://user-images.githubusercontent.com/81662688/130744556-163fb5e4-0260-42d8-b8c1-4c78052bd7d1.png)
   

  On Windows, the SDK does not support compilation outside of Cygwin. If you must run it outside of Cygwin (e.g., using a cross-compiler with Windows as the build environment), please refer to the section [Windows as the Compilation Environment]Windows-as-the-Compilation-Environment) to adjust the compilation files.

  When porting the SDK to an RTOS, the dependency on libcurl needs to be ported or the RPC methods need to be rewritten.

### Compilation Preparation
#### BoAT Infra Arch SDK Source Code Path
In the path where the SDK source code is stored, starting from the root directory, the names of each level of directories should consist of alphanumeric characters, underscores, or hyphens. Special characters such as spaces, Chinese characters, plus signs, @, parentheses, and other special symbols should not be used.

For example, the following are suitable paths:
/home/developer/project-blockchain
C:\Users\developer\Documents\project

The following are unsuitable paths:
/home/developer/project+blockchain/, the path contains a plus sign.
C:\Documents and Settings\developer\project, the path contains a space.

If it is unavoidable to have the above unsuitable characters in the path, please use the following methods to work around:
- Linux: Create a symbolic link pointing to the SDK directory in a path without unsuitable characters: ln -s \<BoAT-ProjectTemplate\> boatiotsdk, and compile under the path of the symbolic link.
- Windows: Use the command SUBST Z: \<BoAT-ProjectTemplate\> to create a virtual drive Z: (or any other unused drive letter), and compile under the Z: drive.

#### Building Compilation Directory
The source code of BoAT-Engine and BoAT-SupportLayer included in the BoAT Infra Arch SDK cannot be directly downloaded and compiled. You need to first download the BoAT-ProjectTemplate compilation template repository to your local machine, and then run the configuration script locally to complete the **source code download**, **blockchain selection**, **target platform selection**, **generation of Makefile**, and **building of compilation directory**. The following steps explain the entire process in order.

+ **Download BoAT-ProjectTemplate Development Template**  
   The BoAT-ProjectTemplate compilation template can be downloaded from GitHub.  
   Open a terminal on Linux/Windows operating system and execute the following command to clone the BoAT-ProjectTemplate repository to your local machine:
   ```
   git clone https://github.com/aitos-io/BoAT-ProjectTemplate.git
   ```
   After successful execution, a BoAT-ProjectTemplate directory will be created in the current path, which will store the BoAT-ProjectTemplate development template cloned from GitHub. The content of the BoAT-ProjectTemplate/ directory after cloning is as follows:
   ```
   <BoAT-ProjectTemplate>
   |-- BoATLibs.conf
   |-- config.py
   |-- README.md
   |-- README_cn.md
   ```
   Alternatively, you can clone the BoAT-ProjectTemplate repository as a development project directory. For example, build a boatDevelop development directory:
   ```
   git clone https://github.com/aitos-io/BoAT-ProjectTemplate.git boatDevelop
   ```
   Clone the BoAT-Engine repository into the boatDevelop directory and develop within this directory.
   In this article, the \<BoAT-ProjectTemplate\> directory is used as the root directory of the development template.

+ **Configure BoATLibs.conf**  
   The BoATLibs.conf file in the \<BoAT-ProjectTemplate/\> directory is used to configure the open-source repositories used in the current project. The subsequent configuration script, config.py, reads the configuration information in the BoATLibs.conf file and clones the corresponding open-source repositories to the local machine.  
   The BoATLibs.conf file contains the BoAT-SupportLayer repository by default, with the following content:
   ```
   BoAT-SupportLayer
   ```
   When compiling the BoAT-Engine middleware, you need to add "BoAT-Engine" to the BoATLibs.conf file. The modified content is as follows:
   ```
   BoAT-SupportLayer
   BoAT-Engine
   ```
   BoAT-Engine is a cross-platform IoT blockchain application middleware developed based on the BoAT-SupportLayer in the BoAT Infra Arch framework. Therefore, the BoATLibs.conf file must include the BoAT-SupportLayer base library.
   
+ **Run the config.py Script**  
   Run the config.py script and follow the prompts to select the blockchain and target platform, and build the compilation directory.  
   Execute the script in the \<BoAT-ProjectTemplate/\> directory. There will be several interactions during the process. The detailed process is as follows.  
   Run the script:
   ```
   python3 config.py
   ```
   The explicit prompt is as follows:
   ```
   We will clone the BoAT- SupportLayer' repository, which may take several minutes
   
   Input the branch name or null:
   ```
   Press Enter directly to clone the "BoAT-SupportLayer" open-source repository's main branch.
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
   Press Enter to clone the "BoAT-Engine" open-source repository's main branch.  
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
   Press Enter, which is considered as Y: Yes; if you don't want to overwrite the Makefile, you can enter n, and the compilation configuration will end immediately.  
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
   Enter the selected blockchain in the application. Enter 9 to select the "VENACHAIN" blockchain.  
   ```
   input:9
   Blockchain selected:
    [9] VENACHAIN
   
   Select the platform list as below:
   [1] linux-default             : Default linux platform
   [2] Fibocom-L610              : Fibocom's LTE Cat.1 module
   [3] create a new platform
   ```
   Enter 1 to select the target platform as "linux-default", and the script will automatically generate the Makefile after selection.  
   ```
   1
   platform is : linux-default
   
   include BoAT-SupportLayer.conf
   
   include BoAT-Engine.conf
   
   ./BoAT-SupportLayer/demo/ False
   ./BoAT-Engine/demo/ True
   Configuration completed
   ```
   After the script is executed, the compilation directory for BoAT-Engine is built.  
   At this point, the relevant source code of BoAT-Engine has been downloaded to the local machine, and the compilation configuration of BoAT-Engine has been completed. The Makefile is generated according to the selected platform.  
   After the configuration is completed, the development directory will include the following content:
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
   After the script is executed, the directories \<BoAT-Engine\> and \<BoAT-SupportLayer\>, as well as the Makefile file, will be added.
   
#### External Environment Variables Referenced
Modify the environment variables in the following files of the SDK as needed:  
\<BoAT-ProjectTemplate\>/BoAT-SupportLayer/platform/\<platform_name\>/external.env: Configure the external compilation environment dependencies and hardware-related header file search paths.
When compiling on the host, if gcc and binutils are already installed in the system, it is usually not necessary to modify these environment variable configurations.

If cross-compiling, if the cross-compilation environment needs to configure specific INCLUDE paths, add the paths in the above file.

#### BoAT Infra Arch SDK Configuration
- Enable/Disable Blockchain Protocols  
   Run the config.py script in the BoAT-ProjectTemplate. If the BoATLibs.conf file contains the BoAT-Engine repository, you will be prompted to select a blockchain during the configuration process, with the following prompt:
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
   According to your needs, select one or more blockchain numbers to enable the corresponding blockchain protocols. The blockchain protocols that are not selected will be disabled. The above prompt provides an example of selecting a blockchain. When entering "1a", it means choosing to enable the "ETHEREUM" and "QUORUM" blockchains, and the remaining blockchains will be disabled, see [Building Compilation Directory]Building-Compilation-Directory) for details.

- Adjusting Log Printing Level
According to your needs, adjust the value of `BOAT_LOG_LEVEL` in the path `<BoAT-ProjectTemplate>/BoAT-SupportLayer/platform/<platform_name>/src/log/boatlog.h` to adjust the log printing level.

### Contract C Interface Code Generation
Smart contracts are executable code on the blockchain, executed on blockchain virtual machines (such as EVM and WASM), and called by clients through remote procedure calls (RPC).  
Different virtual machines and contract programming languages have different application binary interfaces (ABI). When clients call contract functions through RPC, they must assemble the interface according to the corresponding ABI.  
The BoAT-Engine repository in the SDK provides the following tools under the tools/ directory to generate C interface code based on the contract ABI, so that the generated interface code can be used to call smart contracts on the blockchain like calling regular C functions:
| Conversion Tool                                           | Purpose                                     |
|:---------------------------------------------------------|:--------------------------------------------|
| `<BoAT-ProjectTemplate>/BoAT-Engine/tools/eth2c.py`               | Generate C calling code based on Ethereum Solidity ABI      |
| `<BoAT-ProjectTemplate>/BoAT-Engine/tools/fiscobcos2c.py`         | Generate C calling code based on FISCO-BCOS Solidity ABI |
| `<BoAT-ProjectTemplate>/BoAT-Engine/tools/platoneSolidity2c.py`   | Generate C calling code based on PlatONE Solidity ABI    |
| `<BoAT-ProjectTemplate>/BoAT-Engine/tools/platoneWASM2c.py`       | Generate C calling code based on PlatONE WASM ABI        |
| `<BoAT-ProjectTemplate>/BoAT-Engine/tools/venachainSolidity2c.py` | Generate C calling code based on Venachain Solidity ABI  |
| `<BoAT-ProjectTemplate>/BoAT-Engine/tools/venachainWASM2c.py`     | Generate C calling code based on Venachain WASM ABI      |

Since contract programming languages generally support object-oriented programming, while C language does not support object-oriented programming and cannot pass objects using a unified paradigm, only contract functions with parameter types consistent with built-in C types can be converted to C calling code using the tools. For specific supported contract function input types, please refer to the [Contract Invocation (Auto-generated)]Contract-Call-Automatically-Generated) section.

Before making a call, you need to compile the contract and copy the generated ABI interface description JSON file to the corresponding directory in the SDK.
| ABI Storage Path                                          | Purpose                                             |
|:----------------------------------------------------------|:----------------------------------------------------|
| `<BoAT-ProjectTemplate>/demo/demo_ethereum/demo_contract`           | Copy the Ethereum ABI JSON file to this directory            |
| `<BoAT-ProjectTemplate>/demo/demo_fiscobcos/demo_contract`          | Copy the FISCO-BCOS ABI JSON file to this directory          |
| `<BoAT-ProjectTemplate>/demo/demo_platone/demo_contract/Solidity`   | Copy the PlatONE (Solidity) ABI JSON file to this directory  |
| `<BoAT-ProjectTemplate>/demo/demo_platone/demo_contract/WASM`       | Copy the PlatONE (WASM) ABI JSON file to this directory      |
| `<BoAT-ProjectTemplate>/demo/demo_venachain/demo_contract/Solidity` | Copy the Venachain (Solidity) ABI JSON file to this directory|
| `<BoAT-ProjectTemplate>/demo/demo_venachain/demo_contract/WASM`     | Copy the Venachain (WASM) ABI JSON file to this directory    |

***Note: The ABI JSON file must have the ".json" file name extension.***

During the compilation of the demo, the auto-generated tool will generate the corresponding C interface calling code based on the contract ABI JSON file. If the auto-generation of C interfaces fails during compilation, you need to manually delete unsupported ABI JSON files (or delete unsupported interfaces) from the corresponding directories in `<BoAT-ProjectTemplate>/contract` and write C code to assemble the ABI interfaces, see [Transfer Invocation]Transfer-Invocation) section for details.

### Host Compilation
Host compilation refers to compiling the software in an environment that is the same as the target environment, such as compiling x86 programs on an x86 machine. There are usually two scenarios for using host compilation:
1. During software debugging, test the software functionality on a PC.
2. The target software runs on a device based on an x86/x86-64 processor, such as some edge gateways.

#### Linux as the Compilation Environment
For host compilation based on a Linux distribution (such as Ubuntu), there is generally no need to configure the compilation environment specially, just ensure that the required software dependencies are installed.

The compilation follows the steps below:
1. Build the BoAT architecture project development template [build directory](#Building-Compilation-Directory) in a path that meets the requirements of the [SDK source code path](#BoAT-Infra-Arch-SDK-Source-Code-Path).
2. Optional: Place the ABI JSON file of the smart contract to be called in the corresponding directory under `<BoAT-ProjectTemplate>/BoAT-Engine/demo/demo_<protocol>/demo_contract` (see [Contract C Interface Code Generation](#Contract-C-Interface-Code-Generation) section).
3. In the `<BoAT-ProjectTemplate>` directory, execute the following command:
```
$ make all
```
4. After the compilation is completed, the generated library files are in the `./lib` directory. The application should include the header files under the include directory of BoAT-Engine and BoAT-SupportLayer, and link the static libraries `libboatengine.a` and `libboatvendor.a` under the `./lib` directory to implement the functionality of accessing the blockchain. See the [Header Files and Libraries](#Headers-and-Libraries) section for details.

#### Compiling with Cygwin as the Environment

On Windows, the SDK does not support compilation outside of the Cygwin environment, nor does it support using compilers other than gcc.

The compilation steps are the same as on Linux.

### Cross-Compilation

In cross-compilation, it is generally necessary to configure compilation configuration files based on the specific compilation environment.

#### Linux as the Compilation Environment

##### Independent Cross-Compilation Environment
An independent cross-compilation environment refers to having arm-oe-linux-gnueabi-gcc (or a similar cross-compiler) installed on the Linux system, which can be called independently.

The SDK requires that the following environment variables be set in the system to point to the cross-compilation environment:

| Environment Variable | Description                                 |
|:---------------------|:--------------------------------------------|
| CC                   | Points to the executable file of the cross-compiler gcc |
| AR                   | Points to the executable file of the cross-compiler ar  |

When the CC and AR environment variables are not defined in the environment, GNU make will default to CC=cc and AR=ar. Usually, the gcc and bintuils compilation environment of the host will be installed on the Linux system. Therefore, when the above environment variables are not defined, the host compilation will be performed.

When configuring the cross-compilation environment, it is usually necessary to execute a specific shell script to set the above environment variables to point to the cross-compilation environment. For the bash shell, a command similar to the following is usually executed:
```
$ source cross_compiler_config.sh
```
or
```
$ . cross_compiler_config.sh
```
In the example above, `cross_compiler_config.sh` is not a script in this SDK, but a configuration script in the cross-compilation environment. Please refer to the relevant documentation of the cross-compilation environment for the specific name and location.

The `source` or `.` in the example is required, which allows the script to be executed in the context of the current shell, so that the modifications to the environment variables in the script will only take effect in the current shell.

You can execute the following command to view the environment variable settings in the current shell:
```
$ export
```

If the CC and AR environment variables are set, you can execute the following command to view the versions of CC and AR in the current shell to confirm if they are pointing to the expected cross-compilation environment:
```
${CC} -v
${AR} -v
```

After completing the above configuration, follow the steps in the [Linux as the Compilation Environment](#Linux-as-the-Compilation-Environment) section for compilation.

##### Integrated Cross-Compilation Environment with Module Development Environment
Some OpenCPU modules integrate a compatible cross-compilation environment in their provided development environment, eliminating the need for customers to separately install a cross-compiler on the Linux system. This is especially useful for developing application software on multiple different models of modules on a single host computer without having to switch cross-compilation environments repeatedly.

#### Compiling with GNU Make as the Compilation Tool in the Module Development Environment

If the module development environment uses GNU Make as the compilation tool (with Makefiles in each level of the source code directory), you can adjust the compilation configuration of the BoAT Infra Arch SDK to integrate it into the module development environment for compilation.

Usually, the module development environment provides an Example of customer code and includes the compilation configuration for the customer example code in the compilation system. First, copy the `<BoAT-ProjectTemplate>` directory (replaced by the directory name `boatiotsdk` in the example) to the customer code Example directory in the module development environment, and then modify the Makefile for the customer example code to add a target for compiling the BoAT Infra Arch SDK.

For example:
Assume that in the original compilation environment, the Makefile for the customer example code is as follows:
```
.PHONY: all
all:
    $(CC) $(CFLAGS) example.c -o example.o

clean:
    -rm -rf example.o
```
Adjust it to:
```
.PHONY: all boatiotsdkall boatiotsdkclean
all: boatiotsdkall
    $(CC) $(CFLAGS) example.c -o example.o

clean: boatiotsdkclean
    -rm -rf example.o

boatiotsdkall:
    make -C boatiotsdk boatlibs

boatiotsdkclean:
    make -C boatiotsdk clean
```
In the above example, `boatiotsdk` is the directory where the SDK is located, and the `-C` parameter after `make` specifies that the compilation is performed according to the Makefile in the `boatiotsdk` directory.

***Note: In the Makefile, the commands under the target must start with a tab character (ASCII code 0x09) and not with spaces.***

The above steps are only used to compile the SDK library. After the SDK library is compiled, the compiled lib files need to be integrated into the module development environment. See the [Header Files and Libraries](#Headers-and-Libraries) section for details.

#### Module Development Environment Using Non-GNU Make Compilation Tool

Since the BoAT Infra Arch SDK uses GNU Make as the compilation tool, if the module development environment uses a non-GNU Make compilation tool (such as Ninja, ant, etc.) or uses a tool for automatically generating the compilation project (such as automake, CMake), the SDK cannot be compiled directly in the module development environment.

To compile the SDK in such a module development environment, the gcc and binutils compilation tools in the module development environment need to be released and the environment variables described in the [Independent Cross-Compilation Environment](#Independent-Cross-Compilation-Environment) section need to be configured so that they can be called in the system, similar to an independent cross-compilation environment. Then, compile the SDK.

#### Windows as the Compilation Environment

In Windows, the SDK does not support compilation outside of Cygwin. If the cross-compiler used in the Windows compilation environment can only run outside of Cygwin, adjustments need to be made to the compilation environment and configuration files.

When cross-compiling outside of Cygwin, Cygwin still needs to be installed, and the Makefile needs to be adjusted.

##### Installing Cygwin

The SDK compilation project relies on some Cygwin tools. The required tools are as follows:

| Tool      | Purpose                                                                                                               |
|:--------- |:--------------------------------------------------------------------------------------------------------------------- |
| find      | The Cygwin find.exe is required for recursively searching for the subdirectories to be compiled. The FIND.EXE provided by Windows has a completely different function and cannot be used. |
| rm        | Used to delete specified directories and files. The RMDIR/RD and DEL commands built into the Windows cmd shell can only be used to delete directories (trees) and files, respectively, and their syntax is not compatible with Cygwin's rm.exe. |
| mkdir     | Used to create one or more levels of directories. The MKDIR/MD command built into the Windows cmd shell has the same function, but its syntax is not compatible.
|GNU make   | GNU make can be installed in Cygwin, or GNU make can be compiled on Windows using a compiler such as Microsoft Visual Studio. The latter does not depend on Cygwin.      |

After installing Cygwin, the path needs to be configured. Since some Cygwin tools that the SDK compilation depends on have the same names as the built-in Windows tools, it is necessary to ensure that the relevant tools referenced in the compilation point to the Cygwin version.

First, in the cmd shell where the compilation is performed, execute the following command to add the search path of Cygwin:
```
set PATH=%PATH%;\<Path_to_Cygwin\>\bin  
```
where <Path_to_Cygwin> is the absolute path of the Cygwin installation directory, such as: C:\Cygwin64  


***Note: The above command can be written in a batch file (bat) or directly added to the Windows system environment variable for easy invocation. If added directly to the Windows system environment variable, do not place Cygwin before the %SystemRoot%\System32 path, otherwise, when calling the Windows FIND command in other scenarios, the find version of Cygwin will be mistakenly called, which will affect the use of the built-in Windows command in other scenarios.***

Then, modify `<BoAT-ProjectTemplate>/BoAT-SupportLaer/platform/<platform_name>/external.env` and add the paths for the dependent tools:
```
# Commands
CYGWIN_BASE := C:\cygwin64  # Modify to actual path to Cygwin
BOAT_RM := $(CYGWIN_BASE)\bin\rm -rf
BOAT_MKDIR := $(CYGWIN_BASE)\bin\mkdir
BOAT_FIND := $(CYGWIN_BASE)\bin\find

```

Taking Windows 10 as an example, the method to add the search path to the Windows environment variable is as follows:
a) Right-click on the Windows logo menu and select "System".
b) Click on "System Information" in the "About" page.
c) Click on "Advanced system settings" in the "System" page.
d) Click on "Environment Variables" in the "System Properties" page.
e) In the "Environment Variables" page, click on "Path" in the "System variables" section, and click on "Edit".
f) In the "Edit Environment Variable" page, click on "New" and add the bin path of the Cygwin installation directory, and make sure that the newly added path is located after the path %SystemRoot%\system32 in any position.

###### Other Adjustments
When cross-compiling outside of Cygwin, in addition to the adjustments mentioned in the previous section, the following adjustments need to be made:

1. Try running make, if there is a path error, modify the corresponding path separators in the Makefile from "/" to "\\". Do not change all "/" to "\\" at the beginning, as some Windows versions of tools derived from Linux can recognize "/" as a path separator.
2. Configure the environment variables described in the [Independent Cross-Compilation Environment](#Independent-Cross-Compilation-Environment) section to point to the correct cross-compilation environment. In these environment variables, the path should be separated by "\\".

### Compiling and Running the Demo
#### Preparation
The BoAT Infra Arch SDK provides demos based on Ethereum, PlatON, PlatONE, FISCO-BCOS, Hyperledger Fabric, HW-BCS, Venachain, and Chainmaker in the BoAT-Engine library. Before running these demos, you need to install the corresponding blockchain node software (or have known nodes) and deploy the smart contracts required by the demos.

The smart contracts used by the demos and their ABI JSON files are placed in:
|Demo Smart Contract                                                |Contract ABI JSON File                                                |Purpose           |
|:----------------------------------------------------------- |:------------------------------------------------------------ |:------------ |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_ethereum/demo_contract/StoreRead.sol   |\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_ethereum/demo_contract/StoreRead.json   |Ethereum Demo     |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_platone/demo_contract/WASM/my_contract.cpp    |\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_platone/demo_contract/WASM/my_contract.cpp.abi.json    |PlatONE Demo    |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_fiscobcos/demo_contract/HelloWorld.sol |\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_fiscobcos/demo_contract/HelloWorld.json |FISCO-BCOS Demo |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_venachain/demo_contract/WASM/mycontract.cpp    |\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_venachain/demo_contract/WASM/mycontract.cpp.abi.json    |Venachain Demo   |

Before running the Ethereum demo, you need to install the Ethereum node simulator ganache and the Truffle smart contract compilation and deployment tool.
Ganache and Truffle can be accessed from this website: https://truffleframework.com
Ganache has a command-line interface version called ganache-cli, and a graphical interface version called Ganache. The command-line version ganache-cli and the graphical version Ganache 1.x do not persist data. If the process of ganache-cli or Ganache 1.x is terminated, the deployed contracts will be lost, and you need to redeploy the contracts using the command truffle migrate --reset. The addresses of the redeployed contracts may change. The graphical version Ganache 2.x can create a Workspace to save the state. After closing and reopening the Workspace, the deployed contracts will still be available without redeployment.
In addition to using the ganache simulator, you can also use Ethereum test networks such as Ropsten (requires free test tokens).

Before running the PlatON demo, you need to install the PlatON node.
The source code and tools for PlatON can be accessed from this website: https://platon.network/

Before running the PlatONE demo, you need to install the PlatONE node and the smart contract compilation and deployment tool.
The source code and tools for PlatONE can be accessed from this website: https://platone.wxblockchain.com

Before running the Venachain demo, you need to install the Venachain node and the smart contract compilation and deployment tool.
The source code and tools for Venachain can be accessed from this website: https://venachain-docs.readthedocs.io/zh/latest/documents/quick/env.html

Before running the FISCO-BCOS demo, you need to install the FISCO-BCOS node and deploy the contracts.
The source code and installation deployment steps for FISCO-BCOS can be accessed from this website: https://fisco-bcos-documentation.readthedocs.io
After completing the deployment of the node (or simulator), you need to follow the instructions on the relevant website to deploy the Demo smart contract. Once the smart contract is successfully deployed, a contract address will be generated.

The Demo C code for invoking the smart contract is located in:
|Demo C Code                                                 |Purpose                 |
|:---------------------------------------------------------- |:--------------------- |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_ethereum/demo_ethereum_storeread.c    |Ethereum contract demo |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_ethereum/demo_ethereum_transfer.c     |Ethereum transfer demo |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_platon/demo_platon_transfer.c         |PlatON transfer demo   |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_platone/demo_platone_mycontract.c     |PlatONE contract demo  |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_fiscobcos/demo_fiscobcos_helloworld.c |FISCO-BCOS contract demo |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_fabric/demo_fabric_abac.c             |Fabric contract demo   |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_hw_bcs/demo_hw_bcs.c                  |HW-BCS contract demo   |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_chainmaker/demo_chainmaker.c |CHAINMAKER contract demo |
|\<BoAT-ProjectTemplate\>/BoAT-Engine/demo/demo_venachain/demo_venachain_mycontract.c |Venachain contract demo |

Before compiling the Demo, you need to modify the following parts in the Demo C code:
- For ETHEREUM, PLATON, FISCO-BCOS, PLATONE, Venachain:
  1. Search for `demoUrl` and fill in the actual IP address and RPC port of the deployed node or simulator.
  2. If the demo requires the use of a native private key, search for `native_demoKey` and set the client private key as follows:
     - For ETHEREUM, set it to the private key of any account generated by Ganache.
     - For PlatON, no modification is needed in the Demo code.
     - For PlatONE, no modification is needed in the Demo code.
     - For Venachain, no modification is needed in the Demo code.
     - For FISCO-BCOS, set it to the native format private key corresponding to the private key in `<FISCO-BCOS_ROOT>/console/accounts`.
  3. If the demo requires the use of a PKCS format private key, search for `pkcs_demoKey` and set the client private key as follows:
     - For Ethereum, set it to the PKCS format private key corresponding to the private key of any account generated by Ganache.
     - For PlatONE, no modification is needed in the Demo code.
     - For FISCO-BCOS, set it to the private key in `<FISCO-BCOS_ROOT>/console/accounts`.
  4. Search for `demoRecipientAddress` and modify it to the deployment address of the Demo contract.
- For FABRIC:
  1. Search for `fabric_client_demokey` and set the private key used by the client.
  2. Search for `fabric_client_democert` and set the certificate corresponding to the client's private key.
  3. If the demo is using TLS, search for `fabric_org1_tlsCert`, `fabric_org2_tlsCert`, and `fabric_order_tlsCert` to set the CA certificate chain.
  4. Search for `fabric_demo_endorser_peer0Org1_url`, `fabric_demo_endorser_peer1Org1_url`, `fabric_demo_endorser_peer0Org2_url`, and `fabric_demo_endorser_peer1Org2_url` to set the URL addresses of the endorsement and ordering nodes.
  5. If the demo is using TLS, search for `fabric_demo_endorser_peer0Org1_hostName`, `fabric_demo_endorser_peer1Org1_hostName`, `fabric_demo_endorser_peer0Org2_hostName`, and `fabric_demo_endorser_peer1Org2_hostName` to set the host names of the nodes.

- For HW-BCS:
  1. Search for `hw_bcs_client_demokey` and set the private key used by the client.
  2. Search for `hw_bcs_client_democert` and set the certificate corresponding to the client's private key.
  3. If the demo is using TLS, search for `hw_bcs_org1_tlsCert` and `hw_bcs_org2_tlsCert` to set the CA certificate chain.
  4. Search for `hw_bcs_demo_endorser_peer0Org1_url`, `hw_bcs_demo_endorser_peer0Org2_url`, and `hw_bcs_demo_order_url` to set the URL addresses of the endorsement and ordering nodes.
  5. If the demo is using TLS, search for `hw_bcs_demo_endorser_peer0Org1_hostName`, `hw_bcs_demo_endorser_peer0Org2_hostName`, and `hw_bcs_demo_order_hostName` to set the host names of the nodes.

- For CHAINMAKER:
  1. Search for `chainmaker_user_key` and set the private key used by the client.
  2. Search for `chainmaker_user_cert` and set the certificate corresponding to the client's private key.
  3. If the demo is using TLS, search for `chainmaker_tls_ca_cert` and set the CA certificate.
  4. Search for `chainmaker_node_url` and set the URL address.
  5. If the demo is using TLS, search for `chainmaker_host_name` and set the host name of the node.

#### Compiling the Demo
To compile the SDK's demo, execute the following command in the `<BoAT-ProjectTemplate>` directory:
```
$ make demo
```
The generated demo programs will be located in the `<BoAT-ProjectTemplate>/build/BoAT-Engine/demo/demo_\<protocol\>/<demo_name>` directory, where `<protocol>` can be `ethereum`, `platon`, `fisco-bcos`, `platone`, `venachain`, `fabric`, `hwbcs`, or `chainmaker`.

### Common Compilation Issues
1. Error message like "Makefile: 120: *** missing separator. Stop." during compilation.  
This issue is usually caused by commands under targets in the Makefile not starting with a tab (ASCII code 0x09). Make sure to use a tab character instead of spaces when indenting commands in the Makefile.

2. Error message "curl/curl.h" not found during compilation.  
This issue occurs when the curl library and its development files are not installed on the system. When compiling on a Linux distribution, make sure to install both the curl package and its development package. The name of the development package may vary depending on the Linux distribution, but it is usually named something like `curl-devel` or `libcurl`. Please refer to the package management tool of your Linux distribution for specific instructions.

   If curl is compiled from source and not installed in the system directory, you should specify its search path in `external.env` and specify the path to the curl library during linking.
   In cross-compilation, it is especially important to ensure that the search paths and libraries point to the headers and libraries in the cross-compilation environment, rather than the paths on the host where the compilation is being performed.

3. Cross-compilation linking prompts byte order, bit width, or ELF format mismatch  
This issue is usually caused by libraries referencing the host's libraries in the cross-compilation process, while the object files are generated by cross-compilation, or when some libraries are 32-bit and others are 64-bit. Carefully check all library paths to avoid mixing host and target libraries or libraries with different bit widths.

You can use the following command to check whether a library file is an ARM version or an x86 version, and its bit width:  
```
$file <lib or obj file name>
```
4. Compilation prompts "openssl/evp.h" not found  
This issue is caused by the lack of OpenSSL and its development files in the system.  
On Ubuntu, execute the following command:  
```
sudo apt install libssl-dev
```
This provides support for the openssl/evp.h header file.

5. Compilation prompts executable file not found or parameter error  
Common prompts:  
'make' is not recognized as an internal or external command, operable program or batch file.  
mkdir... command syntax is incorrect.  
FIND: Invalid parameter format.  

This issue is usually caused by compiling on Windows without installing Cygwin or not correctly configuring the paths for BOAT_RM, BOAT_MKDIR, and BOAT_FIND in the Makefile. Please refer to the section "Compiling Environment on Windows" for installing Cygwin and configuring the Makefile.

## Programming Model

### Headers and Libraries
After the compilation of the BoAT Infra Arch basic framework SDK is completed, applications can initiate blockchain transactions or invoke smart contracts using the SDK headers and libraries.

After the SDK compilation, the following files are required for compiling and linking the application:
- All header files included in the paths specified in the `<BoAT-ProjectTemplate>/BoAT-SupportLayer/include/BoAT-SupportLayer.conf` file.
- All header files included in the paths specified in the `<BoAT-ProjectTemplate>/BoAT-Engine/include/BoAT-Engine.conf` file.
- All library files in the `<BoAT-ProjectTemplate>/lib` directory.
- If the C interface code for the smart contract is generated based on the contract ABI JSON file, the generated smart contract C interface code file should also be included.

1. Include SDK headers in the application

- Add all the header file paths specified in the BoAT-SupportLayer.conf and BoAT-Engine.conf files to the header file search paths of the application, or copy all the header files in these paths to the application's header file directory.
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


- In the application's related C code, add the following header files:
  ```
  #include "boatiotsdk.h" //SDK entry header file
  #include "boatconfig.h" //SDK configuration header file
  #include "boatEngine.h" //SDK blockchain application header file
  #include "boatosal.h"   //SDK OSAL platform abstraction interface header file
  #include "boatdal.h"    //SDK DAL driver abstraction interface header file
  ```
  If the C interface code for the smart contract is generated based on the contract ABI JSON file, the header file for the generated smart contract C interface code should also be included, and the generated `*.c` file should be added to the application's compilation script.

2. Link SDK library files in the application

- In the application's library linking, add the two static libraries in `<BoAT-ProjectTemplate>/lib` in order:  
  `libboatengine.a`
  `libboatvendor.a`

- In the application's library linking, add the dynamic library for curl:  
  `-lcurl`

For cross-compilation, ensure that the version of curl in the development environment is consistent with the one in the target board's runtime environment.

### SDK Initialization and Destruction
Before calling the SDK, you must call BoatIotSdkInit() to initialize the global resources of the SDK:
   ```
   BOAT_RESULT BoatIotSdkInit(void);
   ```
After usage, call BoatIotSdkDeInit() to release the resources:
   ```
   void BoatIotSdkDeInit(void);
   ```
### Key Pair Creation/Retrieval/Deletion

The SDK supports two types of key pairs: one-time key pairs and persistent key pairs. One-time key pairs are created temporarily and only exist in memory, becoming invalid after shutdown. Persistent key pairs are persistently saved when created, and can be loaded after shutdown and reboot to reuse the previously created persistent key pairs.  
Key pairs are used to generate blockchain wallet addresses. The SDK supports creating 6 key pairs on the same IoT device, including 1 one-time key pair and 5 persistent key pairs. After the key pairs are created, they are stored in the system, and each key pair corresponds to a storage index. The IoT device can retrieve the key pairs stored in the system based on the index number and apply different key pairs to different scenarios according to the application's needs.  
In addition to being used in secure applications, key pairs are also a component of blockchain wallets.  
***Note: The persistent implementation method in `<BoAT-ProjectTemplate>/BoAT-SupportLayer/common/storage` is for reference only. In commercial products, it is recommended to consider more secure persistent methods based on the actual hardware capabilities.***



#### Key Pair Creation
IoT devices generate key pairs through the key pair creation function. The creation of key pairs requires pre-configuring the properties of the key pair, including the mode, type, algorithm, format, etc. The creation process generates a private key and a public key for the key pair, and stores the key pair in the system's encrypted file. After successful creation, the index number of the key pair in the system is obtained for loading and deleting the key pair.  
The key pair creation function is as follows:
```
BOAT_RESULT BoatKeypairCreate(BoatKeypairPriKeyCtx_config *keypairConfig, 
                              BCHAR *keypairName, 
                              BoatStoreType storeType)
```
**Parameters:**

|Parameter Name        |Description                                                        |
|:---------------------|:------------------------------------------------------------------|
|keypairConfig         |A pointer to a keypair configuration structure. See boatkeypair.h for more details.  |
|keypairName           |A string of keypair name.<br>If the given \<keypairName\> is NULL, the SDK will generate an internal name for the current keypair.|
|storeType             |An one-time or persistent keypair type|


**Return Value:**  
```
This function returns the non-negative index of the loaded keypair.
It returns -1 if keypair creation fails.
```
Example (Creating a one-time key pair):
```
    BUINT8 keypairIndex = 255;
    BoatKeypairPriKeyCtx_config keypair_config = {0};

	/* keypair_config value assignment */
    keypair_config.prikey_genMode = BOAT_KEYPAIR_PRIKEY_GENMODE_INTERNAL_GENERATION;
    keypair_config.prikey_type    = BOAT_KEYPAIR_PRIKEY_TYPE_SECP256K1;


	/* create ethereum keypair */
    keypairIndex = BoatKeypairCreate( &keypair_config, keypairName,BOAT_STORE_TYPE_RAM);

```

#### Get Key Pair
After creating a key pair, the IoT application can use the returned key pair index number to obtain the detailed content of the key pair, including the index number, name, format, type, and public key of the key pair.
The IoT device application can load the key pair and get its basic information using the BoATKeypair_GetKeypairByIndex function, which is defined as follows:


```
BOAT_RESULT BoATKeypair_GetKeypairByIndex(BoatKeypairPriKeyCtx *priKeyCtx, 
                                          BUINT8 index)
```

**Parameters:**

|Parameter Name        |Description                                                        |
|:---------------------|:------------------------------------------------------------------|
|priKeyCtx             |A BoatKeypairPriKeyCtx structure pointer used to store key pair information read from the system.|
|index                 |The keypair index want to read.|

**Return Value:**  
```
This function returns 0 if the keypair was successfully obtained.
It returns -1 if obtaining the key pair fails.
```

#### Delete Key Pair
The IoT device application can delete the loadable key pair corresponding to the index using the BoATIotKeypairDelete function.
```
BOAT_RESULT BoATIotKeypairDelete(BUINT8 index)
```
**Parameters:**

|Parameter Name        |Description                                                        |
|:---------------------|:------------------------------------------------------------------|
|index                 |The index of the loadable keypair.|

**Return Value:**  
```
This function returns 0 if kepair deletion is successful.
It returns -1 if keypair deletion fails.
```

### Blockchain Network Creation/Get/Delete
Different blockchains have different network structures and application methods. Different blockchains create their network application information according to their network characteristics and store it in the system. The blockchain network is a component of the blockchain wallet.
#### Blockchain Network Creation
When creating a blockchain network, the network information required for the blockchain application is stored in the system, and the index number returned is used by the IoT application to obtain the network information. The storage type of the network information is divided into two categories: one-time network and persistent network.
The creation of the network function is implemented separately for different blockchains. Taking Ethereum blockchain as an example, the creation function for Ethereum blockchain network is as follows:
```
BOAT_RESULT BoATEthNetworkCreate(BoatEthNetworkConfig *networkConfig, 
                                 BoatStoreType storeType)
```
**Parameters:**
|Parameter Name        |Description                                                        |
|:---------------------|:------------------------------------------------------------------|
|networkConfig         |A pointer to a Etherum network configuration structure. See network_ethereum.h for more details.  |
|storeType             |An one-time or persistent keypair type|

**Return Value:**  
```
This function returns the non-negative index of the loaded Ethereum network.
It returns -1 if Ethereum network creation fails.
```
Example: (Creating a one-time network information)
```
    BUINT8 networkIndex = 255;
    BoatEthNetworkConfig network_config = {0};

    network_config.chain_id             = 1;
    network_config.eip155_compatibility = BOAT_FALSE;
    strncpy(network_config.node_url_str, demoUrl, BOAT_ETH_NODE_URL_MAX_LEN - 1);

	/* create ethereum network */
    networkIndex = BoATEthNetworkCreate( &network_config, BOAT_STORE_TYPE_RAM);
```

#### Get Network Information
The IoT application can obtain the blockchain network information by using the index number returned by the blockchain network creation function.
```
BOAT_RESULT BoATEth_GetNetworkByIndex(BoatEthNetworkData *networkData, 
                                      BUINT8 index)
```
**Parameters:**
|Parameter Name        |Description                                                        |
|:---------------------|:------------------------------------------------------------------|
|networkData           |A pointer to a BoatEthNetworkData structure used to store the network information.|
|index                 |The index of the network.|

**Return Value:**  
```
This function returns 0 if the network was successfully obtained.
It returns -1 if obtaining the network fails.
```

#### Delete Network Information
The IoT device application can delete the specified blockchain network information through the network information deletion interface provided by each blockchain.
The function for deleting Ethereum blockchain network information is as follows:
```
BOAT_RESULT BoATEthNetworkDelete(BUINT8 index)
```
**Parameters:**

|Parameter Name        |Description                                                        |
|:---------------------|:------------------------------------------------------------------|
|index                 |The index of the loadable Ethereum network.|

**Return Value:**  
```
This function returns 0 if network deletion is successful.
It returns -1 if network deletion fails.
```

### Blockchain Wallet Initialization/Deinitialization
A wallet is a collection of attributes of a blockchain account, including private keys, blockchain node URLs, and other key attributes. The wallet must be initialized before initiating transactions or invoking smart contracts.

#### Initialize Wallet
The SDK creates wallets for different blockchains according to their specific requirements.

Wallets are created based on the principle of on-demand creation. They obtain key pairs and blockchain network information based on the index number, and calculate the wallet address information based on the address algorithm required by the blockchain. The wallet address is stored in memory and serves as the account for accessing the blockchain.

When creating a wallet, make sure to call the wallet creation function `BoatXxxWalletInit()` in the same thread, where `Xxx` is the name of the blockchain, such as `BoatEthWalletInit()` for Ethereum.

When creating a wallet, specific wallet configuration parameters should be passed according to the specific blockchain protocol. For example, the function for creating an Ethereum wallet is described as follows:
```
BoatEthWallet *BoatEthWalletInit(BUINT8 walletIndex, 
                                 BUINT8 networkIndex)
```
Parameters:

|Parameter Name        |Description                                                        |
|:---------------------|:------------------------------------------------------------------|
|walletIndex           |The index of the loadable keypair.                                  |
|networkIndex          |The index of the loadable Ethereum network.                         |

**Return Value:**  
Returns 0 if the wallet initialization is successful.  
Returns -1 if the wallet initialization fails.  

#### Deinitialize Wallet
Each blockchain in the SDK has its own deinitialization function for the blockchain wallet. Deinitializing the wallet unloads the initialized wallet from memory. Unloading does not delete the persisted key pairs and blockchain networks, but the wallet cannot be used until it is initialized again.

The function for deinitializing an Ethereum wallet is as follows:
```
void BoatEthWalletDeInit(BoatEthWallet *wallet_ptr)
```
Parameters:

|Parameter Name        |Description                                                        |
|:---------------------|:------------------------------------------------------------------|
|wallet_ptr            |A pointer to the initialized Ethereum blockchain wallet structure variable.|

**Return Value:**  
Returns 0 if the wallet deinitialization is successful.  
Returns -1 if the wallet deinitialization fails.  

### Transfer Invocation
Transfer tokens from the current account to another account (not all blockchain protocols support transfers).

Using Ethereum as an example:
```
BOAT_RESULT BoatEthTransfer(BoatEthTx *tx_ptr,
                            BCHAR *value_hex_str);
```
Parameters:

|Parameter Name        |Description                                                        |
|:---------------------|:------------------------------------------------------------------|
|tx_ptr                |Transaction pointer.                                               |
|value_hex_str         |A string representing the value (Unit: wei) to transfer, in HEX format like "0x89AB3C".<br>Note that decimal value is not accepted. If a decimal value such as "1234" is specified, it's treated as "0x1234".|

**Return Value:**  
This function returns BOAT_SUCCESS if transfer is successful.
Otherwise it returns one of the error codes.

### Contract Call Automatically Generated

#### Restrictions on Automatically Generated Contracts
Since contract programming languages generally support object-oriented, and C language does not support object-oriented, and cannot use a unified paradigm to transfer objects, only contract functions whose parameter types are consistent with the built-in types of C language can be converted into C calling code by tools.

For contracts written by Solidity, the tool supports the following parameter types:
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
  - T[N] : N > 0, N is an integer, T is Any of the above types.
  - T[]  : T is Any of the above types except T[N].

For WASM contracts written in C++, the tool supports the following parameter types:
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

If the contract function contains unsupported parameter types, the automatic conversion tool will not be able to complete the conversion, and the contract must be written manually.

#### Automatically Generated Contract Call Interface
The successfully generated contract call interface is the following C function:

`BCHAR *<Contract ABI JSON file name >_< Contract function name>(<Wallet type> *tx_ptr, …);`

E.g. :

`BCHAR *StoreRead_saveList(BoatEthTx *tx_ptr, Bbytes32 newEvent);`

The first parameter of the calling interface is always the pointer of the initialized transaction object.

If the call is successful, the return value of the call interface is a string in JSON format.

*Note: The memory of the string is allocated inside the SDK. The application must copy the string to the memory allocated by the application for subsequent use, and must not modify the content of the returned string or save the pointer to the string for use.*

The contract function that changes the state of the blockchain refers to any function that changes the persistent storage information in the blockchain ledger. For example, functions that write and modify member variables in the contract are all contract functions that change the state of the blockchain.If a contract function only modifies the local variables in the function, but does not modify the contract member variables, then the contract function is a contract function that does not change the state of the blockchain.

In some contract programming languages, the two types of contract functions have a clear modifier difference on the prototype, while in other programming languages, there is no obvious mark. In the ABI, these two types of functions have different identifiers.

For example, take the following Ethereum Solidity contract as an example:

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


After the contract is compiled, the ABI corresponding to the three functions are described as follows:

````
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
````

In the above contract, eventList is a member variable of the contract. saveList() will change the value of eventList, which is a contract function that changes the state of the blockchain; readListLength() and readListByIndex() fuction have a view modifier and only reads the value of eventList. It is a contract function that does not change the state of the blockchain.  

In particular, there are significant differences in the principles of invoking between smart contract functions that update the ledger and those who don’t, even though the two types of functions are quite similar in prototype. The same is true for the interface functions in C language generated through the conversion tool.

Any change to the state of the blockchain needs to be implemented through blockchain transaction and reach a consensus across the entire network. The contract function that changes the state of the blockchain is called asynchronously. When called, it merely initiates the blockchain transaction, and the contract is not executed until the blockchain network packages the transaction into a block. Take StoreRead demo as an example. The related calling code is as follows:  
```
  BCHAR *result_str;
  result_str = StoreRead_saveList(&tx_ctx, (BUINT8*)"HelloWorld");
```
The return value result_str obtained from the StoreRead_saveList function is only the hash value used to identify the transaction, not the return value of the StoreRead contract saveList function. Therefore, when designing a smart contract, the public interface function that changes the state of the blockchain, the information it tries to return should be stored in the contract member variable. To get the value of the changed blockchain, refer to the following code, where the "hash" is the hash of the transaction obtained in the previous step.  
```
  BUINT32 list_len;
  if (BOAT_SUCCESS == BoatEthGetTransactionReceipt(hash))
  {
    list_len = StoreRead_readListLength(&tx_ctx);
    result_str = StoreRead_readListByIndex(&tx_ctx, list_len - 1);
  }
```
When BoatEthGetTransactionReceipt (hash) return value is BOAT_SUCCESS, shows the state of the corresponding change block chain contract function call has been mined. Assume that the contract will only be changed by the user, and then get the length list_len of the eventList array from StoreRead_readListLength().The value "HelloWorld" is uploaded by StoreRead_saveList() through a call to StoreRead_readListByIndex().  

Contract functions that do not change the state of the blockchain, only need the blockchain node to read the existing information in its database, without transactions and consensus. So the call to this type of function is a synchronous call. The return value is the return value in the form of the contract function. The readListLength and readListByIndex functions of the StoreRead contract are such contract functions.

*Note: This code can only be pseudocode, in order to facilitate understanding, the return value needs to be converted accordingly. For details, see Demo_ethereum_Storeread.c.*

#### Transaction Initialization
To call the automatically generated contract interface, first initialize a transaction object, and then call the generated contract interface.  
Even if the called contract function does not change the state of the blockchain, a transaction object needs to be initialized first.

Take Ethereum as an example (different blockchain protocol parameters are different):

````
BOAT_RESULT BoatEthTxInit(BoatEthWallet *wallet_ptr,
                          BoatEthTx *tx_ptr,
                          BBOOL is_sync_tx,
                          BCHAR *gasprice_str,
                          BCHAR *gaslimit_str,
                          BCHAR *recipient_str)
````

Parameters:

|parameter name  |Parameter Description                                        |
|:-------------- |:----------------------------------------------------------- |
|wallet_ptr      |The wallet pointer that this transaction is combined with.   |
|tx_ptr          |Pointer a transaction object.                                |
|is_sync_tx      |For a stateful transaction, specify BOAT_TRUE to wait until the transaction is mined.<br>Specifiy BOAT_FALSE to allow multiple transactions to be sent continuously in a short time.<br>For a state-less contract call, this option is ignored. |
|gasprice        |A HEX string representing the gas price (unit: wei) to be used in the transaction.<br>Set \<gasprice\> = NULL to obtain gas price from network.<br>BoatEthTxSetGasPrice() can later be called to modify the gas price at any time before the transaction is executed. |
|gaslimit        |A HEX string representing the gas limit to be used in the transaction.<br>BoatEthTxSetGasLimit() can later be called to modify the gas limit at any time before the transaction is executed. |
|recipient_str   |A HEX string representing the recipient address, in HEX format like"0x19c91A4649654265823512a457D2c16981bB64F5".<br>BoatEthTxSetRecipient() can later be called to modify the recipient at any time before the transaction is executed. |

**return value:**
This function returns `BOAT_SUCCESS` if initialization is successful.
Otherwise it returns one of the error codes.

E.g. :

````
BoatEthTx tx_ctx;
BOAT_RESULT result;
result = BoatEthTxInit(wallet_ptr,
                       &tx_ctx,
                       BOAT_TRUE,
                       NULL,
                       "0x333333",
                       "0x9e7f3ae22cf97939a2e4cd68dd33bb29268a1ec9");
````

#### Call Contract Interface
After completing the transaction initialization, you can call the automatically generated contract interface:

E.g. :

```
BCHAR *result_str;
result_str = StoreRead_saveList(&tx_ctx, (BUINT8 *)"HelloWorld");
```
#### Frequently Asked Questions
**Q:The following error is reported when compiling**
```
    for abi_item in self.abi_object['abi']:
TypeError: list indices must be integers, not str
```
A:The problem is caused by using the wrong input file. This input file should be the complete JSON file which is generated by the compiler. If just the contents of the ABI is copied and a JSON file is created, the ABI is need to add on the outermost layer. For specific format, please refer to:  
https://github.com/aitos-io/BoAT-X-Framework/issues/355


### Manually Construct Contract Calls
If the automatic generation tool cannot generate the C call interface, you need to manually construct the transaction message. In addition, because the Fabric and hwbcs invocation itself is so convenient that there is no need to use automatically generate interface tools, all contracts need to be invoked manually.

The manual construction of transactions needs to follow the ABI interface of the specific blockchain protocol.

**Example 1: Ethereum transaction structure**
- **Step 1** Call BoatEthTxInit() to initialize the transaction
- **Step 2** Set transaction parameters
  -Set transaction parameter nonce:

  ```
  BOAT_RESULT BoatEthTxSetNonce(BoatEthTx *tx_ptr, BUINT64 nonce);
  ```

  Nonce is usually set to BOAT_ETH_NONCE_AUTO, and the nonce value is obtained from the network.

  -If necessary, set the "value" parameter of the transaction(in the initialized transaction object, value defaults to 0):

  ```
  BOAT_RESULT BoatEthTxSetValue(BoatEthTx *tx_ptr, BoatFieldMax32B *value_ptr);
  ```
- **Step 3** For contract calls (transactions) that change the state of the blockchain, set the "data" parameter of the transaction:
  ```
  BOAT_RESULT BoatEthTxSetData(BoatEthTx *tx_ptr, BoatFieldVariable *data_ptr);
  ```
  Among them, the format of data_ptr follows the Ethereum ABI, including the first 4 bytes of the Keccak-256 hash of the contract function prototype as the Function Selector, and then the parameters are arranged in sequence:
  <https://solidity.readthedocs.io/en/develop/abi-spec.html>

- **Step 4** Send transaction
  - For contract calls that change the state of the blockchain, the following contract functions are called:
  ```
  BOAT_RESULT BoatEthTxSend(BoatEthTx *tx_ptr);
  ```

  - For contract calls that do not change the state of the blockchain, call the State-less contract function
  ```
  BCHAR *BoatEthCallContractFunc(BoatEthTx *tx_ptr,
                                  BCHAR *func_proto_str,
                                  BUINT8 *func_param_ptr,
                                  BUINT32 func_param_len);
  ```
  Among them, the format of func_param_ptr follows the same rules as Step 3.  

**Example 2: PlatONE transaction structure**
- **Step 1** Call BoatPlatONETxInit() to initialize the transaction, and the transaction type field is set according to the actual transaction type.

- **Step 2** Set transaction parameter:
  ```
  BOAT_RESULT BoatPlatoneTxSetNonce(BoatEthTx *tx_ptr, BUINT64 nonce); 
  ```
  The nonce is usually set to BOAT_PLATONE_NONCE_AUTO, and the nonce value is obtained from the network.

  - If necessary, set the "value" parameter of the transaction (in the initialized transaction object, value defaults to 0):
  ```
  BOAT_RESULT BoatPlatoneTxSetValue(BoatEthTx *tx_ptr, BoatFieldMax32B *value_ptr);
  ```
- **Step 3** For contract calls (transactions) that change the state of the blockchain, set the "data" parameter of the transaction:
  ```
  BOAT_RESULT BoatPlatoneTxSetData(BoatEthTx *tx_ptr, BoatFieldVariable *data_ptr);
  ```
  Among them, data_ptr is coded according to RLP and is sequentially compiled into the following fields:
  ```
  {
    TransactionType (Fixed unsigned 64bit, BigEndian),
    FunctionName,
    FunctionArgument1,
    FunctionArgument2,
  …
  }
  ```
  RLP encoding is as follows:
  - a) Call RlpInitListObject() to initialize the top-level LIST object
  - b) Call RlpInitStringObject() to initialize the first encoding field object
  - c) Call RlpEncoderAppendObjectToList() to add the first encoding field object to the top-level LIST object
  - d) Repeat b and c until all code objects are added to the top LIST object
  - e) Call RlpEncode() to perform RLP encoding on the top-level LIST object and its subobjects
  - f) Call RlpGetEncodedStream() to get the encoded stream
  - g) After completing the contract call, call RlpRecursiveDeleteObject() to destroy the top-level LIST object and all its child objects

- **Step 4** Send transaction
  - For contract calls that change the state of the blockchain, the following contract functions are called:
  ```
  BOAT_RESULT BoatPlatoneTxSend(BoatEthTx *tx_ptr);
  ```

  - For contract calls that do not change the state of the blockchain, call the State-less contract function
  ```
  BCHAR *BoatPlatoneCallContractFunc(BoatPlatoneTx *tx_ptr,
                                      BUINT8 *rlp_param_ptr,
                                      BUINT32 rlp_param_len)
  ```
  Among them, the format of rlp_param_ptr follows the same rules as Step 3.  

  - For the specific calling method, please refer to the automatically generated code of the Demo attached to the SDK, which is located under `\<SDKRoot\>/demo/demo_platone/demo_contract/WASM` or `<SDKRoot\>/demo/demo_platone/demo_contract/Solidity`.

**Example 3: Hyperledger Fabric transaction structure**  
- **Step 1** Call BoatHlfabricTxInit() to initialize the transaction, The parameters are set based on actual usage.  

- **Step 2** If the node discovery function is not turned on,call BoatHlfabricWalletSetNetworkInfo() to set the newwork parameters. 
  
- **Step 3** Call BoatHlfabricTxSetTimestamp() to set timestamp, The real-time is obtained based on hardware functions.

- **Step 4** If the node discovery function is turned on,call BoatHlfabricDiscoverySubmit() to get all the nodes information on current chain. 

- **Step 5** Set trasaction parameters.  
  Examples of using demo_fabric_abac.c code:  
  ```
  result = BoatHlfabricTxSetArgs(&tx_ptr, "invoke", "a", "b", "10", NULL);
  ```
  All function call of Fabric's input data are string. In the above code, "invoke" is the function name in the ABAC chain code. "a", "b", and "10" are the corresponding three inputs to the function. Regardless of the type of the corresponding variable in the chain code, the shape of string is used as the input.This is why there is no need to use automatically generate the contract interface tool.  

- **Step 6** Send the transaction.  
  - For contract calls that change the state of the blockchain, call the BoatHlfabricTxSubmit function：
    ```
    BOAT_RESULT BoatHlfabricTxSubmit(BoatHlfabricTx *tx_ptr);
    ```

  - For contract calls that do not change the state of the blockchain, call the BoatHlfabricTxEvaluate contract function：
    ```
    BOAT_RESULT BoatHlfabricTxEvaluate(BoatHlfabricTx *tx_ptr);
    ```
  When the return result is BOAT_SUCCESS, the call succeeds。

**Example 4: HW BCS transaction structure**  

- **Step 1** Call BoatHwbcsTxInit() to initialize the transaction, The parameters are set based on actual usage.  

- **Step 2** Call BoatHwbcsWalletSetNetworkInfo() to set the newwork parameters. 
  
- **Step 3** Call BoatHwbcsTxSetTimestamp() to set timestamp, The real-time is obtained based on hardware functions.

- **Step 4** Set trasaction parameters.  
  Examples of using demo_hw_bcs.c code:  
  
  ```
  result = BoatHwbcsTxSetArgs(&tx_ptr, "initMarble", "a","1" , NULL, NULL);
  ```
All function call of Hwbcs's input data are string. In the above code, "initMarble" is the function name in the hw chain code. "a", "1" are the corresponding two inputs to the function. Regardless of the type of the corresponding variable in the chain code, the shape of string is used as the input.This is why there is no need to use automatically generate the contract interface tool.  
  
- **Step 5** Send the transaction.  
  
  - For contract calls that change the state of the blockchain, call the BoatHwbcsTxSubmit function：
    ```
    BOAT_RESULT BoatHwbcsTxSubmit(BoatHwbcsTx *tx_ptr)
  ```
  
  - For contract calls that do not change the state of the blockchain, call the BoatHwbcsTxEvaluate contract function：
    ```
    BOAT_RESULT BoatHwbcsTxEvaluate(BoatHwbcsTx *tx_ptr);
    ```
  When the return result is BOAT_SUCCESS, the call succeeds。

**Example 5: CHAINMAKER  transaction structure**

- **Step 1** Call BoatHlChainmakerTxInit() to initialize the transaction, The parameters are set based on actual usage.  

- **Step 2** Call BoatHlchainmakerAddTxParam() to set transaction parameters.

  Examples of using code:  

  ```
   BoatHlchainmakerAddTxParam(&tx_ptr, 6, "time", "6543235", "file_hash", "ab3456df5799b87c77e7f85", "file_name", "name005", NULL);
  ```

- **Step 3** Call BoatHlchainmakerContractInvoke() invoke transaction.

- **Step 4** Call BoatHlchainmakerContractQuery() query transaction.



## Suggestions for Porting SDK to RTOS

If the SDK is ported to RTOS, the following points should generally be followed:
###### 1. Remove The Dependency On Curl

curl is a communication protocol library under Linux, used in the SDK to support http/https communication. Blockchain nodes usually use the http/https protocol to communicate.

For modules using RTOS, you should add a call package to the module's http/https interface in\<SDKRoot\>/vendor/platform/\<platform_name\>/src/rpc, and modify \<SDKRoot\>/vendor/platform/\<platform_name\>/scripts/gen.py, close RPC_USE_LIBCURL and set the new RPC USE OPTION.


###### 2. Remove Dependence on The File System

The SDK uses files as a persistent storage method for the wallet. If the RTOS does not support the file system, you should modify the file operation related to  `BoatGetFileSize` , `BoatWriteFile` , `BoatReadFile` , `BoatRemoveFile` ,from \<SDKRoot\>/vendor/platform/\<platform_name\>/port_xx/boatplatform_internal.c, modify the read/write file to the persistence method supported by the system.

###### 3. Memory Trimming

If the memory of the target system is too tight to load, you can try to trim the memory. The points that can be cropped include:

a) According to actual needs, in \<SDKRoot\>/makefile, close the blockchain protocol that does not need to be supported  
b) According to the actual situation, reduce the storage space BOAT_XXX_NODE_URL_MAX_LEN of the node URL string in \<SDKRoot\>/api_\<protocol\>.h  
c) According to actual needs, reduce the number of wallets BOAT_MAX_WALLET_NUM in \<SDKRoot\>/include/boatwallet.h  
d) According to the actual situation, reduce the maximum number of members supported in a LIST in \<SDKRoot\>/include/boatrlp.h MAX_RLP_LIST_DESC_NUM  
e) According to the actual situation, reduce the self-increment step size of web3 data buffer in <SDKRoot>/sdk/protocol/common/web3intf/web3intf.h WEB3_STRING_BUF_STEP_SIZE


###### If After The Above Cropping, The Memory is Still Too Large To Fit, Please Try:
- a) According to actual needs, the simple RLP encoding method for specific transaction parameters is used to replace the recursive general RLP encoding method in \<SDKRoot\>/sdk/rlp
- b) According to actual needs, tailor the APIs that are not actually used

## BoAT's Extended AT Command Suggestion

### Create Keypair AT^BCKEYPAIR
|Command                                                                         |Response(s)                                              |
|:-----------------------------------------------------------------------------  |:------------------------------------------------------- |
|Write Command:<br>^BCKEYPAIR=\<store_type\>,\<keypair_name\>[,\<kepair_config\>] |^BCKEYPAIR: \<wallet_index\><br>OK<br>                      |
|Test Command:<br>^BCKEYPAIR=?                                                      |+BCKEYPAIR: (list of supported \<protocol_type\>s)<br>OK<br>|

Function:
Create a keypair, corresponding to BoatKeypairCreate().

Parameters:
\<store_type\>: integer type
- 1: persistent
- 0: one-time

\<keypair_name\>: string type; 
a string of keypair name

\<keypair_config\>: string type;
A JSON string representing the keypair configuration of \<protocol_type\>. The exact format is manufacture specific.

\<keypair_index\>: integer type; 
the index of the created keypair

### Create Blockchain Network AT^BCNETWORK
|Command                                                                         |Response(s)                                              |
|:-----------------------------------------------------------------------------  |:------------------------------------------------------- |
|Write Command:<br>^BCNETWORK=\<protocol_type\>,\store_type\>[,\<network_config\>] |^BCNETWORK: \<wallet_index\><br>OK<br>                      |
|Test Command:<br>^BCNETWORK=?                                                      |+BCNETWORK: (list of supported \<protocol_type\>s)<br>OK<br>|

Function:
Create a blockchain network, corresponding to BoatXxxNetworkCreate().

Parameters:
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


### Initialize Wallet AT^BCWALT
|Command                                                                         |Response(s)                                              |
|:-----------------------------------------------------------------------------  |:------------------------------------------------------- |
|Write Command:<br>^BCWALT=\<protocol_type\>,\<keypair_index\>[,\<network_index\>] |^BCWALT: <br>OK<br>                      |
|Test Command:<br>^BCWALT=?                                                      |+BCWALT: (list of supported \<protocol_type\>s)<br>OK<br>|

Function:
Initialize a wallet, corresponding to BoatXxxWalletInit().

Parameters:
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


### Uninitialize Wallet AT^BUWALT
|Command**                               |Response(s)                                            |
|:-------------------------------------- |:----------------------------------------------------- |
|Write Command:<br>^BUWALT=<wallet_tpye> |<br>OK<br>                                             |
|Test Command:<br>^BUWALT=?              |+BUWALT: (list of loadeded \<wallet_index\>s)<br>OK<br>|

Function:
Uninitialize a wallet, corresponding to BoatWalletUnload().

Parameters:
\<wallet_tpye\>: integer type; wallet type to unload, previously protocol_type parameter entered by AT^BCNETWORK earlier

### Delete Keypair AT^BDKEYPAIR
|Command                                  |Response(s)   |
|:--------------------------------------- |:------------ |
|Write Command:<br>^BDKEYPAIR=\<keypair_index\>|<br>OK<br>    |

Function:
Delete a created persistent keypair, corresponding to BoatKeypairDelete().

Parameters:
\<keypair_index\>: integer type; the index of the stored keypair

### Delete Network AT^BDNETWORK
|Command                                  |Response(s)   |
|:--------------------------------------- |:------------ |
|Write Command:<br>^BDNETWORK=\<network_index\>|<br>OK<br>    |

Function:
Delete a created persistent blockchain network, corresponding to BoatXxxNetworkDelete().

Parameters:
\<network_index\>: integer type; the index of the stored network

### Call Contract Function AT^BCALLFUNC
|Command                                                    |Response(s)   |
|:--------------------------------------------------------- |:------------ |
|Write Command:<br>^BCALLFUNC=\<tx_object\>|<br>OK<br>    |

Function:
Initiate a contract function call, corresponding to the generated C contract interface based on the contract ABI JSON file.

Parameters:
\<tx_object\>: string type;
A JSON string representing the transaction object as per the generated C contract interface. The exact format is manufacturer-specific.


### Transfer AT^BTRANS
|Command                                                           |Response(s)   |
|:---------------------------------------------------------------- |:------------ |
|Write Command:<br>^BTRANS=\<recipient\>,\<value\>|<br>OK<br>    |

Function:  
Initiate a transfer from the specified wallet (not all blockchains support transfers). For example, for Ethereum, it corresponds to BoatEthTransfer().

Parameters:  
\<recipient\>: string type;  
A HEX string representing the recipient (payee) address.

\<value\>: integer type; the value to transfer, represented by the smallest token unit of the protocol (e.g. for Ethereum, the unit is wei or 10-18 ether).

## References
[1].BoAT IoT Framework SDK Reference Manual, AITOS.20.70.100IF, V1.0.0

[1].BoAT IoT Framework SDK Reference Manual, AITOS.20.70.100IF, V1.0.0
