## Overview

File sharing powers collaboration, but without the right controls, it creates significant risk. Contracts, designs, source code, and financial reports can be leaked, copied, or misused. Traditional access controls stop at the perimeter and rarely follow the file. 

The **EaseFilter DRM Secure File Sharing** solution utilizes Digital Rights Management (DRM) and the powerful EaseFilter File Security SDK to embed policy enforcement directly into the file workflow. This ensures that your sensitive data remains protected across endpoints, servers, hybrid clouds, and external devices.

## Key Features

*   **Granular File Access Control:** Define exactly who can open, read, modify, or delete a file. Scope rules by user identity, process name, device posture, file path, file type, or network location.
*   **Transparent File Encryption (TFE):** Encrypt data at rest and decrypt on-the-fly only for authorized processes. Files remain encrypted even if copied outside the organization, preventing exfiltration.
*   **Usage Restrictions & View-Only:** Block printing, copy-pasting, screen capture, and bulk exporting. Set view-only permissions for external collaboration and enforce dynamic watermarks.
*   **Secure Collaboration & Rapid Revocation:** Safely share files with partners and clients while retaining full owner control. Revoke access instantly or set auto-expiry dates and view limits.
*   **Auditing, Visibility & Compliance:** Capture detailed, forensic-level logs of every open, read, write, rename, and delete event to support GDPR, HIPAA, SOX, and ISO 27001 compliance.
*   **Cross-Environment Protection:** Enforce consistent DRM policies across local storage, network shares, VDI, and hybrid cloud environments.

## Architecture

