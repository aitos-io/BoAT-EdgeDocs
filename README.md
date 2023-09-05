
# BoAT-Edge Framework
[![Issue](https://img.shields.io/github/issues/aitos-io/BoAT-X-Framework)](https://github.com/aitos-io/BoAT-X-Framework/issues)![Forks](https://img.shields.io/github/forks/aitos-io/BoAT-X-Framework)![Stars](https://img.shields.io/github/stars/phengao/hello-world)[![GitHub Release](https://img.shields.io/github/license/aitos-io/BoAT-X-Framework)](https://github.com/aitos-io/BoAT-X-Framework/blob/master/LICENSE)[![Join the chat at https://gitter.im/BoAT-X/community](https://badges.gitter.im/BoAT-X/community.svg)](https://gitter.im/BoAT-X/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

![BoAT logo](https://aitos-io.github.io/BoAT-X-Framework/logo/BoAT_RGB_Horizontal_100.png)


## Introduction to BoAT-Edge
**BoAT-Edge** is a set of blockchain edge-side application products for the Internet of Things (IoT) developed by Molian Technology. It helps IoT devices achieve trusted data on-chain, empowering data to be trusted throughout its lifecycle from the data source to the cloud and subsequent open exchange of IoT data. It is an important component of the BoAT family's key components. BoAT stands for Blockchain of AI Things, and "Edge" specifically refers to the various related components of blockchain edge-side applications covered by this product in the BoAT product family, including the foundational library for embedded blockchain client applications, specific functional libraries, and supporting tools. IoT devices can access various blockchain services by flexibly invoking this set of components.

- **BoAT-Infra-Arch**
  The multi-chain coexistence and multi-platform support provided by BoAT-Edge is based on the BoAT-Infra-Arch infrastructure. BoAT-Infra-Arch is a development framework designed by Molian for blockchain edge-side applications, suitable for developing various types of blockchain clients, also known as IoT device wallets.
  It is a lightweight blockchain client SDK for IoT devices written in C language, consisting of several foundational libraries with specific functionalities such as BoAT-ProjectTemplate, BoAT-SupportLayer, BoAT-Engine, etc. These libraries can be flexibly combined to build clients that meet different requirements. Clients can run independently or work in conjunction with BoAT-Anchor.

### Why BoAT-Edge Empowers the Creation of a Trusted Digital Foundation for IoT

The Internet of Things (IoT) has become a digital infrastructure for our daily lives and production. Massive IoT devices collect various types of data through sensors and transmit the collected data to cloud servers to execute relevant instructions, such as the smart locks on shared bicycles. Currently, major industries worldwide have reached a consensus that only real and trusted data is valuable.

More precisely, the valuable aspect lies in the information extracted from the data. If we look at a typical commercial use case of a data producer (the person collecting the data is not always responsible for analyzing the data and extracting information), the data producer must prove that the data is captured from actual IoT devices, rather than being generated or forged in a computer, when sharing the data with users. Therefore, the main purpose of BoAT-Edge is to enable IoT devices to access blockchain services. BoAT-Edge empowers IoT devices to achieve on-chain data at the network edge, as close as possible to the source of IoT data, extending the value of data through blockchain-based trusted proofs.

Blockchain is a distributed and tamper-proof ledger that helps achieve trusted data sharing among parties that lack mutual trust. Additionally, once data is stored on the blockchain, it is resistant to tampering throughout its lifecycle. However, there is still a cross-domain obstacle to overcome, which is how to ensure that the data stored on the blockchain is consistent with the data captured by IoT devices.

When IoT devices capture data and transmit it to the database within the IoT cloud platform, there are three possible anchor points to generate blockchain-based trusted data proofs: the cloud side, the edge side, or the device itself.

- If the data from the IoT cloud platform generates a **trusted data proof** through blockchain blocks, the trusted anchor point is the IoT platform. The IoT platform itself needs to have authority or a good reputation to have sufficient trustworthiness.
- If the IoT edge side aggregates IoT device data and generates **trusted data proofs**, while relaying them to the cloud platform and blockchain nodes, the trusted anchor point extends to the edge device or gateway.
- If the **trusted data proofs** are generated directly on the IoT devices, the trusted anchor point goes deep into the IoT devices themselves.

Compared to attacking or tampering with the cloud-side database, individually tampering with each on-site IoT device is much more difficult and costly. Therefore, the closer the anchor point is to the data source, the higher the trustworthiness of the proofs.

The BoAT-Edge product family provides a complete set of foundational components that leverage blockchain technology to transform IoT edge devices and establish trust anchor points at the nearest position to the data source on the IoT device side. BoAT-Edge ensures data trustworthiness from the data collection source, enhances the value of IoT data, and creates a trusted digital foundation for the Internet of Things.


### Unique Advantages of BoAT-Edge

Most blockchain or BaaS (Blockchain-as-a-Service) services provide different forms of nodes or client/wallet software. However, these nodes or software are designed for personal computers, cloud servers, or smartphones, typically written in high-level languages such as Go, Java, JavaScript, Python, etc. Some software requires a virtual machine or interpreter to execute, and some even need to dynamically download code at runtime. Meanwhile, IoT devices, due to differences in usage scenarios and various limiting factors, do not have large hardware resources and usually run on RTOS or lightweight Linux. Due to limited resources, most IoT devices can only support local C language applications, making it difficult to access blockchain services.

In the BoAT-Edge product, the BoAT Edge component is a lightweight C language multi-chain client that supports mainstream IoT wireless modules and chips. BoAT-Edge extends the service capabilities of blockchain from being only available for computers and smartphones to fragmented IoT devices.

### BoAT-Edge Working Principle

#### Operational Mode

![Direct Approach](https://aitos-io.github.io/BoAT-X-Framework/en-us/images/BoAT_README_Direct_Approach.png)

IoT devices can directly access blockchain nodes and network services. IoT devices send data to the IoT platform and send the hash value and signature of the data to the blockchain by calling the BoAT-Engine API. Data consumers then compare the on-chain hash value with the hash value generated by the data stored on the IoT platform to determine the trustworthiness of the data. Consumers can also check if the data comes from a registered IoT terminal.

### Code Download

#### BoAT Infra Arch

The **BoAT Infra Arch** development framework adopts a layered structure, consisting of multiple components that support specific functions. It includes the following components, each with detailed application instructions and examples.

- **BoAT-ProjectTemplate**: Used to build the compilation directory for applications based on the BoAT Infra Arch framework.

  Download link: [BoAT-ProjectTemplate](https://github.com/aitos-io/BoAT-ProjectTemplate)

- **BoAT-SupportLayer**: Provides a static library for the BIA system's abstract general API interface.

  Download link: [BoAT-SupportLayer](https://github.com/aitos-io/BoAT-SupportLayer)

- **BoAT-Engine**: Provides a static library for various blockchain application APIs.

  Download link: [BoAT-Engine](https://github.com/aitos-io/BoAT-Engine)


## Releases and Status

### Release Information
For a complete list of new features, please visit the [Release Information](https://github.com/phengao/hello-world/blob/master/BoATInfraArch_Releases.md).

### Project Status Updates
For project status updates, please visit the [Project Status Update](https://github.com/aitos-io/project-status-update).

### List of Supported BoAT Blockchain IoT Modules
For a list of supported BoAT Blockchain IoT modules, please refer to the [Supported List](./SUPPORTED_LIST.md).

## Documentation

### BoAT Blockchain IoT Module White Paper
Visit the [BoAT Blockchain IoT Module Product White Paper](https://aitos-io.github.io/BoAT-X-Framework/en-us/BoAT_Blockchain_IoT_Module_Product_White_Paper_en.pdf).

### BoAT Blockchain IoT Module Technology and Application
Visit the [BoAT Blockchain IoT Module Technology and Application](https://aitos-io.github.io/BoAT-X-Framework/en-us/BoAT_Blockchain_IoT_Module_Technology_and_Application_en.pdf).

### Documentation Index
For the complete documentation index, please visit the [BoAT Documentation](https://aitos-io.github.io/BoAT-X-Framework).

### Frequently Asked Questions
Coming soon.

## Community
BoAT-X community related information:
+ Contact Email: info@aitos.io
+ Bug Submission: [BoAT-X Issues](https://github.com/aitos-io/BoAT-X-Framework/issues)
+ News: https://aitos-io.medium.com/
+ LinkedIn: https://www.linkedin.com/company/aitos-io

## Community Contributions
Community contributions from core team members are welcome, including (but not limited to) style/defect fixes, feature implementations, suggestions for solutions/algorithms, and documentation updates.

For more information, please refer to the [Contribution Guideline](./CONTRIBUTING.md).

For development documentation, please refer to the [BoAT documentation](https://aitos-io.github.io/BoAT-X-Framework).

To submit Pull Requests, please visit [Pull Requests](https://github.com/aitos-io/BoAT-X-Framework/pulls).

## License

Apache License 2.0, please visit [LICENSE](./LICENSE).

## Sailors, welcome to embark on the fantastic journey of BoAT

**Open source by [aitos.io](http://www.aitos.io/)**

[![aitos.io](https://aitos-io.github.io/BoAT-X-Framework/logo/aitos_logo_100.png)](http://www.aitos.io/)
