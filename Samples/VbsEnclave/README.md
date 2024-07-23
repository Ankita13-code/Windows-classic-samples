---
page_type: sample
languages:
- cpp
products:
- windows-api-win32
name: Virtualization-based security (VBS) enclave sample
urlFragment: VbsEnclave
description: "Shows how to create and run code in a Virtualization-based security (VBS) enclave."
extendedZipContent:
- path: LICENSE
  target: LICENSE
---

Virtualization-based security (VBS) Enclave Sample Code
=======================

This sample demonstrates the lifecycle of a VBS enclave including making function calls into the enclave. It also serves as a starting point for new enclave projects since it has all the MSVC configuration for enclaves inside the `.vcxproj` files.

Requirements
------------

- Windows 11
- Visual Studio 2022 version 17.9 or above, with Build Tools installed. It's recommended to install the Desktop development with C++ workload through the Visual Studio installer. It will install all the necessary tools including the Windows Software Development Kit (SDK).
- [Windows SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/) version 10.0.22621.3233 or above 
- [Windows Driver Kit](https://learn.microsoft.com/en-us/windows-hardware/drivers/download-the-wdk)


Build the sample
----------------
1.  Create a Hyper-V VM with Windows 11 installed. Make sure to turn off Secure Boot mode for the created VM.

2.  Now open a command prompt in Admin mode and run the following command `Bcdedit.exe -set TESTSIGNING ON`. Reboot the VM after this step. Windows VM displays a watermark with the text "Test Mode" in the lower right corner of the desktop to remind users that the system has test-signing enabled.

3.  Start Visual Studio and select **File** \> **Open** \> **Project/Solution**.

4.  Go to the `Samples/VbsEnclave` directory, and double-click the Microsoft Visual Studio Solution (.sln) file titled `Enclave Sample.sln`.

5.  Press F7 or use **Build** \> **Build Solution** to build the sample.

6.  After the build is successful, run `cd x64\Debug` in an elevated Powershell. You will find the `vbsenclave.dll` here which needs to be signed for this sample to run.

7.  Create a certificate for signing the vbsenclave.dll using `New-SelfSignedCertificate -CertStoreLocation Cert:\\CurrentUser\\My -DnsName "MyTestEnclaveCert" -KeyUsage DigitalSignature -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256 -TextExtension "2.5.29.37={text}1.3.6.1.5.5.7.3.3,1.3.6.1.4.1.311.76.57.1.15,1.3.6.1.4.1.311.97.814040577.346743379.4783502.105532346"`. You can find more about the certificate name format [here](https://learn.microsoft.com/en-us/windows/win32/trusted-execution/vbs-enclaves-dev-guide).

8.  For signing the dll run `signtool sign /ph /fd SHA256 /n "MyTestEnclaveCert" vbsenclave.dll`.

9.  Once this is done, navigate back to the visual studio and press `Ctrl + F5` to run the project. 

*Note*: The enclave won't run until it is signed. Please refer the VBS Enclaves Development Guide for the details.

VBS Enclaves Development Guide
------------------------------

The development guide provides insights on writing, compiling and debugging enclaves using this sample code as an example.

It is available at: [aka.ms/VBSEnclavesDevGuide](https://aka.ms/VBSEnclavesDevGuide)
