---
description: Cisco IOS is the operating system used on Cisco Devices.
icon: rectangle-terminal
---

# Cisco IOS CLI

## Command-line Interface

* CLI = command-line interface.
* It's the interface you use to configure Cisco Devices.
* To connect to a device's console you use a USB mini-b cable or a cable with a RJ45 end and a serial port end. This cable is called a '**rollover cable**' and have a specific pin connection. However, most laptops nowadays don't have a serial port, so we use an adapter.&#x20;

<figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

## Terminal Emulator (PuTTy)

* The PuTTy is used to access the device's CLI.
* User EXEC mode is indicated by the '**>**' sign next to the host name of the device. It is very limited, the users can look at some things, but cannot make any changes to the configuration.
* Also called 'user mode'.
* The **enable** command in user EXEC mode places you in privileged exec mode. It is indicated by the '**#**' sign. It provides complete access to view the device's configuration, restart the device, etc. Cannot change the configuration, but can change the time on the device, save the configuration file etc.
* Use a question mark '**?**' to view the available commands.

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption><p>#</p></figcaption></figure>

### Global Configuration Mode

* The command to enter configuration mode is:

<pre class="language-bash"><code class="lang-bash"><strong>Router>enable
</strong>Router#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#
</code></pre>

* Password configuration:
  * Passwords **are** case-sensitive.

```bash
Router(config)#enable password CCNA
```

### Configuration files and password

* There are two separate configuration files kept on the device at once.
  * **running-config** = the current, active configuration file on the device. As you enter commands in the CLI, you edit the active configuration.
  * **startup-config** = the configuration file that will be loaded upon restart of the device.

```bash
Router#show running-config
```

```bash
Router#show startup-config
```

There are three ways you can save the running-configuration, to make it the startup-configuration. All those three commands are executed from privileged exec mode.

```bash
Router#write
Router#write memory
Router#copy running-config startup-config
```

Because the password is plain text saved in the configuration files,  we need to level up the security a bit. You can do that by:

```bash
Router>enable
Router#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#service password-encryption 
```

This will encrypt all passwords in a jumble of numbers and letters, so that they cannot be easily read.

```bash
Router#show running-config
Building configuration...

Current configuration : 719 bytes
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname Router
!
!
!
enable password 7 08026F6028
!
```

* The number 7 indicates the type of encryption used to encrypt the password, and in this case it is using the **Cisco's proprietary encryption algorithm, from the service password-encryption.**
* However, this is not very secure, cause there are crackers through the internet, that can break it in seconds. The good news, though, is that there is a more secure enable password that can be used on Cisco devices, with a tougher type of encryption.
* The more secure method is to use the enable secret command, instead of the enable password command. You can then view the running configuration once more. However, since I was still in global configuration mode, I typed '**do**' in front of the command, which is equivalent to the '**sudo**' command from the UNIX-like operating systems.

```bash
Router(config)#enable secret Cisco
Router(config)#do sh run
Building configuration...

Current configuration : 766 bytes
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname Router
!
!
!
enable secret 5 $1$mERr$YlCkLMcTYWwkF1Ccndtll.
enable password 7 08026F6028
```

* The 5 means MD5 encryption, which means its a hashing algorithm. It can still be cracked, no password is invicible, but its much better.

### Canceling/Deleting commands

* This is done by typing '**no**' in front of the command, which will disable the specified command.

```bash
Router(config)#no service password-encryption
Router(config)#do sh run
Building configuration...

Current configuration : 769 bytes
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Router
!
!
!
enable secret 5 $1$mERr$YlCkLMcTYWwkF1Ccndtll.
enable password 7 08026F6028
```

* Even though the command disables encryption, any passwords already configured, such as the **enable password** or **enable secret**, will remain in their encrypted or hashed state. New passwords added after the command will also not be automatically encrypted. However, passwords stored with **enable password 7** or **enable secret 5** will remain encrypted and cannot be decrypted without specific tools, even though password encryption has been disabled.
* If you enable **service password-encryption:**
  * current passwords **will be** encrypted
  * future passwords **will be** encrypted
  * the enable secret **will not be** affected
* If you disable **service password-encryption**:
  * current passwords **will not be** decrypted
  * future passwords **will not be** encrypted
  * the enable secret **will not be** affected
