---
title: LINUX Commands You need to Know 
description: >-
 A list of 60 most used linux commands for beginners
author: anorak
date: 2024-08-18 00:69:00 +0530
categories: [GUIDE,LINUX]
tags: [Linux, Cheatsheet,  Reference]
pin: true
---

## Introduction:

![mascot](/assets/img/202408/peng.jpg){: width="200" height="245" .w-50 .right}
For those new to Linux, mastering its commands is crucial for effective system management. Here's a succinct yet comprehensive cheatsheet encompassing the top 60 essential Linux commands, including their syntax. 
This guide provides a foundational overview, helping beginners quickly familiarize themselves with key operations and functions. From file manipulation to system monitoring, these commands are pivotal for navigating and controlling the Linux environment with precision and efficiency.


## 1. `ls`
- **Description:** Lists the contents of a directory, providing an overview of files and directories.
- **Syntax:** `ls [options] [directory]`
- **Example:** 
  ```bash
  ls -l /home
  ```

## 2. `cd`
- **Description:** Changes the current directory to a specified directory.
- **Syntax:** `cd [directory]`
- **Example:** 
  ```bash
  cd /var/log
  ```

## 3. `sudo`
- **Description:** Executes a command with elevated (superuser) privileges.
- **Syntax:** `sudo [command]`
- **Example:** 
  ```bash
  sudo apt-get update
  ```

## 4. `pwd`
- **Description:** Prints the current working directory.
- **Syntax:** `pwd`
- **Example:** 
  ```bash
  pwd
  ```

## 5. `mkdir`
- **Description:** Creates a new directory.
- **Syntax:** `mkdir [directory name]`
- **Example:** 
  ```bash
  mkdir my_folder
  ```

## 6. `rm`
- **Description:** Removes files or directories.
- **Syntax:** `rm [options] [file/directory]`
- **Example:** 
  ```bash
  rm -rf my_folder
  ```

## 7. `cp`
- **Description:** Copies files or directories from one location to another.
- **Syntax:** `cp [options] [source] [destination]`
- **Example:** 
  ```bash
  cp file.txt /backup/file.txt
  ```

## 8. `mv`
- **Description:** Moves or renames files or directories.
- **Syntax:** `mv [options] [source] [destination]`
- **Example:** 
  ```bash
  mv file.txt /backup/newfile.txt
  ```

## 9. `chmod`
- **Description:** Changes the file mode (permissions) of a file or directory.
- **Syntax:** `chmod [options] [permissions] [file/directory]`
- **Example:** 
  ```bash
  chmod 755 script.sh
  ```

## 10. `chown`
- **Description:** Changes the ownership of a file or directory.
- **Syntax:** `chown [options] [owner][:group] [file/directory]`
- **Example:** 
  ```bash
  chown root:root file.txt
  ```

## 11. `touch`
- **Description:** Creates an empty file or updates the timestamp of an existing file.
- **Syntax:** `touch [file name]`
- **Example:** 
  ```bash
  touch newfile.txt
  ```

## 12. `cat`
- **Description:** Concatenates and displays the content of files.
- **Syntax:** `cat [options] [file(s)]`
- **Example:** 
  ```bash
  cat file.txt
  ```

## 13. `echo`
- **Description:** Outputs the provided string to the terminal or redirects it to a file.
- **Syntax:** `echo [options] [string]`
- **Example:** 
  ```bash
  echo "Hello, World!" > hello.txt
  ```

## 14. `grep`
- **Description:** Searches for patterns within files using regular expressions.
- **Syntax:** `grep [options] [pattern] [file(s)]`
- **Example:** 
  ```bash
  grep "error" /var/log/syslog
  ```

## 15. `find`
- **Description:** Searches for files and directories within a directory hierarchy based on various criteria.
- **Syntax:** `find [path] [options] [expression]`
- **Example:** 
  ```bash
  find /home -name "*.txt"
  ```

## 16. `df`
- **Description:** Reports file system disk space usage.
- **Syntax:** `df [options] [file]`
- **Example:** 
  ```bash
  df -h
  ```

## 17. `du`
- **Description:** Estimates file space usage, showing the amount of disk space used by files and directories.
- **Syntax:** `du [options] [file]`
- **Example:** 
  ```bash
  du -sh /home
  ```

