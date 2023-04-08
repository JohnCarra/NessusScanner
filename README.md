# Nessus Scanner
This repo focuses on a hands on lab with Nessus and covers several features. This is a summary of running the scan on a Windows 10 Virtual Machine as the target machine. I will walk you through this lab and show you a basic process to eliminate some commonly known vulnernabilites.


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

Creation button is shown below
![new scan](https://user-images.githubusercontent.com/82400181/230707337-d5690cb9-d261-4a6b-b9d0-d63d40be853a.png)

Select basic scan and enter the targets IP address

![basic scan](https://user-images.githubusercontent.com/82400181/230742721-d68f0c4d-ea51-4c19-97de-6fa400eae965.png)


After that ensure to run and now we wait.  
![1st Scan](https://user-images.githubusercontent.com/82400181/230706510-62e18475-2072-4fe1-b5ff-7e88aa88c21c.png)

We found a medium risk and Nessus provides the explanation as pictured below.  

![medium](https://user-images.githubusercontent.com/82400181/230706695-43b1f6d2-da6c-45d6-a522-7d7c99b7835a.png)


The scope is a bit narrow here without our credentials so lets provide them and see what happens  

## Credentialed Scan

Let's setup a credentialed network scan but first we have a few steps to take.

### 1. Enable remote registry

![remote](https://user-images.githubusercontent.com/82400181/230707002-c7c65448-8146-423a-a317-917f70491006.png)

ensure its in the automatic startup type and manually ensure its running as pictured.

### 2. Registry edit

![reg](https://user-images.githubusercontent.com/82400181/230707026-1c7951c8-4151-4ab5-8e58-6e928e9b6f06.png)
Inside the registry path we create a new DWORD titled 'LocalAccountFilterPolicy' and set the value to 1.


### Note
These steps help us follow Nessus recommend settings for our setup as it is not in a domain.
If you encounter any issues ensure printer sharing and discovery is turned on in the VM and temporarly disable User Account Control in the VM for the scan.

### 3. Credential setup

We go back to our original scan and follow the sequence to reach the menu needed as shown below.

![cred](https://user-images.githubusercontent.com/82400181/230707263-328bcc1b-d9ca-4a9e-88ae-efb34dda8077.png)  

Simply enter your VM account credentials. 

![cred2](https://user-images.githubusercontent.com/82400181/230707264-885ecba8-6e02-4ab2-b73b-1d6811f0a08d.png)  

### 4. Scan Results
We can see many more vulenrabilities with this type of scan, including criticals and highs.

![scans](https://user-images.githubusercontent.com/82400181/230707459-97dd1dfa-c63d-412a-93c5-8efc3bbe7b6e.png)


#### Example missing updates

We can see below that our VM is unpatched which is a huge concern
This really helps us see the many different things we can find when running a vulnerability scan such as this.

![dotnet](https://user-images.githubusercontent.com/82400181/230707520-29638cb3-acd3-4bb9-aefb-853779cecda5.png)


#### Critical 

Here we can see that a critical issue was found during the scan 

![critical](https://user-images.githubusercontent.com/82400181/230742480-21857b54-85f9-483c-9acd-3eceacbf8e1c.png)

We can remediate this with the suggested solution. 

#### Remediations

Nessus recommends ways to remediate these issues and most of it here pertains to updating our system.

![remediation](https://user-images.githubusercontent.com/82400181/230707776-374d6c96-4ad2-4df2-a6bc-a44c9de79b33.png)

A possible long term solution here is to ensure we have a continous delivery and installation of all patches including third party software.


### New Scan post Remediation

While better it appears we still have a critical issue and a few highs lets investigate.

![Untitled](https://user-images.githubusercontent.com/82400181/230743118-3a9c117a-7f7e-46b8-bddf-bb9d884a500a.png)

It appears that the issue here stems from Internet Explorer

![IE](https://user-images.githubusercontent.com/82400181/230743143-fd5d4105-3f4a-479f-872f-9ba55554e082.png)

However in this situation we can no longer update this application as per Microsoft 

"As of June 15, 2022,the IE11 desktop application is no longer supported"

and they further state 

"The Internet Explorer 11 (IE11) desktop application will be permanently disabled through a Microsoft Edge update on certain versions of Windows 10 on February 14, 2023."

SOURCE: https://learn.microsoft.com/en-us/lifecycle/faq/internet-explorer-microsoft-edge  

Since this is at the time of writing April, 2023, it would be good to simply uninstall this application. 

Another thing we can do is update our other individual applications installed onto this system.   

So lets remediate this and run another scan.

### Important note
We have to do more than simply uninstall the app to make it disappear from the plugin

Some of the underlying components are still around it seems but to achieve this we will follow the steps given by Microsofts documentation

So lets open up the group policy, just type 'edit group policy' into the desktop search bar.

![GPDis](https://user-images.githubusercontent.com/82400181/230744069-8c511fdc-19c7-465c-8262-c35ff6ce612a.png)

Then we simply select disable IE explorer and enable it.

![gp2](https://user-images.githubusercontent.com/82400181/230744079-66f67b22-6000-44d3-970d-f9cdb1569ba2.png)

SOURCE: https://learn.microsoft.com/en-us/deployedge/edge-ie-disable-ie11

#### Additional Note

We also want to make sure we open up the Microsoft store and update those apps as well.

### Final Scan

Now we run our final credentialed scan on our target system.

![final](https://user-images.githubusercontent.com/82400181/230744308-38caa0b1-147e-4911-ba4e-ebab63cf11a6.png)

It appears we still have one high issue but we are doing far better.


### Conclusion  

This demo will conclude here though.  

Now its up to you to find what the issue is and use our process to remediate this.





