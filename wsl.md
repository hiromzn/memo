# How to install
- https://learn.microsoft.com/ja-jp/windows/wsl/install
- - PowerShell または Windows コマンド プロンプトを管理者モードで開
```
> wsl --install

C:\Users\s-hiromichi.mizuno>wsl -l -o
The following is a list of valid distributions that can be installed.
The default distribution is denoted by '*'.
Install using 'wsl --install -d <Distro>'.

  NAME                                   FRIENDLY NAME
* Ubuntu                                 Ubuntu
  Debian                                 Debian GNU/Linux
  kali-linux                             Kali Linux Rolling
  Ubuntu-18.04                           Ubuntu 18.04 LTS
  Ubuntu-20.04                           Ubuntu 20.04 LTS
  Ubuntu-22.04                           Ubuntu 22.04 LTS
  OracleLinux_7_9                        Oracle Linux 7.9
  OracleLinux_8_7                        Oracle Linux 8.7
  OracleLinux_9_1                        Oracle Linux 9.1
  openSUSE-Leap-15.5                     openSUSE Leap 15.5
  SUSE-Linux-Enterprise-Server-15-SP4    SUSE Linux Enterprise Server 15 SP4
  SUSE-Linux-Enterprise-15-SP5           SUSE Linux Enterprise 15 SP5
  openSUSE-Tumbleweed                    openSUSE Tumbleweed
  
> wsl --install -d Ubuntu

Installing: Windows Subsystem for Linux
Windows Subsystem for Linux has been installed.
Installing: Ubuntu
Ubuntu has been installed.
The requested operation is successful. Changes will not be effective until the system is rebooted.
```

```
C:\Users\s-hiromichi.mizuno>wsl --install -d Ubuntu-22.04
Installing: Ubuntu 22.04 LTS
Ubuntu 22.04 LTS has been installed.
Launching Ubuntu 22.04 LTS...
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username: hmizuno
New password:
Retype new password:
passwd: password updated successfully
Installation successful!
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.90.1-microsoft-standard-WSL2 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


This message is shown once a day. To disable it please create the
/home/hmizuno/.hushlogin file.
hmizuno@JP-PF4G49WD:~$ uname -a
Linux JP-PF4G49WD 5.15.90.1-microsoft-standard-WSL2 #1 SMP Fri Jan 27 02:56:13 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
hmizuno@JP-PF4G49WD:~$

C:\Users\s-hiromichi.mizuno>wsl -l -v
  NAME            STATE           VERSION
* Ubuntu          Running         2
  Ubuntu-22.04    Stopped         2

C:\Users\s-hiromichi.mizuno>wsl
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

hmizuno@JP-PF4G49WD:/mnt/c/Users/s-hiromichi.mizuno$ uname -a
Linux JP-PF4G49WD 5.15.90.1-microsoft-standard-WSL2 #1 SMP Fri Jan 27 02:56:13 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux

hmizuno@JP-PF4G49WD:/mnt/c/Users/s-hiromichi.mizuno$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.2 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
```
