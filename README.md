# Creating a Persistent Kali Linux Live USB

## Objective
The objective of this project is to demonstrate how to create a persistent Kali Linux Live USB using Rufus, from the point of view of a Windows 11 machine. This setup allows you to run Kali Linux from the USB drive without installing it on the host system, while enabling persistence to save data and configurations across reboots. This provides a portable and reliable security testing environment that can be used on different machines.

Additionally, we will cover important security verification practices, such as checking the SHA256 hash of the Kali Linux Live ISO to ensure file integrity, and explaining Rufus’s certificate-based verification for confirming the authenticity of the downloaded file.

### Skills Learned
  - Creating a persistent bootable USB using Rufus
  - Understanding the role of persistence in penetration testing and cybersecurity operations
  - Ensuring file integrity and authenticity by verifying the SHA256 hash and using Rufus’s certificate-based validation
  - Navigating BIOS/UEFI settings to configure USB boot options for a live operating system
  - Gaining familiarity with the Kali Linux environment and executing basic terminal commands

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
A SHA256 checksum generates a unique cryptographic hash for a file, allowing users to verify its integrity by comparing it to a trusted hash, ensuring the file hasn't been tampered with or corrupted. Trust the checksum provided by Kali Linux and use it as the baseline for comparison. 

Compare the value of that checksum, which you identified in step 3a, to the value you generated in step 3b. 

If the two values do not match exactly, there is an issue with your file. This could be caused by various factors, ranging from something benign like a corrupted file due to download errors, to something more sinister, such as a man-in-the-middle attack. Do not open the file, and delete it from your system immediately 

Only proceed with creating your bootable USB if the hash value of your file matches the provided SHA256 checksum. 

As shown in the images above, my values match, so I can proceed with confidence. 

### 4. **Install Rufus**  <br>

Rufus is a powerful and efficient tool for creating bootable USB drives. We’re using it in this project because it simplifies the process of enabling persistence, making it quick and straightforward without the need for additional configuration or partitioning.

Before downloading Rufus, it’s important to note that it is an open-source tool licensed under the GNU General Public License (GPL) version 3 or later. Open-source software is defined by its emphasis on freedom. Specifically, the freedom to view, modify, and distribute the source code. While those capabilities are beyond the scope of this project, it’s worth highlighting that Rufus is also free to use in the more familiar sense, often described in the open-source community as "free as in free beer." 

Because Rufus is provided at no cost to the public, the website may contain advertisements. Be mindful to avoid clicking on ads and instead navigate directly to the Latest Releases section. From there, select the blue hyperlink for the version appropriate to your system. For my Windows 11 machine, I chose the following version:

rufus-4.6.exe Standard Windows x64 (1.5 MB). 