## 18. `top`
- **Description:** Displays dynamic real-time information about running processes, including CPU and memory usage.
- **Syntax:** `top`
- **Example:** 
  ```bash
  top
  ```

## 19. `ps`
- **Description:** Reports a snapshot of the current processes.
- **Syntax:** `ps [options]`
- **Example:** 
  ```bash
  ps aux
  ```

## 20. `kill`
- **Description:** Terminates a process using its PID (Process ID).
- **Syntax:** `kill [PID]`
- **Example:** 
  ```bash
  kill 1234
  ```

## 21. `ping`
- **Description:** Sends ICMP ECHO_REQUEST packets to network hosts to check connectivity.
- **Syntax:** `ping [options] [destination]`
- **Example:** 
  ```bash
  ping google.com
  ```

## 22. `ifconfig`
- **Description:** Configures or displays network interface parameters.
- **Syntax:** `ifconfig [interface] [options]`
- **Example:** 
  ```bash
  ifconfig eth0
  ```

## 23. `ip`
- **Description:** Manages network interfaces, routing tables, and IP addresses.
- **Syntax:** `ip [options] [object] [command]`
- **Example:** 
  ```bash
  ip addr show
  ```

## 24. `netstat`
- **Description:** Displays network connections, routing tables, interface statistics, and more.
- **Syntax:** `netstat [options]`
- **Example:** 
  ```bash
  netstat -tuln
  ```

## 25. `ss`
- **Description:** Displays detailed information about network socket connections.
- **Syntax:** `ss [options]`
- **Example:** 
  ```bash
  ss -tuln
  ```

## 26. `iptables`
- **Description:** Configures the IP packet filter rules of the Linux kernel firewall.
- **Syntax:** `iptables [options] [command]`
- **Example:** 
  ```bash
  iptables -L
  ```

## 27. `sudo`
- **Description:** Executes commands with elevated privileges.
- **Syntax:** `sudo [command]`
- **Example:** 
  ```bash
  sudo apt update
  ```

## 28. `apt`
- **Description:** Manages packages on Debian-based systems.
- **Syntax:** `apt [options] [command]`
- **Example:** 
  ```bash
  sudo apt install vim
  ```

## 29. `uname`
- **Description:** Displays system information, including the operating system, kernel version, and hardware details.
- **Syntax:** `uname [options]`
- **Example:** 
  ```bash
  uname -a
  ```

## 30. `htop`
- **Description:** An interactive process viewer similar to `top`, but with more features and a better user interface.
- **Syntax:** `htop`
- **Example:** 
  ```bash
  htop
  ```

## 31. `history`
- **Description:** Displays the command history of the current terminal session.
- **Syntax:** `history [options]`
- **Example:** 
  ```bash
  history
  ```

## 32. `shutdown`
- **Description:** Powers off or reboots the system.
- **Syntax:** `shutdown [options] [time]`
- **Example:** 
  ```bash
  sudo shutdown -h now
  ```

## 33. `reboot`
- **Description:** Reboots the system.
- **Syntax:** `reboot [options]`
- **Example:** 
  ```bash
  sudo reboot
  ```

## 34. `man`
- **Description:** Displays the manual page for a command, providing detailed usage information.
- **Syntax:** `man [command]`
- **Example:** 
  ```bash
  man ls
  ```

## 35. `whatis`
- **Description:** Provides a brief description of a command.
- **Syntax:** `whatis [command]`
- **Example:** 
  ```bash
  whatis grep
  ```

## 36. `curl`
- **Description:** Transfers data from or to a server using various protocols like HTTP, FTP, and more.
- **Syntax:** `curl [options] [URL]`
- **Example:** 
  ```bash
  curl https://example.com
  ```

## 37. `zip`
- **Description:** Compresses files into a zip archive.
- **Syntax:** `zip [options] [archive name] [file(s)]`
- **Example:** 
  ```bash
  zip archive.zip file1.txt file2.txt
  ```

## 38. `unzip`
- **Description:** Extracts files from a zip archive.
- **Syntax:** `unzip [options] [archive name]`
- **Example:** 
  ```bash
  unzip archive.zip
  ```

## 39. `less`
- **Description:** Views file content one page at a time, allowing backward

