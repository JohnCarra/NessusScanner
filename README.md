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

With our Nessus account setup and our VM configured we run the initial non-credentialed basic scan on our target machine via its IP

Creation button is shown below just follow the basic scan setup  
![new scan](https://user-images.githubusercontent.com/82400181/230707337-d5690cb9-d261-4a6b-b9d0-d63d40be853a.png)

After that ensure to run and now we wait.  
![1st Scan](https://user-images.githubusercontent.com/82400181/230706510-62e18475-2072-4fe1-b5ff-7e88aa88c21c.png)

We found a medium risk and Nessus provides the explanation as pictured below.

![medium](https://user-images.githubusercontent.com/82400181/230706695-43b1f6d2-da6c-45d6-a522-7d7c99b7835a.png)

## Credentialed Scan

### 1. Enable remote registry

![remote](https://user-images.githubusercontent.com/82400181/230707002-c7c65448-8146-423a-a317-917f70491006.png)

ensure its in the automatic startup type and manually ensure its running as pictured.

### 2. Registry edit

![reg](https://user-images.githubusercontent.com/82400181/230707026-1c7951c8-4151-4ab5-8e58-6e928e9b6f06.png)
Inside the registry path we create a new DWORD titled 'LocalAccountFilterPolicy' and set the value to 1.


### Note
These steps help us follow Nessus recommend settings for our setup as it is not a domain.  
If you encounter any issues ensure printer sharing and discovery is turned on in the VM and temporarly disable User Account Control in the VM for the scan.

### 3. Credential setup

We go back to our original scan and folow the sequence to reach the menu needed as shown below.

![cred](https://user-images.githubusercontent.com/82400181/230707263-328bcc1b-d9ca-4a9e-88ae-efb34dda8077.png)  

Simply enter your VM account credentials. 

![cred2](https://user-images.githubusercontent.com/82400181/230707264-885ecba8-6e02-4ab2-b73b-1d6811f0a08d.png)  

### 4. Scan Results
We can see many more vulenrabilities with this type of scan.  

![scans](https://user-images.githubusercontent.com/82400181/230707459-97dd1dfa-c63d-412a-93c5-8efc3bbe7b6e.png)


#### Example missing updates

We can see below that our VM is unpatched which is a huge concern
This really helps us see the many different things we can find when running a vulnerability scan such as this.

![dotnet](https://user-images.githubusercontent.com/82400181/230707520-29638cb3-acd3-4bb9-aefb-853779cecda5.png)

