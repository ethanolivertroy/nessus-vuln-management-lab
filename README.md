---
title: "Tenable Nessus Vulnerability Management Home Lab"
date: 2021-11-28
lastmod: 2022-12-17
tags: ["Vulnerability Management"]
categories: ["Cybersecurity"]
---

![featured](https://user-images.githubusercontent.com/63926014/221760051-9124d932-13fd-4741-80cf-2752634a97aa.png)



>> Tenable Nessus is probably the most common vulnerability scanner I've come across on security assessment engagements. Creating a lab is a great project idea because it allows you to test and evaluate the features of the vulnerability scanner in a controlled environment. This can be useful for learning how to use Nessus, testing configuration changes, and simulating scans against different types of devices and systems. By creating a lab, you can experiment with Nessus without the risk of affecting live production systems. Additionally, a Nessus lab can be a valuable resource for training and education, as it allows students and team members to practice using the tool and learn about different types of vulnerabilities and how to identify them.

# Create a Nessus Vulnerability Management Lab

> The value in this exercise is getting some hands-on working knowledge of the Nessus scanning tool especially if you’ve never used it before. Working as an security controls assessor as well as vulnerability scanning engineer it can be helpful to understand how Nessus works and outputs even if I’m not running scans everyday. Also there is entire career field and skill in vulnerability management for those interested. These orgs need it!

## Objective: Create a vulnerability management lab from an Ubuntu box

## 1. Preparation Steps

I just use an old Dell running Ubuntu for most of my cyber experimentation and labs but this lab can technically be done from any OS. If you are looking for instructions on how to do this on a PC this might be helpful: [https://youtu.be/lT6Px9zJM3s](https://youtu.be/lT6Px9zJM3s)

### Download VMWare Player, Windows 10 ISO, Nessus Essentials

1.  Download VMware Player [https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)
    
2.  Download Windows 10 ISO [https://www.microsoft.com/en-us/software-download/windows10ISO](https://www.microsoft.com/en-us/software-download/windows10ISO)
    ![](https://i.imgur.com/sJB2Fd9.png)
3.  Download Nessus Essentials [https://www.tenable.com/products/nessus/nessus-essentials](https://www.tenable.com/products/nessus/nessus-essentials)
    

Note: If you are running a Windows host machine you might have to pick a different executable but I’m running Ubuntu locally so opted for the Ubuntu 20.04 Nessus debian package

![](https://i.imgur.com/ODjeIqu.png)


### Install Virtualization Software - VMWare Player

4.  Got to make the .bundle file from VMWare executable
    
5.  Install VMware Player (run bundle script like this: **./my_shell_script**)
    

![](https://i.imgur.com/vEy3MGR.png)


![](https://i.imgur.com/AbfL9lD.png)


### Install Nessus Essentials

6.  Get Nessus running on Ubuntu

```bash
cd Downloads
sudo apt install ./Nessus*_amd64.deb

# How I activated Nessus this time I ran this lab
/bin/systemctl start nessusd.service

# The way I activated Nessus last time I ran this lab
sudo systemctl enable --now nessusd
sudo systemctl status nessusd
sudo ufw allow 8834
```

7.  Go to [https://localhost:8834/#/](https://localhost:8834/#/)

**Note:** When you originally download Nessus the website will prompt you to sign up for an activation code which would have been emailed to you. Here is where you will need that.

![](https://i.imgur.com/02zgoMK.png)


8.  Input activation code. Choose Nessus Essentials.

### Create Dummy Windows Virtual Machine

9.  Setup the Dummy Windows Virtual Machine using the Windows 10 ISO.

![](https://i.imgur.com/eGSU57m.png)


![](https://i.imgur.com/oUzH46B.png)


![](https://i.imgur.com/7JMMz26.png)


![](https://i.imgur.com/1aPxELM.png)


### Ensure Connectivity between Host Machine and VM

10.  Run `ipconfig` on Dummy Windows VM
    
11.  Ping that IP from your host machine (it will fail)
    
12.  We must lower the firewall from within the Windows VM so we can get connected for this lab.
    
13.  Ping again and it should go thru.
    

![](https://i.imgur.com/q7ahxyY.png)


### Might Need these steps?

I included these steps for a matter of record if needed but in my last two times running this lab they were not needed due to the pre-configuration of the Windows VM.

![](https://i.imgur.com/yd6XihE.png)


![](https://i.imgur.com/M9wsYPf.png)


## 2. Nessus Scanning

### Basic Scan

14.  Use the Dummy Windows VM IP address that we just pinged as the target for a basic Nessus Scan.

![](https://i.imgur.com/a6QrsLS.png)


![](https://i.imgur.com/QiE4EhI.png)


15.  Scan again…this time using a **credentialed scan**.

### Credentialed Scans

16.  Add the login credentials for your user into the credentialed information for the Nessus configuration

![](https://i.imgur.com/jdfCtQP.png)


17.  Enable Remote Registry from Windows Services

![](https://i.imgur.com/jRQT1J4.png)


![](https://i.imgur.com/1zUPR5D.png)


18.  Open Registry Editor and Add a DWORD

![](https://i.imgur.com/XSt6YcM.png)


![](https://i.imgur.com/3MHoejN.png)


19.  Create LocalAccountTokenFilterPolicy, Set Value to 1

![](https://i.imgur.com/dwsn4jT.png)


20.  Restart Windows. Scan Again!

![](https://i.imgur.com/CjLni04.png)


Obviously, we see a lot more trouble going now that Nessus has the credentials to poke into more areas.

### Installing Deprecated Software on Dummy Windows VM

**Why would we do this?** It’s great practice for actual real world vulnerability management. Systems using old software is a common issue. In this case, we are using old firefox. After scanning and seeing the issues, we can remediate it.

21.  Install old Firefox version from [https://ftp.mozilla.org/pub/firefox/releases/3.6.12/win32/en-US/](https://ftp.mozilla.org/pub/firefox/releases/3.6.12/win32/en-US/) on your Dummy Windows VM

![](https://i.imgur.com/wReaEvn.png)


22.  Scan again…see the fuckery!

![](https://i.imgur.com/iHhQCGR.png)


23.  From there I can work on remediating things such as removing the deprecated firefox, updating windows, updating chrome (or edge), and addressing other vulnerabilities as they come up in future scans.

![](https://i.imgur.com/wJ6XEzj.png)


## 3. Possible Remediation Steps

![](https://i.imgur.com/7ArZlZp.png)


![](https://i.imgur.com/AwMiIBg.png)


![](https://i.imgur.com/k2T428G.png)


![](https://i.imgur.com/Z2Rm0sD.png)


![](https://i.imgur.com/RXLl9bz.png)


We might have to even do some deeper research into some of the CVEs to figure out how to remediate

![](https://i.imgur.com/WmOdUnM.png)


![](https://i.imgur.com/bx3Lwv9.png)


![](https://i.imgur.com/evoH46S.png)


![](https://i.imgur.com/LGasoL8.png)