- **Description:** Views file content one page at a time, allowing backward and forward navigation.
- **Syntax:** `less [options] [file]`
- **Example:**
  ```bash
  less file.txt
  ```

## 40. `head`
- **Description:** Outputs the first part of files, by default the first 10 lines.
- **Syntax:** `head [options] [file]`
- **Example:**
  ```bash
  head -n 5 file.txt
  ```

## 41. `tail`
- **Description:** Outputs the last part of files, by default the last 10 lines.
- **Syntax:** `tail [options] [file]`
- **Example:**
  ```bash
  tail -n 5 file.txt
  ```

## 42. `diff`
- **Description:** Compares files line by line.
- **Syntax:** `diff [options] [file1] [file2]`
- **Example:**
  ```bash
  diff file1.txt file2.txt
  ```

## 43. `sort`
- **Description:** Sorts lines of text files.
- **Syntax:** `sort [options] [file]`
- **Example:**
  ```bash
  sort file.txt
  ```

## 44. `awk`
- **Description:** A powerful text-processing language used for pattern scanning and processing.
- **Syntax:** `awk 'pattern {action}' [file]`
- **Example:**
  ```bash
  awk '{print $1}' file.txt
  ```

## 45. `resolvectl`
- **Description:** Resolves domain names, queries DNS information, and changes DNS settings.
- **Syntax:** `resolvectl [options]`
- **Example:**
  ```bash
  resolvectl status
  ```

## 46. `cal`
- **Description:** Displays a calendar in the terminal.
- **Syntax:** `cal [options] [month] [year]`
- **Example:**
  ```bash
  cal 08 2024
  ```

## 47. `whoami`
- **Description:** Prints the current logged-in user name.
- **Syntax:** `whoami`
- **Example:**
  ```bash
  whoami
  ```

## 48. `finger`
- **Description:** Displays information about system users.
- **Syntax:** `finger [options] [user]`
- **Example:**
  ```bash
  finger root
  ```

## 49. `shred`
- **Description:** Securely deletes files by overwriting their data.
- **Syntax:** `shred [options] [file]`
- **Example:**
  ```bash
  shred -u file.txt
  ```

## 50. `ln`
- **Description:** Creates hard and symbolic links to files.
- **Syntax:** `ln [options] [target] [link name]`
- **Example:**
  ```bash
  ln -s /path/to/file linkname
  ```

## 51. `nano`
- **Description:** A simple and easy-to-use text editor for terminal-based editing.
- **Syntax:** `nano [file]`
- **Example:**
  ```bash
  nano file.txt
  ```

## 52. `vim`
- **Description:** A highly configurable text editor that is built to enable efficient text editing.
- **Syntax:** `vim [file]`
- **Example:**
  ```bash
  vim file.txt
  ```

## 53. `exit`
- **Description:** Exits the current shell session.
- **Syntax:** `exit [exit code]`
- **Example:**
  ```bash
  exit 0
  ```

## 54. `passwd`
- **Description:** Changes the user password.
- **Syntax:** `passwd [options] [username]`
- **Example:**
  ```bash
  passwd
  ```

## 55. `adduser`
- **Description:** Adds a new user to the system.
- **Syntax:** `adduser [username]`
- **Example:**
  ```bash
  sudo adduser newuser
  ```

## 56. `useradd`
- **Description:** A low-level command to add a new user.
- **Syntax:** `useradd [options] [username]`
- **Example:**
  ```bash
  sudo useradd -m newuser
  ```

## 57. `su`
- **Description:** Switches to another user account.
- **Syntax:** `su [options] [username]`
- **Example:**
  ```bash
  su - root
  ```

## 58. `clear`
- **Description:** Clears the terminal screen.
- **Syntax:** `clear`
- **Example:**
  ```bash
  clear
  ```

## 59. `ufw`
- **Description:** Uncomplicated Firewall (UFW) provides an easy interface for managing firewall rules.
- **Syntax:** `ufw [options] [command]`
- **Example:**
  ```bash
  sudo ufw enable
  ```

## 60. `neofetch`
- **Description:** Displays system information in a visually appealing format.
- **Syntax:** `neofetch [options]`
- **Example:**
  ```bash
  neofetch
  ```