This is the first option shown in the accompanying screenshot below. Download Rufus only from the official site to ensure safety and authenticity.
   Download the tool from this [site](https://rufus.ie/).   
   <img src="https://github.com/user-attachments/assets/cb4af6c8-4e1f-4336-a4f4-98a92d626c95" width="600"/> 

### 5. **Create the Persistent Kali Linux Live USB with Rufus** <br>
We’ve verified the integrity of the Kali Linux ISO and downloaded Rufus, and now we're ready for the fun stuff. We’re going to create the bootable USB with persistence. This is a crucial feature for portable security testing environments. Persistence allows your Kali Linux setup to retain data and configuration changes across reboots, meaning you can save files, download applications, and make edits to your system. Everything will still be there the next time you log in.
 
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
  
   1. From the Device dropdown menu, choose the USB drive you want to use. ALL DATA ON THIS DEVICE WILL BE PERMANENTLY ERASED.
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


# ***Congratulations! You should now have a Kali Linux Live bootable USB drive with persistence!***

let's move on to the booting process, but first, let's make sure we understand what we're doing.

## **Accessing BIOS/UEFI Theory**

For the next section, we will have to navigate into the BIOS/UEFI of our machine. I want to be clear: once you go into BIOS/UEFI, you will not easily be able to go back and forth to reference this document. You may want to open this page on another screen not tied to the machine you will be using to boot into Kali Linux. If that’s not an option, take your time to read through the remainder of this material. Only proceed once you feel confident in your ability to complete all steps without guidance.

Additionally, the process for entering BIOS/UEFI varies greatly from machine to machine. Some involve pressing F2 or Del upon reboot, but hotkey methods can differ. I will show the most universal and common way to enter BIOS/UEFI on Windows 11, but you may need to search the internet for help achieving this on your specific machine.

If this is your first experience with BIOS/UEFI, I suggest going to YouTube and watching some videos on it, preferably videos with a machine similar to yours. That way, you know what to expect when you’re in there, as BIOS/UEFI interfaces can be very different from a standard computer interface (you will likely navigate using only your keyboard and no mouse), and BIOS/UEFI interfaces can differ drastically between machines. For instance, even though they both run Windows 11, an Intel and an Asus machine might have completely different interfaces. It is best to take a second and familiarize yourself so you don't feel overwhelmed once you are inside.

## **Boot Priority Theory**

Booting is the process of starting up a computer, where the system loads essential software, such as the operating system and kernel, from storage into memory, making the computer operational.

Boots typically work with the default kernel by loading the operating system's core files from the boot device, which then initializes the system hardware and begins the process of starting up the rest of the operating system.

In most cases, this is great, but we want to boot Kali Linux, and that boot information is now stored on our USB drive.

So, we need to adjust something called Boot Priority.

Boot priority determines the order in which a computer's system firmware (BIOS or UEFI) checks devices (such as hard drives, USB drives, or network interfaces) to find a bootable operating system, allowing you to choose which device the system will attempt to boot from first.

We will change our boot priority to USB so that our system will boot from there first, before trying its default OS, in my case Windows 11.

## **Secure Boot Theory**

The last concept we should familiarize ourselves with before proceeding is Secure Boot.

Secure Boot is a security standard designed to protect your system from malicious software. When enabled, it ensures that only trusted, signed operating systems and bootloaders are allowed to run by checking their digital signatures during startup. If a bootloader isn't signed or doesn’t meet certain security requirements, the system will block it from running.

In most cases, Secure Boot is a good security measure, especially for Windows OS. However, it can interfere with booting non-Windows operating systems like Kali Linux, which may not have the necessary signature.

Since Kali Linux is often unsigned or not recognized by Secure Boot, we need to turn off Secure Boot to allow the system to boot from the USB drive with Kali Linux on it.

Make sure you understand and feel comfortable with this step. We are essentially reducing the security posture of our machine. You need to be aware that while Secure Boot is disabled, your machine is at risk of certain attacks, including rogue USB attacks.

Make sure to re-enable Secure Boot when you're not using your Kali Linux system. On my machine, I need to re-enable Secure Boot before my Windows 11 OS will even boot again. Don't be alarmed if this happens to you too. Your machine might boot to a blue screen BIOS or UEFI, but if you understand how we disabled Secure Boot, you should feel ready to re-enable it when necessary.

EVEN IF YOUR MACHINE DOES NOT REQUIRE SECURE BOOT TO BE RE-ENABLED TO BOOT TO YOUR ORIGINAL OS, PLEASE RE-ENABLE IT ANYWAY.
IT IS AN IMPORTANT SECURITY FEATURE, AND YOU ONLY WANT IT OFF WHILE IT SERVES A PURPOSE.

Now that you understand the theory behind it, let's move on to the technical steps. Also, please note that I do not have the capability to screenshot my UEFI interface. So the following explanations will only have text. This is another reason I recommend watching a YouTube video on your/any BIOS/UEFI interface.

## 1. **Launch BIOS/UEFI using the SHIFT + Restart Method on a Windows 11 Machine**

1a. Hold down the Shift key on your keyboard.
   
1b. While holding Shift, click the Start button, then select Power > Restart.
   
1c. Your PC will reboot into the Advanced Startup menu.
   
1d. Go to Troubleshoot > Advanced options > UEFI Firmware Settings > Restart.

## 2. **Change Settings**
This step is more difficult for me to explain via text. As explained earlier, there are various ways in which the BIOS/UEFI may function depending on your machine.

It is likely that you will have to navigate using arrows or hotkeys.

I will explain the process as it works for me on my Intel Windows machine.

1. I am initially presented with a startup menu containing options. At this point, I must select F10 for BIOS Setup. You will be looking for something worded similarly if presented with options.
2. Then I am brought to a screen with several section headers up top. I use my arrows to navigate to the section titled Boot Options.
3. In my Boot Options, I can see options for both Boot Priority and Secure Boot.
4. I use my arrows to navigate down to Secure Boot and press ENTER.
5. I receive a pop-up which states "Enable" and "Disable".
6. I use arrows to navigate to "Disable" and I hit ENTER.
7. I am returned to my prior screen and my Secure Boot now states Disabled.
8. I use my arrow keys to navigate slightly further down the page until I reach Boot Order. Under Boot Order, I see a list of devices.
9. Once I am there, instructions appear to my left.
10. They state that I can use my up and down arrows to select a device, and my F5 and F6 keys to move the devices up and down in the list.
11. I move USB to the very top of the list, which gives it priority.
12. I use my arrow keys to navigate back to the top menu and select Exit.
13. Under the Exit menu, I see "Save Changes and Exit".
14. I navigate there and press ENTER.
15. I receive a pop-up and press ENTER again to confirm I want to save changes and exit.

At this point, the machine exits BIOS/UEFI and restarts.

On my machine, I am prompted with a blue screen that informs me that changes have been made to my system. It asks me to confirm these changes by entering the supplied password and pressing Enter. I comply, and my machine begins to boot Kali Linux.

Upon this boot, and every boot from the USB drive from here on out, I am presented with the Kali Linux Live Menu.

At this menu, use your arrow keys to navigate to the fourth option, which states "Live system with USB Persistence," and hit Enter to select it.

Your machine should now proceed to boot into Kali Linux Live.

<img src="https://github.com/user-attachments/assets/11bc3672-8fa0-45c2-847a-30b811dc5826" width="600"/>

If you see this screen then...

### Congratulations! You now have your very own Kali Linux Machine to explore! 

I hope this proves to be a worthwhile tool for you. Use it well and ethically. I believe in you! :)

