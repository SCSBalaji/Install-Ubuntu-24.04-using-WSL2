# 🖥️ Install Ubuntu 24.04 with GUI in Windows 11 using WSL2 🎨

Welcome to the guide! In this repository, you’ll learn how to install Ubuntu 24.04 with a graphical user interface (GUI) on Windows 11 using WSL2 (Windows Subsystem for Linux). WSL2 allows you to run a full-fledged Ubuntu environment on your Windows machine without the need for dual-booting or using a virtual machine, making it an incredibly efficient way to work with Linux.



## 🚀 Why Ubuntu 24.04 with GUI on WSL2?

WSL2 is a lightweight, fast, and convenient way to run Linux distributions on your Windows 11 machine. By using Ubuntu 24.04 with a GUI on WSL2, you unlock the power of Linux while maintaining the familiarity of Windows. Here are some of the advantages:

- Performance: WSL2 uses a real Linux kernel, offering near-native Linux performance.

- Seamless Integration: Access Linux files from Windows and vice versa.

- No dual booting: Run both Windows and Linux side by side.

- Great for developers: Supports the same Linux tools, command-line utilities, and applications.

- High performance: with the latest WSL2 architecture.

- Access to Linux GUI apps alongside your favorite Windows apps.



## 🔧 Prerequisites

Before getting started, make sure your system meets the following requirements:

- Windows 11 installed on your PC

- WSL2 enabled on your system

- Windows Subsystem for Linux Update is up-to-date

- Virtual Machine Platform feature enabled in Windows.

If you don't have WSL2 enabled yet, follow the setup instructions provided below.



## 📝 Step-by-Step Installation Guide

### 1️⃣ Step 1: Enable WSL on Windows 11

1. Open PowerShell as Administrator and run the following command to enable WSL:
```bash
wsl --install
```
2. Restart your system to apply the changes.

### 2️⃣ Step 2: Set WSL2 as the Default Version

To ensure that future installations use WSL2 (which has improved performance and full system call compatibility), set WSL2 as the default version by running the following command in PowerShell:

```bash
wsl --set-default-version 2
```

### 3️⃣ Step 3: Verify WSL Version

Once WSL is installed and the default version is set, verify the WSL version by running the following command:

