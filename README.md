# hackintosh-ryzen-efi
Hackintosh-MacOS Monterey 12.6 ryzen r5 1600af Sapphire rx590 pulse Gigabyte B450M S2H opencore 0.8.5 version

---
This efi folder is made using OpenCore documentation [OpenCore (opens new window)](https://dortania.github.io/)
If you want a more detailed document, you can refer to the OpenCore guide.

We are not responsible for any errors.**
---

the tools we need

-   [ProperTree(opens new window)](https://github.com/corpnewt/ProperTree)
    -   Universal plist editor
-   [GenSMBIOS(opens new window)](https://github.com/corpnewt/GenSMBIOS)
    -   For generating our SMBIOS data
---
Next, let's open ProperTree and edit our config.plist:

-   `ProperTree.command`
    -   For macOS
    -   Pro tip: there's a  `buildapp.command`  utility in the  `Scripts`  folder that lets you turn ProperTree into a dedicated app in macOS
-   `ProperTree.bat`
    -   For Windows

Once ProperTree is running, open your config.plist  by pressing  **Cmd/Ctrl + O**  and selecting the  `config.plist` 

    path->EFI->OC->config.plist
---
**First step:**
You need write to `kernel->patch-> 0,1,2 blocks` to change the `Replace` value your cpu model how many core have .
  So for example a 6 Core 5600X Replace value. We write hexadecimal replace label to value
  
B8 **06** 0000 0000 / BA **06** 0000 0000 / BA **06** 0000 0090

| CoreCount | Hexadecimal|
|--------|---------|
|   4 Core  | `04` |
|   6 Core  | `06` |
|   8 Core  | `08` |
|   12 Core | `0C` |
|   16 Core | `10` |
|   24 Core | `18` |
|   32 Core | `20` |

---
**Second step:**
We need a hackintosh id because necessary for the stable operation of the system. 
[look this](https://github.com/hnnesimioglu/hackintosh-ryzen-efi/blob/main/pleaseOpenandEditBeforeInstall.png)
For setting up the SMBIOS info, we'll use CorpNewt's  [GenSMBIOS (opens new window)](https://github.com/corpnewt/GenSMBIOS)application.

For this example, we'll choose the MacPro7,1 SMBIOS but some SMBIOS play with certain GPUs better than others:

-   MacPro7,1: AMD Polaris and newer
    -   Note that MacPro7,1 is exclusive to macOS 10.15, Catalina and newer
-   iMacPro1,1: NVIDIA Maxwell and Pascal or AMD Polaris and newer
    -   Use if you need High Sierra or Mojave, otherwise use MacPro7,1
-   iMac14,2: NVIDIA Maxwell and Pascal
    -   Use if you get black screens on iMacPro1,1 after installing Web Drivers with an NVIDIA GPU
-   MacPro6,1: AMD GCN GPUs (supported HD and R5/R7/R9 series)

Run GenSMBIOS, pick **option 1** for downloading MacSerial and **Option 3** for selecting out SMBIOS. This will give us an output similar to the following:

The order is  `Product | Serial | Board Serial (MLB)`

The  `Type`  part gets copied to Generic -> SystemProductName.

The  `Serial`  part gets copied to Generic -> SystemSerialNumber.

The  `Board Serial`  part gets copied to Generic -> MLB.

The  `SmUUID`  part gets copied to Generic -> SystemUUID.

The  `Apple ROM`  part gets copied to Generic -> ROM.

**Reminder that you need an invalid serial! When inputting your serial number in  [Apple's Check Coverage Page (opens new window)](https://checkcoverage.apple.com/), you should get a message such as "Unable to check coverage for this serial number."**

## all transactions are completed. Now you can upload the EFI folder to the EFI partition on your disk and start installing hackintosh.
