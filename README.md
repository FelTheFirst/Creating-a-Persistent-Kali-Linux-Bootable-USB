# Creating a Persistent Kali Linux Live USB

## Objective
The objective of this project is to demonstrate how to create a persistent Kali Linux Live USB using Rufus, from the point of view of a Windows 11 machine. This setup allows you to run Kali Linux from the USB drive without installing it on the host system, while enabling persistence to save data and configurations across reboots. This provides a portable and reliable security testing environment that can be used on different machines.

Additionally, we will cover important security verification practices, such as checking the SHA256 hash of the Kali Linux Live ISO to ensure file integrity, and explaining Rufus’s certificate-based verification for confirming the authenticity of the downloaded file.

### Skills Learned
  - Creating a bootable USB with persistence using Rufus
  - Understanding the role of persistence in penetration testing and cybersecurity operations
  - Troubleshooting USB-related issues such as partitioning and boot errors
  - Familiarity with managing USB boot settings and Linux operating systems
  - Practical experience with Linux and Kali Linux in particular

### Tools Used
  - [Kali Linux](https://www.kali.org/get-kali/#kali-live) ISO: The operating system used for the bootable USB
  - [Rufus](https://rufus.ie/): A tool for creating a bootable USB drive with persistence, which simplifies the process by enabling persistence without the need for additional tools or partitioning
  - [Samsung Bar 3.1 128GB USB Drive](https://www.samsung.com/us/computing/memory-storage/usb-flash-drives/usb-3-1-flash-drive-bar-plus-128gb-champagne-silver-muf-128be3-am/): Is the USB drive I used for creating my bootable environment. To follow along, you do not need to have the same USB. However, I recommend one with at least 18GB of storage to supply sufficient capacity for both the Kali Linux ISO and persistence storage. Additionally I recommend at least USB version 3.0 or newer for optimum data transfer speed. 


## Steps

### 1. **Download the Kali Linux Live ISO** <br>
   First, always make sure you are getting your downloads directly from the reputable manufacturers. With that in mind you can download the official Kali Linux Live ISO safely from [here](https://www.kali.org/get-kali/#kali-live). I will be using the recommended file displayed in the following screenshot, as I am using a Windows 11 Machine. Please select the download most compatible with your system. <br>
    <img src="https://github.com/user-attachments/assets/104b9f08-d0e8-4227-88f4-c9e36d183798" width="400"/>
   <br>
 
### 2. **Copy the file path for the Kali Linux Download.** <br>
Open File Explorer, and locate the Kali Linux file we just downloaded. <br>
Right click the file and select "Copy as path" <br>
Paste that into your preferred work space. We will reference it again in step 3b. <br>
<img src="https://github.com/user-attachments/assets/d13ec119-2fda-40eb-9fd7-fcbd538ddabf" width="400"/>
 
### 3. **Verify the SHA256 Hash of the Kali Linux ISO** <br>
Before proceeding with the creation of the persistent Kali Linux USB, it’s important to verify the integrity of the downloaded Kali Linux ISO to ensure that the file has not been corrupted or tampered with during the download process. <br>
<br>
While Rufus, the tool we will use in step 4, will perform its own certificate-based verification to confirm the authenticity of the Kali Linux ISO, we will manually verify the file using the SHA256 hash as an additional best practice. This extra layer of verification helps ensure that the file you downloaded is exactly what it’s supposed to be, without any alterations or corruption. <br>
<br>
The process of verifying the SHA256 hash consists of the following three steps: <br>
<br>
 ### 3a. **Obtain the SHA256 Checksum from the Kali Linux Website**  <br> 
  Find the SHA256 Checksum for your specific Kali download by clicking the "Sum" button for the version you downloaded.  
       <img src="https://github.com/user-attachments/assets/43d3a204-98da-4d8e-9a21-983a81e56168" width="400"/>

### 3b. **Generate the SHA256 hash value for your downloaded Kali Linux file** <br>
 Press `Windows + R`, type `powershell`, and press `Enter`.  
 **IMPORTANT:** In the code below, replace `C:\path\to\your\file.iso` with the actual file path you copied and saved in Step 1.  
 In PowerShell, run the following command:  

```powershell
Get-FileHash "C:\path\to\your\file.iso" -Algorithm SHA256
```

This will output a SHA256 hash value specific to your local file. I ran this code and highlighted my results in yellow below. <br>

<img src="https://github.com/user-attachments/assets/9aaa2339-280d-463a-9922-d761e558c6e3" width="600"/> <br>

 ### 3c. **Compare the two values**
A SHA256 checksum generates a unique cryptographic hash for a file, allowing users to verify its integrity by comparing it to a trusted hash, ensuring the file hasn't been tampered with or corrupted. Trust the checksum provided by Kali Linux and use it as the baseline for comparison. <br>
Compare the value of that checksum, which you identified in step 3a, to the value you generated in step 3b. <br>
If the two values do not match exactly, there is an issue with your file. This could be caused by various factors. Do not open the file, and delete it from your system immediately. <br>
Only proceed with creating your bootable USB if the hash value of your file matches the provided SHA256 checksum. <br>
As shown in the images above, my values match, so I can proceed with confidence. <br>

### 4. **Install Rufus**  <br>
   Rufus is a powerful and efficient tool for creating bootable USB drives. We’re using it in this project because it simplifies the process of enabling persistence, making it quick and straightforward without the need for additional configuration or partitioning.<br>
<br>
Before downloading Rufus, it’s important to note that it is an open-source tool licensed under the GNU General Public License (GPL) version 3 or later. Open-source software is defined by its emphasis on freedom. Specifically, the freedom to view, modify, and distribute the source code. While those capabilities are beyond the scope of this project, it’s worth highlighting that Rufus is also free to use in the more familiar sense, often described in the open-source community as "free as in free beer." <br>
<br>
Because Rufus is provided at no cost to the public, the website may contain advertisements. Be mindful to avoid clicking on ads and instead navigate directly to the Latest Releases section. From there, select the blue hyperlink for the version appropriate to your system. For my Windows 11 machine, I chose the following version:<br>
<br>
rufus-4.6.exe Standard Windows x64 (1.5 MB). <br>
<br>
This is the first option shown in the accompanying screenshot below. Download Rufus only from the official site to ensure safety and authenticity.
   Download the tool from this [site](https://rufus.ie/).   
   <img src="https://github.com/user-attachments/assets/cb4af6c8-4e1f-4336-a4f4-98a92d626c95" width="600"/> <br>

### 5. **Create the Persistent Kali Linux Live USB with Rufus** <br>
 We’ve verified the integrity of the Kali Linux ISO and downloaded Rufus, and now we're ready for the fun stuff. We’re going to create the bootable USB with persistence. This will allow your Kali Linux environment to retain data and configuration changes across reboots. Meaning you can save files, download applications and make edits to your system and it will all still be that way when you log back on the next time. <br>
 
 ### 5a. **Launch Rufus** <br>
 Just like checking the SHA256 hash to ensure the integrity of the downloaded file, verifying the publisher is a vital security step. While some tools release their SHA value for this purpose, Rufus does not 
 because it uses a digital signature embedded within its certificate. This certificate-based verification helps confirm that the version of Rufus you're using is legitimate and has not been tampered with. By 
 checking that the publisher is 'Akeo Consulting' in the User Account Control (UAC) prompt, you ensure that you are running the trusted version of the software.<br>
 <br>
 With this important security step in mind, launch Rufus through your preferred method. <br>
 <br>
 When prompted by User Account Control (UAC), verify that the publisher listed is 'Akeo Consulting.' If the publisher is correct, click 'Yes' to allow Rufus to make changes to your device<br>
 <br>

  ### 5b. **Drive Properties** <br>
  
  This step is critical to ensuring your Kali Linux Live USB works properly with persistence enabled. Follow these instructions carefully:
  
   1. From the Device dropdown menu, choose the USB drive you want to use. ALL DATA ON THIS DEVICE WILL BE PERMENANTLY EREASED
   PLEASE SELECT WITH CAUTION.

   2. Click the SELECT button next to Boot Selection, and navigate to the Kali Linux Live ISO file you downloaded in Step 1.

   3. Move the Persistent Partition Size slider to its maximum value. This partition will store files and save changes across reboots. Without persistence, any files or settings you create in Kali Linux will be lost when you shut down.
      
 <img src="https://github.com/user-attachments/assets/9e4e87fb-d464-44b6-b400-fe774aa47154" width="400" height="500"/>


 ### 5c. **Partition Scheme**

 The Partition Scheme determines how data is organized on your USB drive and how your computer’s firmware interacts with the drive. There are two main options:

 MBR (Master Boot Record): An older, widely compatible partitioning method that works with both BIOS and many UEFI systems. It’s simple and reliable but has limitations like supporting drives only up to 2TB and a maximum of four primary partitions.

 GPT (GUID Partition Table): A newer standard designed for modern UEFI systems. It supports larger drives (over 2TB) and more partitions, but it’s not always compatible with older machines using BIOS.

 For this tutorial, we will leave the Partition Scheme set to MBR because it offers the most compatibility across different systems. If you know your computer exclusively uses UEFI, you may want to choose GPT. For simplicity and ease in this tutorial, MBR is our choice.

 ### 5d. **File System**

The File System determines how data is stored and retrieved on the USB drive. In Rufus, the default for creating a Kali Linux bootable USB is usually FAT32. Here’s why we leave it at the default:

FAT32 (File Allocation Table 32):

Pros: Highly compatible with most operating systems (Windows, Linux, macOS) and bootloaders.

Cons: Has a 4GB file size limit — but this isn’t an issue here, as the Kali ISO and persistence files are well below this limit.

Other options like NTFS or exFAT aren’t ideal because they can cause compatibility issues with the Linux-based Kali Live environment. For the broadest compatibility and easiest setup, FAT32 is the best choice.

 ### 5e. **Cluster Size**

 The Cluster Size defines how much space each unit of storage occupies. Rufus defaults to 32KB, which works well for most users.

While you can adjust the cluster size for specific needs (larger for performance with big files, smaller for efficiency with small files), the default setting is usually sufficient for most use cases.

For simplicity, we’ll stick with the default 32KB cluster size

### 5f. **Press Start**

Now that we have made all the selections we need to make and understand why we left some settings at their default, we can finally wrap up this section.

Go ahead and hit start! You may see a prompt informing you that all data on the chosen device will be destroyed and asking you if you want to continue. Hopefully, you selected a USB drive that either currently houses no data, or only data you are prepared to lose forever. If you have, select proceed and select Yes. If you have not, you will need to start step 5 over with a USB drive that you are prepared to have wiped.

<img src="https://github.com/user-attachments/assets/43b23a79-6655-46fe-9cd7-132c3df4357c" width="400" height="500"/>

Rufus will take over from here. It may take a little bit, so feel free to take a break and return later.  :)

Once the process is completed, as indicated on status bar, you may close Rufus.

## ***Congratulations! You should now have a Kali Linux Live bootable USB drive with persistence!***

Now, let's move on to the booting process, but first, let's make sure we understand what we're doing.

Booting is the process of starting up a computer, where the system loads essential software, such as the operating system and kernel, from storage into memory, making the computer operational.

Boots typically work with the default kernel by loading the operating system's core files from the boot device, which then initializes the system hardware and begins the process of starting up the rest of the operating system.

In most cases, this is great, but we want to boot Kali Linux, and that boot information is now stored on our USB drive.

So, we need to adjust something called Boot Priority.

Boot priority determines the order in which a computer's system firmware (BIOS or UEFI) checks devices (such as hard drives, USB drives, or network interfaces) to find a bootable operating system, allowing you to choose which device the system will attempt to boot from first.

We will change our boot priority to USB so that our system will boot from there first, before trying it's default OS, in my case Windows 11.

The last concept we should familiarize ourselves with before proceeding is Secure Boot.

Secure Boot is a security standard designed to protect your system from malicious software. When enabled, it ensures that only trusted, signed operating systems and bootloaders are allowed to run by checking their digital signatures during startup. If a bootloader isn't signed or doesn’t meet certain security requirements, the system will block it from running.

In most cases, Secure Boot is a good security measure, especially for Windows OS. However, it can interfere with booting non-Windows operating systems like Kali Linux, which may not have the necessary signature.

Since Kali Linux is often unsigned or not recognized by Secure Boot, we need to turn off Secure Boot to allow the system to boot from the USB drive with Kali Linux on it.

Make sure you understand and feel comfortable with this step. We are essentially reducing the security posture of our machine. You need to be aware that while Secure Boot is disabled, your machine is at risk of certain attacks, including rogue USB attacks.

Make sure to re-enable Secure Boot when you're not using your Kali Linux system. On my machine, I need to re-enable Secure Boot before my Windows 11 OS will even boot again. Don't be alarmed if this happens to you too. Your machine might boot to a blue screen BIOS or UEFI, but if you understand how we disabled Secure Boot, you should feel ready to re-enable it when necessary.

EVEN IF YOUR MACHINE DOES NOT REQUIRE SECURE BOOT TO BE RE-ENABLED TO BOOT TO YOUR ORIGINAL OS, PLEASE RE-ENABLE IT ANYWAY.
IT IS AN IMPORTANT SECURITY FEATURE, AND YOU ONLY WANT IT OFF WHILE IT SERVES A PURPOSE.

Now that you understand the theory behind it let's move on to the technical steps.