```bash
wsl --version
```
![Reference](https://github.com/SCSBalaji/Install-Ubuntu-24.04-using-WSL2/blob/1dbfe5aa6c65e9d024afc23d5080c70fb2bbf886/Reference%20Images/wsl%20version.png)

Ensure that Version 2 is shown as the default.


### 4️⃣ Step 4: (Method - 1) Install Ubuntu 24.04 via Microsoft Store

1. Open the Microsoft Store and search for Ubuntu 24.04.
![Reference](https://github.com/SCSBalaji/Install-Ubuntu-24.04-using-WSL2/blob/a6543f2cf94a850a0f58bb3f388f4e41407a093d/Reference%20Images/microsoft%20store%20ubuntu.png)

3. Click on Get and then Install.

4. After installation, launch Ubuntu 24.04 from the Start menu.

4. Follow the on-screen setup, including creating a username and password.

### 5️⃣ Step 5: (Method 2) Install Ubuntu 24.04 using Command Line

If you prefer using the command line to install Ubuntu, follow these steps:

1. Open PowerShell as Administrator.

2. Run the following command to list the available distributions:

```bash
wsl --list --online
```
![Reference](https://github.com/SCSBalaji/Install-Ubuntu-24.04-using-WSL2/blob/a6543f2cf94a850a0f58bb3f388f4e41407a093d/Reference%20Images/wsl%20list%20online.png)

3. Install Ubuntu 24.04 using this command:
```bash
wsl --install -d Ubuntu-24.04
```

4. Follow the setup instructions after installation.


### 6️⃣ Step 6: Update and Upgrade Ubuntu Packages

After installing Ubuntu, it’s important to update the package lists and upgrade the system to ensure you have the latest software and security patches.

1. Update and Upgrade Packages:
```bash
sudo apt update && sudo apt upgrade -y
```

2. Full System Upgrade (to upgrade all packages including kernel and essential system packages):
```bash
sudo apt update && sudo apt full-upgrade -y
```

### 7️⃣ Step 7: Install a GUI and XRDP for Ubuntu 24.04

To install a graphical desktop environment and access it via Remote Desktop, follow these steps:

1. Install XFCE Desktop and XFCE goodies (a lightweight and fast desktop environment):

```bash
sudo apt install xfce4 xfce4-goodies -y
```

2. Install XRDP (Remote Desktop Protocol server) to enable remote desktop access:

```bash
sudo apt install xrdp -y
```

3. Backup the XRDP configuration file:

```bash
sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak
```

4. Modify XRDP port number to avoid conflicts with the default port:

```bash
sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
```

5. Increase XRDP color depth for better display quality:

```bash
sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini
```

```bash
sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
```

6. Start the XRDP service to allow remote connections:

```bash
sudo /etc/init.d/xrdp start
```


### 8️⃣ Step 8: Configure XRDP to Use XFCE4 Desktop

To ensure that XRDP starts the XFCE desktop environment correctly when you connect via Remote Desktop, you need to modify the XRDP startup script:

1. Open the startwm.sh file for editing:

```bash
sudo nano /etc/xrdp/startwm.sh
```

2. Comment out the last two existing lines: In the file, find the following lines at the bottom:

```bash
test -x /etc/X11/Xsession && exec /etc/X11/Xsession
exec /bin/sh /etc/X11/Xsession
```

Comment these lines by adding # at the beginning:

```bash
#test -x /etc/X11/Xsession && exec /etc/X11/Xsession
#exec /bin/sh /etc/X11/Xsession
```


3. Add the command to start XFCE4: Below the commented lines, add the following command:

```bash
startxfce4
```
![Reference](https://github.com/SCSBalaji/Install-Ubuntu-24.04-using-WSL2/blob/a6543f2cf94a850a0f58bb3f388f4e41407a093d/Reference%20Images/stratxfce4.png)

4. Save and exit the Nano editor:

- Press Ctrl+S to save the changes.
- Press Ctrl+X to exit the editor.


5. Restart the XRDP service to apply the changes:

```bash
sudo /etc/init.d/xrdp start
```

This step ensures that when you log in via Remote Desktop, the XFCE desktop environment will be launched, giving you access to the graphical interface of Ubuntu 24.04.

❗If you run into an issue where the RDP session disconnects right away after attempting to connect, you can try the following tweaks to ```startwm.sh```:

The following tweak from the link below wasn't 100% related but the same tweak to ```/etc/xrdp/startwm.sh``` solved the issue for me and now I can RDP successfully to my WSL Ubuntu 24.04 instance:

Link to post: command line - Can not open terminal in XFCE GUI under Windows 11 WSL Ubuntu 24.04 - Ask Ubuntu]    
https://askubuntu.com/questions/1537834/can-not-open-terminal-in-xfce-gui-under-windows-11-wsl-ubuntu-24-04

The actual tweak:   

Edit the startwm.sh file   
sudo nano /etc/xrdp/startwm.sh    
```
# test -x /etc/X11/Xsession && exec /etc/X11/Xsession
# exec /bin/sh /etc/X11/Xsession
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
startxfce4
```
Restart xrdp and then RDP should hopefully work as expected.   

Try an RDP connection again and hopefully it now works.   

### 9️⃣ Step 9: Launch the Ubuntu GUI via Remote Desktop

1. Open Remote Desktop Connection on your Windows machine (search for "Remote Desktop" in the Start menu).

2. Enter ` localhost:3390 ` as the remote address (since we changed the port to 3390).
![Reference](https://github.com/SCSBalaji/Install-Ubuntu-24.04-using-WSL2/blob/a6543f2cf94a850a0f58bb3f388f4e41407a093d/Reference%20Images/localhost3390.png)

3. Login using the username and password you set during the Ubuntu setup process.

4. You should now see the full XFCE desktop environment, allowing you to interact with a GUI on your Ubuntu 24.04 system.
![Reference](https://github.com/SCSBalaji/Install-Ubuntu-24.04-using-WSL2/blob/a6543f2cf94a850a0f58bb3f388f4e41407a093d/Reference%20Images/gui%20interface.png)

### 🔄 Step 10: Verify Installation and Enjoy

After logging in, you should see the full Ubuntu desktop interface. You can now run graphical Linux applications (e.g., GIMP, Visual Studio Code, etc.) alongside your Windows apps.



## ⏰ How to Start Ubuntu 24.04 with GUI After Shutdown or Restart

### 💻 Step 1: (Method - 1) Open Ubuntu 24.04 via WSL

1. Open PowerShell or Command Prompt on your Windows 11 machine.

2. Start WSL and launch Ubuntu 24.04 by running:
```bash
wsl
```
3. If Ubuntu does not start automatically, you can specify the distribution with:
```bash
wsl -d Ubuntu-24.04
```


### 🖥️ Step 2: (Method - 2) Open Ubuntu 24.04 App directly from the Start Menu

If you prefer using the Ubuntu app directly, follow this method:

1. Open Ubuntu 24.04 from Start Menu:

- Click on the Start Menu and scroll down to All Apps.

- Find and click on the Ubuntu 24.04 logo.

2. When the Ubuntu app opens, it will take you directly to the Ubuntu shell.

3. Open Remote Desktop Connection:

- On your Windows 11 machine, open the Remote Desktop Connection app (search in Start menu).

- Enter `localhost:3390` as the computer address and click Connect.

4. If the above step gives any Error Message

Start the XRDP Service:

- In the Ubuntu terminal, start the XRDP service by running:
```bash
sudo /etc/init.d/xrdp start
```
- Then repeat the point 3.

5. Login to XFCE Desktop:

- Enter your Ubuntu username and password to access the XFCE desktop environment.


## 🎥 Demo

https://youtu.be/p3yMG30QAqM?si=lP3MSDA4AQNhyCOt


## 🛠️ Troubleshooting

### ❗ Issue: Ubuntu GUI Not Displaying

1. Ensure the XRDP service is running:

```bash
sudo /etc/init.d/xrdp status
```

2. Ensure the Remote Desktop Connection is set to the correct port (localhost:3390).

3. Restart WSL:
```bash
wsl --shutdown
wsl
```
## 📚 Useful Resources

 - [Official WSL Documentation](https://learn.microsoft.com/en-us/windows/wsl/)
 - [Ubuntu Official Documentation](https://help.ubuntu.com/)
 - [XRDP](https://github.com/neutrinolabs/xrdp)
 - [XFCE Desktop Environment](https://xfce.org/)
 - [XFCE Documentation](https://docs.xfce.org/)
 - [WSL2 Overview](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)
 - [VS Code Remote WSL Extension](https://code.visualstudio.com/docs/remote/wsl)

## 💡 Additional Tips & Tricks
- Running Linux GUI Apps: You can directly run GUI apps without launching the full desktop. For example, run Gedit using:

    ```bash
    sudo apt install gedit
    ```
    ```bash
    gedit
    ```

- File Sharing: Access Windows files from Ubuntu at ` /mnt/c/Users/<your-username> ` and vice versa.

- Customize Your Desktop: Use themes and tweak XFCE settings to personalize your Linux desktop experience.


## 👨‍💻 Author

- [@SCSBalaji](https://www.github.com/SCSBalaji)


## 🤝 Contributing

Feel free to contribute to this repository. If you have suggestions, find bugs, or want to add new features, please open an issue or submit a pull request!

Contributions are always welcome!


## ⚖️ License
This project is licensed under the [MIT](https://choosealicense.com/licenses/mit/) License - see the LICENSE file for more details.


## 💬 Support
For support, join our [Telegram](https://t.me/github_discussion_group) Discussion Group.


## ❓ Frequently Asked Questions (FAQs)

#### 1. What is WSL2?

WSL2 (Windows Subsystem for Linux version 2) is a compatibility layer that allows you to run a Linux distribution (like Ubuntu) on Windows without the need for a virtual machine or dual-booting. It offers full system call compatibility, faster performance, and the ability to run Linux applications directly within Windows.

Learn more: [WSL2 Overview](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)


------------------------------------------------------------
#### 2. Why should I use WSL2 instead of a virtual machine (VM)?


WSL2 is lighter and faster than a VM, offering seamless integration with Windows. You can access Linux apps, services, and files directly from Windows, use fewer system resources, and avoid the need for a separate hypervisor or virtual machine software.


------------------------------------------------------------
#### 3. Can I use any Linux distribution with WSL2?

Yes, many Linux distributions are available for installation via WSL2, including Ubuntu, Debian, Fedora, Kali Linux, and more. In this guide, we focus on Ubuntu 24.04.


------------------------------------------------------------
#### 4. How do I check if WSL is installed on my system?

You can check whether WSL is installed by opening PowerShell or Command Prompt and running:

```bash
wsl --list --verbose
```
![Reference](https://github.com/SCSBalaji/Install-Ubuntu-24.04-using-WSL2/blob/a6543f2cf94a850a0f58bb3f388f4e41407a093d/Reference%20Images/wsl%20list%20verbose.png)

If no distributions are listed, follow the steps in this guide to install WSL.

------------------------------------------------------------
#### 5. What is the difference between WSL1 and WSL2?

- WSL1: Uses a translation layer to map Linux kernel calls to Windows.

-WSL2: Includes a full Linux kernel, providing better performance and compatibility, especially for system-level tasks.

WSL2 is the recommended version for running GUI applications and heavier tasks.

------------------------------------------------------------
#### 6. How do I verify that WSL2 is my default version?

You can verify the default version of WSL by running the following command:

```bash
wsl --version
```
If it says version 2, you’re using WSL2. If not, use:

```bash
wsl --set-default-version 2
```
to set WSL2 as the default.

------------------------------------------------------------
#### 7. What is XFCE, and why use it for a GUI?

XFCE is a lightweight, fast, and stable desktop environment for Linux. It’s perfect for WSL2 because it uses fewer system resources, making it ideal for running Linux with GUI on Windows.

Learn more: [XFCE Documentation](https://docs.xfce.org/)


------------------------------------------------------------
#### 8. Why do I need XRDP to run Ubuntu with a GUI?

XRDP is a remote desktop server that allows you to connect to your Ubuntu system with a graphical interface. It enables you to use the Remote Desktop Connection tool on Windows to access Ubuntu's desktop environment via WSL2.


------------------------------------------------------------
#### 9. How can I access Ubuntu’s GUI after setting everything up?

To access Ubuntu's GUI, follow these steps:

1. Start WSL: Open Ubuntu from PowerShell, Command Prompt, or the Start menu.

2. Start the XRDP service:
```bash
sudo /etc/init.d/xrdp start
```

3. Open Remote Desktop Connection in Windows and connect using localhost:3390.

------------------------------------------------------------
#### 10. How do I change the default port for XRDP?
In this guide, we change the default XRDP port from 3389 to 3390 to avoid conflicts with other services. You can modify the port in the XRDP configuration file (/etc/xrdp/xrdp.ini) as shown in Step 7 of this guide.


------------------------------------------------------------
#### 11. What should I do if I cannot connect to Ubuntu via Remote Desktop?

If you have trouble connecting:

- Ensure that XRDP is running:
    ```bash
    sudo /etc/init.d/xrdp start
    ```
- Make sure you are using the correct port (3390).

- Restart the XRDP service or reboot your Windows machine and try again.

------------------------------------------------------------

#### 12. How do I update Ubuntu packages?

Run the following commands to update and upgrade your system:

```bash
sudo apt update && sudo apt upgrade -y
```
For a more comprehensive upgrade, use:

```bash
sudo apt full-upgrade -y
```

------------------------------------------------------------
#### 13. Can I install other desktop environments besides XFCE?

Yes! You can install other desktop environments like GNOME, KDE, or LXDE. However, XFCE is lightweight and optimized for WSL2, making it a better choice for most users.

------------------------------------------------------------

#### 14. How do I stop the XRDP service?

To stop the XRDP service, run:

```bash
sudo /etc/init.d/xrdp stop
```
This will stop the service and prevent Remote Desktop access until you start it again.

------------------------------------------------------------

#### 15. How do I change the default Ubuntu distribution in WSL2?

If you have multiple distributions installed, you can set a default one using:

```bash
wsl --set-default Ubuntu-24.04
```

------------------------------------------------------------

#### 16. How do I remove Ubuntu from WSL2?


If you want to uninstall Ubuntu from WSL2, run the following in PowerShell:

```bash
wsl --unregister Ubuntu-24.04
```
This will remove all files and configurations for that distribution.


------------------------------------------------------------
#### 17. Can I use VS Code or other development tools with Ubuntu WSL2?

Yes! Visual Studio Code (VS Code) has excellent integration with WSL. You can install the Remote - WSL extension to run, edit, and debug code directly in your Ubuntu environment.

Learn more: [VS Code Remote WSL Extension](https://code.visualstudio.com/docs/remote/wsl)

------------------------------------------------------------
#### 18. Do I need to reconfigure the GUI setup every time I start WSL?

No, you only need to start the XRDP service using:

```bash
sudo /etc/init.d/xrdp start
```
After that, you can connect via Remote Desktop without reconfiguring.

------------------------------------------------------------