You can call it a day here. However, if you want to check your persistence (and see a tiny bit of BASH on a Linux terminal), follow the next steps.

### **Check the persistence of your USB**

1.  In your Kali machine, open the terminal. The icon for the terminal is near the top right-hand corner.
2.  Linux Bash and the file system are case-sensitive, so be exact and type the command as it appears below:
    touch USBProject
3. Hit Enter. You have now created a text file titled "USBProject." (In my screenshot, you will see I took it a step further and added words to the file, but this is not necessary.) <br>
   <img src="https://github.com/user-attachments/assets/6b1b340b-1720-4713-a58b-2097d4945618" width="600"/>
4. Exit the terminal by either clicking the "X" or typing exit, then turn off your computer.
5. Turn your computer back on, making sure to select "Live system with USB Persistence" again when prompted.
6. Check to see if your file is still there by opening the terminal and typing the command:
   ls
7. You should see the file "USBProject" listed among the output. This proves that your system has persistence.
   <img src="https://github.com/user-attachments/assets/05c394e2-c712-4dac-89f5-b035bfbcd259" width="600"/>

If, for any reason, you do not see it there, please reread this tutorial and make sure you have followed all the steps exactly.


## Thank you
Thank you for taking the time to read through this project. I hope you successfully followed along and made your first Kali Linux Live Bootable USB Drive.

If you did, feel free to let me know on LinkedIn.

Have a great day, and keep learning! :)

-Fel






