---
Description: 'WinPE: Create USB Bootable drive'
MS-HAID: 'p\_adk\_online.winpe\_create\_usb\_bootable\_drive'
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: 'WinPE: Create USB Bootable drive'
---

# WinPE: Create USB Bootable drive


Create a Windows PE (WinPE) bootable USB flash drive or an external USB hard drive.

The default installation runs from memory (RAM disk), so you can remove the drive while Windows PE is running.

**Install the Windows ADK**

-   Install the following features from the [Windows Assessment and Deployment Kit (ADK)](http://go.microsoft.com/fwlink/?LinkId=526803):

    -   **Deployment Tools**: includes the **Deployment and Imaging Tools Environment**.

    -   **Windows Preinstallation Environment** : includes the files used to install Windows PE.

**Install Windows PE**

1.  Start the **Deployment and Imaging Tools Environment** as an **administrator**.

2.  Create a working copy of the Windows PE files. Specify either x86, amd64, or arm:

    ``` syntax
    copype amd64 C:\WinPE_amd64
    ```

3.  Install Windows PE to the USB flash drive, specifying the drive letter:

    ``` syntax
    MakeWinPEMedia /UFD C:\WinPE_amd64 F:
    ```

    **Warning**  
    This command reformats the drive.

     

**Boot to Windows PE**

1.  Connect the USB device to the PC you want to work on.

2.  Turn on the PC, and press the key that opens the firmware boot menus.

3.  Select the USB drive. Windows PE starts automatically.

    After the command window appears, the `wpeinit` command runs, which sets up the system. This might take a few minutes.

**Troubleshooting**

1.  If the `copype` command isn't recognized, make sure you're running the command from the Deployment and Imaging Tools Environment, which is part of the Windows ADK.

2.  If Windows PE doesn't appear, try the following workarounds, rebooting the PC each time:

    -   To boot a PC that supports UEFI mode, in the firmware boot menus, try manually selecting the boot files: \\EFI\\BOOT\\BOOTX64.EFI.

    -   Try a different USB port. Avoid hubs or cables.

    -   Avoid USB 3.0 ports if the firmware doesn't contain native support for USB 3.0.

    -   Clean the USB flash drive, and then reinstall Windows PE. This can help remove extra boot partitions or other boot software.

        ``` syntax
        diskpart
          list disk
          select disk <disk number>
          clean
          create partition primary
          format quick fs=fat32 label="Windows PE"
          assign letter="F"
          exit

        MakeWinPEMedia /UFD C:\winpe_amd64 F:
        ```

    -   Try booting Windows PE from a DVD instead. Create an ISO file that you can burn onto a DVD:

        ``` syntax
        MakeWinPEMedia /ISO C:\winpe_amd64 c:\winpe_amd64\winpe.iso
        ```

        In File Explorer, navigate to C:\\winpe\_amd64, right-click **winpe.iso**, and select **Burn to disc**. Follow the prompts to create a DVD.

    -   If your PC requires storage or video drivers to boot, try adding those same drivers to the Windows PE image. For more info, see [WinPE: Mount and Customize](winpe-mount-and-customize.md).

    -   Update the firmware of the PC to the latest version.

3.  If the PC doesn't connect to network locations, see [WinPE Network Drivers: Initializing and adding drivers](winpe-network-drivers-initializing-and-adding-drivers.md).

**Storing Windows Images on the Windows PE Drive**

1.  Typically you won't be able to store or capture Windows images on a Windows PE USB flash drive.

    Most USB flash drives support only a single drive partition. The `MakeWinPEMedia` command formats the drive as FAT32, which supports booting both BIOS-based and UEFI-based PCs. This file format only supports file sizes up to 4 GB.

2.  For external USB hard drives, you can create a separate NTFS partition that can handle larger file sizes:

    ``` syntax
    diskpart
      list disk
      select <disk number>
      clean
      rem === Create the Windows PE partition. ===
      create partition primary size=2000
      format quick fs=fat32 label="Windows PE"
      assign letter=P
      active
      rem === Create a data partition. ===
      create partition primary
      format fs=ntfs quick label="Other files"
      assign letter=O
      list vol
      exit

    MakeWinPEMedia /UFD C:\WinPE_amd64 P:
    ```

## <span id="related_topics"></span>Related topics


[WinPE for Windows 10](winpe-intro.md)

[WinPE: Create a Boot CD, DVD, ISO, or VHD](p_adk_online.winpe_create_a_boot_cd_dvd_iso_or_vhd_blue)

[WinPE: Install on a Hard Drive (Flat Boot or Non-RAM)](winpe-install-on-a-hard-drive--flat-boot-or-non-ram.md)

[WinPE: Mount and Customize](winpe-mount-and-customize.md)

[WinPE: Boot in UEFI or legacy BIOS mode](p_adk_online.winpe_boot_in_uefi_or_legacy_bios_mode_blue)

[Windows Setup Supported Platforms and Cross-Platform Deployments](windows-setup-supported-platforms-and-cross-platform-deployments.md)

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bp_adk_online\p_adk_online%5D:%20WinPE:%20Create%20USB%20Bootable%20drive%20%20RELEASE:%20%284/11/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")