At the core of the EaseFilter solution is a robust, kernel-mode file system filter driver that intercepts Input/Output (I/O) requests at the lowest, most privileged level of the OS.
![File Monitor](https://www.easefilter.com/Images/SecureSharing.png)

1.  **The Kernel Mode Layer (The Enforcer):** Intercepts every file I/O request. Performs high-speed, on-the-fly AES encryption and decryption, making the process invisible to authorized end-users with minimal performance overhead.
2.  **The User Mode Layer (The Policy Engine):** Acts as the bridge between the kernel driver and central management. Evaluates intercepted requests against defined rules and records all allowed or denied actions for auditing.
3.  **The Central Management Server (The Brain):** Integrates with IAM providers (like Active Directory), stores security policies, and manages DRM encryption keys. Keys are delivered in real-time only upon access approval and are destroyed when the session ends.

## How It Works: The File Access Workflow

When a user attempts to open a protected file, the following sequence occurs instantly and silently:

1.  **Interception:** The user attempts to open a sensitive file (e.g., `Financial_Q3.xlsx`). The Filter Driver intercepts the read request in Kernel Mode.
2.  **Policy Check:** The driver pauses the request and queries the User Mode Policy Engine to check permissions.
3.  **Verification:** The Policy Engine consults the Central Management Server to verify user identity, device status, and the file's security policy.
4.  **Action & Decryption:** 
    *   *If Approved:* The Central Server releases the encryption key to the Kernel Module, which decrypts the file on the fly for the authorized application (e.g., Excel).
    *   *If Denied:* The request is immediately blocked, and the file remains encrypted.
5.  **Auditing:** The outcome (who, what, where, when, and result) is securely logged.

## Example Implementation (C#)

Below is a quick start example demonstrating how to set up an auto-encryption filter rule with DRM protection using the EaseFilter SDK.

```csharp
using System;
using EaseFilter.FilterControl;

class DRMExample 
{
    static void Main(string[] args) 
    {
		// Protect sensitive project documents with DRM
        FileFilter autoEncryptFilter = new FileFilter(@"C:\Projects\Confidential\*");
                
        // Enable auto-encryption for newly created files in this folder
        autoEncryptFilter.EnableEncryption = true; 
        
        // Retrieve encryption key and IV from the centralized service
        autoEncryptFilter.EnableEncryptionKeyFromService = true; 
        
        // Authorize encryption/decryption for every file via callback
        autoEncryptFilter.OnFilterRequestEncryptKey += OnFilterRequestEncryptKey; 
        
        // Set the 256-bit encryption key with a passphrase (Example)
        autoEncryptFilter.EncryptionKey = Utils.GetKeyByPassPhrase("passPhrase", 32); 
        
        // Prevent unauthorized processes/users from reading raw encrypted data
        autoEncryptFilter.EnableReadEncryptedData = false; 
        
        FilterControl.AddFilter(autoEncryptFilter);
        
        // Initialize and start the filter driver
        string lastError = "";
        FilterControl.StartFilter(filterType, serviceThreads, connectionTimeOut, licenseKey, ref lastError); 
        
        Console.WriteLine("DRM protection enabled. Press Enter to stop...");
        Console.ReadLine();
        
        FilterControl.StopFilter();
    }

    // Callback function to handle key requests from the service
    private static void OnFilterRequestEncryptKey(object sender, FilterRequestEventArgs e)
    {
        // Custom logic to fetch and authorize the decryption key from the Central Management Server
    }
}
```

## Business Benefits

*   **Stronger Security:** Files stay protected both inside and outside your corporate environment.
*   **Prevent Data Leaks:** Stop accidental sharing and malicious exfiltration.
*   **Enable Remote & Cloud Work:** Collaborate safely with contractors, clients, and distributed teams without losing control over your data.
*   **Meet Compliance:** Simplify regulatory reporting (GDPR, HIPAA, SOX, etc.) with comprehensive, auditable logs.
*   **Resilience:** Drastically reduce the risk associated with insider threats, malware, and ransomware targeting your critical files.
[Read more about secure file sharing example](https://www.easefilter.com/Forums_Files/AssureFiles_Secure_File_Sharing.htm)

## EaseFilter File System Filter Driver SDK Reference
| Product Name | Description |
| --- | --- |
| [Cloud File System SDK](https://www.easefilter.com/cloud/cloud-file-system-sdk.htm) | EaseFilter Cloud File System SDK Introduction. |
| [CloudTier Storage Tiering SDK](https://www.easefilter.com/cloud/storage-tiering-sdk.htm) | EaseFilter Storage Tiering Filter Driver SDK Introduction. |
| [File Monitor SDK](https://www.easefilter.com/kb/file-monitor-filter-driver-sdk.htm) | EaseFilter File Monitor Filter Driver SDK Introduction. |
| [File Control SDK](https://www.easefilter.com/kb/file-control-file-security-sdk.htm) | EaseFilter File Control Filter Driver SDK Introduction. |
| [File Encryption SDK](https://www.easefilter.com/kb/transparent-file-encryption-filter-driver-sdk.htm) | EaseFilter Transparent File Encryption Filter Driver SDK Introduction. |
| [Registry Filter SDK](https://www.easefilter.com/kb/registry-filter-drive-sdk.htm) | EaseFilter Registry Filter Driver SDK Introduction. |
| [Process Filter SDK](https://www.easefilter.com/kb/process-filter-driver-sdk.htm) | EaseFilter Process Filter Driver SDK Introduction. |
| [EaseFilter SDK Programming](https://www.easefilter.com/kb/programming.htm) | EaseFilter Filter Driver SDK Programming. |

## EaseFilter SDK Sample Projects
| Sample Project | Description |
| --- | --- |
| [CloudTier Storage Tiering Demo](https://www.easefilter.com/cloud/cloudtier-storage-tiering-demo.htm) | A HSM File System Filter Driver Demo. |
| [CloudTier S3 Tiering Demo](https://www.easefilter.com/cloud/cloudtier-s3-intelligent-tiering-demo.htm) | CloudTier S3 Intelligent Tiering Demo. |
| [Cloud File DR S3 Demo](https://www.easefilter.com/cloud/cloud-file-dr-demo.htm) | Cloud File DR S3 Demo. |
| [Amazon S3 File Explorer Demo](https://www.easefilter.com/cloud/s3-browser-demo.htm) | Amazon S3 File Explorer Demo. |
| [Auto File DRM Encryption](https://www.easefilter.com/kb/auto-file-drm-encryption-tool.htm) | Auto file encryption with DRM data embedded. |
| [Transparent File Encrypt](https://www.easefilter.com/kb/AutoFileEncryption.htm) | Transparent on access file encryption. |
| [Secure File Sharing with DRM](https://www.easefilter.com/kb/DRM_Secure_File_Sharing.htm) | Secure encrypted file sharing with digital rights management. |
| [File Monitor Example](https://www.easefilter.com/kb/file-monitor-demo.htm) | Monitor file system I/O in real time, tracking file changes. |
| [File Protector Example](https://www.easefilter.com/kb/file-protector-demo.htm) | Prevent sensitive files from being accessed by unauthorized users or processes. |
| [FolderLocker Example](https://www.easefilter.com/kb/FolderLocker.htm) | Lock file automatically in a FolderLocker. |
| [Process Monitor](https://www.easefilter.com/kb/Process-Monitor.htm) | Monitor the process creation and termination, block unauthorized process running. |
| [Registry Monitor](https://www.easefilter.com/kb/RegMon.htm) | Monitor the Registry activities, block the modification of the Registry keys. |
| [Secure Sandbox Example](https://www.easefilter.com/kb/Secure-Sandbox.htm) |A secure sandbox example, block the processes accessing the files out of the box. |
| [FileSystemWatcher Example](https://www.easefilter.com/kb/FileSystemWatcher.htm) | File system watcher, logging the file I/O events. |
| [ZeroTrust Example](https://www.easefilter.com/kb/zero-trust-file-access-control-demo.htm) | Zero trust file access control with encryption feature. |

## Filter Driver Reference

* [Understand MiniFilter Driver](https://www.easefilter.com/kb/understand-minifilter.htm)
* [Understand File I/O](https://www.easefilter.com/kb/File_IO.htm)
* [Understand I/O Request Packets(IRPs)](https://www.easefilter.com/kb/understand-irps.htm)
* [Filter Driver Developer Guide](https://www.easefilter.com/kb/DeveloperGuide.htm)
* [MiniFilter Filter Driver Framework](https://www.easefilter.com/kb/minifilter-framework.htm)
* [Isolation Filter Driver](https://www.easefilter.com/kb/Isolation_Filter_Driver.htm)

## License

This repository contains two distinct licensing categories:

1. **Wrapper/Implementation Code (MIT License):** 
   The source code, scripts, and configuration files provided directly in this repository are licensed under the [MIT License](LICENSE). You are free to modify, distribute, and use this specific integration code.

2. **EaseFilter SDK (Commercial License):**
   This project requires the EaseFilter SDK (including its binaries, filter drivers, and core libraries) to function. The EaseFilter SDK is **proprietary commercial software** and is strictly excluded from the MIT License.   
   To use the EaseFilter SDK for commercial purposes or in a production environment, you must obtain a valid commercial license directly from [EaseFilter](https://www.easefilter.com/license.pdf).

## Support
If you have questions or need help, please contact support@easefilter.com 

[Home](https://www.easefilter.com/) | [Solution](https://www.easefilter.com/solutions.htm) | [Download](https://www.easefilter.com/download.htm) | [Demos](https://www.easefilter.com/online-fileio-test.aspx) | [Blog](https://blog.easefilter.com/) | [Programming](https://www.easefilter.com/kb/programming.htm)
