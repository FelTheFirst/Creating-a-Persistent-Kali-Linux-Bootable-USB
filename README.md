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

  ### 5b. **Launch Rufus** <br>
 
