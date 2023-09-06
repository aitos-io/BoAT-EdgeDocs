# BoAT Edge System Requirements

## Introduction

### Overview
This document describes the system requirements for the BoAT Edge product, which is a blockchain client SDK (C language version) based on the BoAT Infra Arch framework, designed for IoT devices with MCUs/cellular modules. BoAT Edge is an SDK that runs on the application processor of the module. For IoT devices with MCUs/OpenCPU cellular modules, BoAT Edge is linked and called as a library by the application program. For non-OpenCPU cellular modules, the BoAT API needs to be extended as AT commands for application calls on the host computer.

### Abbreviations and Terminology
|Term  |Explanation                                             |
|:---- |:------------------------------------------------------ |
|BoAT  |Blockchain of AI Things                                 |
|BIA   |BoAT Infra Arch                                         |
|BE    |BoAT-Engine                                             |
|BPT   |BoAT-ProjectTemplate                                    |
|BSL   |BoAT-SupportLayer                                       |
|SDK   |Software Development Kit                                |
|API   |Application Programming Interface                       |
|MCU   |Microcontroller Unit                                    |
|RTOS  |Real-Time Operating System                              |
|TRNG  |True Random Number Generator                            |
|CSPRNG|Cryptographically Secure Pseudo-Random Number Generator |
|RTC   |Real-Time Clock                                         |
|NTP   |Network Time Protocol                                   |
|HTTP  |Hyper Text Transfer Protocol                            |
|HTTPs |Hyper Text Transfer Protocol Secure                     |
|CoAP  |Constrained Application Protocol                        |
|MQTT  |Message Queuing Telemetry Transport                     |
|TCP   |Transmission Control Protocol                           |
|TEE   |Trusted Execution Environment                           |
|TA    |Trusted Application                                     |
|ECDSA |Elliptic Curve Digital Signature Algorithm              |
|SHA2  |Secure Hash Algorithm 2                                 |
|RAM   |Random Access Memory                                    |


## Storage Requirements

For the BoAT Infra Arch framework SDK (C language version), the storage requirements are as follows when supporting Ethereum/Venachain/FISCO BCOS:
- Flash (code and read-only data): approximately 210kB
- Flash (persistent read-write data): a few hundred bytes
- RAM (global variables, heap, stack): approximately 10kB

When supporting Hyperledger Fabric only, the storage requirements for the BoAT Infra Arch framework SDK (C language version) are as follows:
- Flash (code and read-only data): approximately 520kB
- Flash (persistent read-write data): a few kilobytes
- RAM (global variables, heap, stack): approximately 30kB

When supporting Chang'an Chain only, the storage requirements for the BoAT Infra Arch framework SDK (C language version) are as follows:
- Flash (code and read-only data): approximately 380kB
- Flash (persistent read-write data): a few kilobytes
- RAM (global variables, heap, stack): approximately 40kB

The above values do not include the system libraries that the BoAT Infra Arch framework SDK (C language version) depends on. The specific values may vary depending on the different blockchain protocols.

## Processing Power Requirements

When supporting Ethereum, the BoAT Infra Arch framework SDK (C language version) requires approximately 1 second (excluding network communication time) to perform cryptographic operations related to a blockchain transaction or smart contract invocation on an ARM Cortex M4 running at around 100MHz. The exact processing power requirements depend on the application that calls the BoAT Infra Arch framework SDK and its requirements for power consumption and latency. BoAT itself does not have specific requirements.


## TEE and Remote Attestation (Optional)

### TEE

If the application processor of the cellular module supports Trusted Execution Environment (TEE), TEE can be used to protect sensitive data and processes such as keys, signatures, and encryption. TEE should support at least the following capabilities:
1. Ability to develop Trusted Applications (TA) by the customer (such as opening necessary keys)
2. Support for Secure Boot, fuse, and other related features
3. Support for secure storage

### Remote Attestation

Remote Attestation is a mechanism that uses the Root of Trust embedded in the chip to provide signature services for device data and may collect device runtime environment feature information. Remote Attestation can help service providers remotely verify the authenticity of devices. If the chip in the module supports remote attestation, it should support at least the following capabilities:
1. Support for attesting and signing given data, with the ability to verify the signature on a remote server
2. If TEE is supported, it should support attesting and signing data within the TEE (optional)

## Cryptographic Operation Hardware Acceleration (Optional)

If the hardware supports cryptographic operation acceleration, it can improve the performance of cryptographic operations.

When supporting Ethereum/Venachain/FISCO BCOS, BoAT requires at least the following cryptographic operations:
1. Elliptic Curve Digital Signature Algorithm (ECDSA) with secp256k1 curve
2. Keccak-256 (variant of SHA3-256)

When supporting Hyperledger Fabric, BoAT requires at least the following cryptographic operations:
1. Elliptic Curve Digital Signature Algorithm (ECDSA) with secp256r1 curve
2. SHA2-256
