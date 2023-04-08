# NessusScanner
This repo focuses on a hands on lab with Nessus. This is a summary of running the scan on a Windows 10 Virtual Machine.


# Setup
## 1. Download and install VMware Player on your computer or setup another hypervisor such as Hyper-V.  
https://www.vmware.com/products/workstation-player.html  

## 2. Download the Windows 10 ISO file and save it to your computer.  
https://www.microsoft.com/en-us/software-download/windows10  
You'll have to walk through the installer and select the ISO option.  

## 3. Download and install Nessus Essentials.
https://www.tenable.com/products/nessus/nessus-essentials  
This will be our vulnerability scanner that will be used to conduct scans against the virtual machine.  

## 4. Create a new virtual machine in VMware Player and install Windows 10 on it.  
Ensure you configure it for briged mode to allow it to be inside your local network for easy communication.

Afterwards test the connection by finding your ip address with the ipconfig command and pinging it from your host terminal.

## First Scan

With our Nessus account setup and our VM configured we run the initial basic scan non-credentialed scan on our target machine via its IP

![1st Scan](https://user-images.githubusercontent.com/82400181/230706510-62e18475-2072-4fe1-b5ff-7e88aa88c21c.png)

We found a medium risk.

![medium](https://user-images.githubusercontent.com/82400181/230706695-43b1f6d2-da6c-45d6-a522-7d7c99b7835a.png)
