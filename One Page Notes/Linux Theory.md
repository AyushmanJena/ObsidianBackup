
use #commands for topics more command based than theory
# Introduction : 
Linux is based on the UNIX operating system, a multi-user and multitasking platform. 
Free and open source

#### Applications of Linux : 
- **Servers and Hosting :** Powers web servers, cloud platforms and data centers with high stability and security.
- **Development** : Provides useful tools and environments for coding, testing and debugging applications.
- **Desktop and Personal Use** : Offers secure and customizable desktop environments for everyday computing tasks.
- **Cybersecurity** : Widely used for ethical hacking, penetration testing and security analysis
- **Embedded Systems** : Runs efficiently on IoT devices, routers and other low resource systems.
- **Education** : Helps students to learn programming, networking and system administration at low cost.


# Linux Distributions 
A Linux Distribution or Distro is an Operating System Built on the Linux Kernel, bundled with essential software, tools and package managers customized to server different users.
Ex : Ubuntu, Debian, Linux Mint, Fedora, Arch Linux, etc.

## Architecture of Linux
The main components of Linux operating system are:
- Application
- Shell
- Kernel
- Hardware
- Utilities

Each layer communicates with the one below it, creating a structured and efficient operating system design

# Kernel
Kernel is the core component of linux os, that sits between the hardware and user space, managing system resources and ensuring smooth communication between software and hardware. 
It controls how processes are executed, scheduled and isolated to maintain system stability and security.

#### The kernel is responsible for : 
- Memory management - Allocates and manages system memory efficiently
- Process management - Schedules processes and controls execution using queues
- Resource allocation - Distributes CPU, memory, and I/O resources among processes
- Device management - Controls hardware devices through device drivers
- Application interaction - Acts as a bridge between applications and hardware
- Security - Enforces access control and system-level security mechanisms

#### Types of Kernel : 
1. **Monolothic Kernel** - All system services run in kernel space, sharing the same memory
2. **Microkernel** - Only essential services like process scheduling and memory management run in kernel space, others execute in user space
3. **Exokernel** - exposes hardware resources directly to applications, allowing them to manage resources at a low level
4. **Hybrid Kernel** - - Combines features of monolithic and microkernel architectures, keeping critical services in kernel space while supporting modular components

#### Main Subsystems of Kernel : 
Process Scheduler : Responsible for fairly distributing processing time among all concurrently running processes.
Memory Management Unit (MMU) : Responsible for proper distribution of memory resources among concurrently running process
Virtual File System (VFS) : Provides interface to access stored data across different file systems and different physical media
Networking Subsystem : Responsible for handling all network communication, including data transmission, routing, and network protocols.
Inter Process Communication Unit (IPC) : Enables communication and synchronization between multiple running processes within the system


### System Libraries : 
System Libraries provide predefined functions that allow application programs and system utilities to access kernel features without interacting with the kernel directly.
Common System libraries : 
- GNU C Library (glibc) : Core system calls and built in functions for C programming
- libpthread (POSIX Threads) : for multithreaded applications
- libm (Math Library) : Mathematical functions
- libcrypt : Cryptographic functions

# Shell 
The shell is a software interface to the kernel. 
Takes commands from the user, interprets them and transmits to the kernel.
Then Kernel performs the requested operations.

Types of Shell : 
![[assets/shell_in_os.webp]]


1. **Bourne Shell (sh)** – Original Unix shell; lightweight and mainly used for basic scripting and system scripts.
2. **C Shell (csh)** – C-like syntax with command history; mainly used for interactive work.
3. **Korn Shell (ksh)** – Combines `sh` features with advanced scripting; common in enterprise systems.
4. **Bash** – Enhanced `sh` with history, tab completion, and powerful scripting; widely used on Linux.
5. **Z Shell (zsh)** – Highly customizable shell with advanced completion, themes, and plugins.
6. **Fish** – User-friendly shell with syntax highlighting, suggestions, and easy configuration.


#### System Utilities 
Command line tools that help users and administrators manage, configure and monitor the linux system.
- Perform file and directory management operations
- Monitor system performance and resource usage
- Manage users, groups and permissions

# Basic Linux Commands

#commands
Directly interacting with the operating system through terminal and perform tasks like file management, navigation and system monitoring

#### 1. `ls` – List files and directories

* **Syntax:** `ls [options]`
* `ls` – List files
* `ls -l` – Detailed listing
* `ls -a` – Show hidden files
* `ls -lh` – Detailed listing with readable sizes

#### 2. `pwd` – Show current directory

* **Syntax:** `pwd`
* `pwd` – Show current path

#### 3. `mkdir` – Create directory

* **Syntax:** `mkdir <directory>`
* `mkdir test` – Create `test`
* `mkdir -p a/b/c` – Create nested directories

#### 4. `cd` – Change directory

* **Syntax:** `cd <directory>`
* `cd test` – Enter `test`
* `cd ..` – Go to parent directory
* `cd ~` – Go to home directory
* `cd /` – Go to root directory
* `cd -` – Go to previous directory
* `cd ../sibling` – Move to a sibling directory

#### 5. `rmdir` – Remove empty directory

* **Syntax:** `rmdir <directory>`
* `rmdir test` – Remove `test`
* `rmdir -p a/b` – Remove directory and empty parent directories

#### 6. `cat` – Display file contents

* **Syntax:** `cat <file>`
* `cat notes.txt` – Display file
* `cat file1 file2` – Display multiple files
* `cat file1 file2 > combined.txt` – Combine files

#### 7. `cp` – Copy files/directories

* **Syntax:** `cp <source> <destination>`
* `cp a.txt b.txt` – Copy file
* `cp a.txt folder/` – Copy into directory
* `cp -r folder1 folder2` – Copy directory recursively

#### 8. `mv` – Move/rename files

* **Syntax:** `mv <source> <destination>`
* `mv old.txt new.txt` – Rename file
* `mv file.txt folder/` – Move file
* `mv folder1 folder2` – Move/rename directory

#### 9. `rm` – Delete files/directories

* **Syntax:** `rm <file>`
* `rm test.txt` – Delete file
* `rm -r folder` – Delete directory and contents
* `rm -i file.txt` – Ask before deleting

#### 10. `uname` – Show system information

* **Syntax:** `uname [options]`
* `uname` – Show kernel name
* `uname -a` – Show complete system information
* `uname -r` – Show kernel version

#### 11. `locate` – Find files by name

* **Syntax:** `locate <filename>`
* `locate notes.txt` – Find file
* `locate "*.pdf"` – Find PDF files

#### 12. `touch` – Create/update file

* **Syntax:** `touch <file>`
* `touch notes.txt` – Create empty file
* `touch a.txt b.txt` – Create multiple files

#### 13. `ln` – Create links

* **Syntax:** `ln [options] <source> <link>`
* `ln file.txt hardlink` – Create hard link
* `ln -s file.txt symlink` – Create symbolic link
hard link points to the same physical space on the disk
symbolic link is a file that holds the text path of another file or directory

#### 14. `clear` – Clear terminal

* **Syntax:** `clear`
* `clear` – Clear the screen

#### 15. `ps` – Show running processes

* **Syntax:** `ps [options]`
* `ps` – Show current processes
* `ps aux` – Show all running processes
* `ps -ef` – Detailed process list

#### 16. `man` – Command manual

* **Syntax:** `man <command>`
* `man ls` – Manual for `ls`
* `man cd` – Manual for `cd`
* `man -k keyword` – Search manuals by keyword

#### 17. `grep` – Search text

* **Syntax:** `grep [options] <pattern> <file>`
* `grep "Linux" notes.txt` – Search for `Linux`
* `grep -i "linux" notes.txt` – Case-insensitive search
* `grep -r "Linux" folder/` – Search recursively

#### 18. `echo` – Print text

* **Syntax:** `echo <text>`
* `echo "Hello"` – Print text
* `echo $HOME` – Display variable value
* `echo "Hello" > file.txt` – Write text to file

#### 19. `wget` – Download files

* **Syntax:** `wget <URL>`
* `wget https://example.com/file.zip` – Download file
* `wget -c <URL>` – Resume interrupted download

#### 20. `whoami` – Show current user

* **Syntax:** `whoami`
* `whoami` – Display username

#### 21. `sort` – Sort text

* **Syntax:** `sort [options] <file>`
* `sort names.txt` – Sort alphabetically
* `sort -r names.txt` – Reverse order
* `sort -n numbers.txt` – Numerical sorting

#### 22. `cal` – Display calendar

* **Syntax:** `cal [month] [year]`
* `cal` – Current month
* `cal 2026` – Full year calendar
* `cal 9 2026` – September 2026

#### 23. `whereis` – Locate command files

* **Syntax:** `whereis <command>`
* `whereis ls` – Locate ls
* `whereis python` – Locate Python files

#### 24. `df` – Show disk space

* **Syntax:** `df [options]`
* `df` – Show disk usage
* `df -h` – Human-readable sizes
* `df -T` – Show filesystem type

#### 25. `wc` – Count lines/words/characters

* **Syntax:** `wc [options] <file>`
* `wc notes.txt` – Lines, words, bytes
* `wc -l notes.txt` – Count lines
* `wc -w notes.txt` – Count words
* `wc -c notes.txt` – Count characters/bytes



## File Permission and Ownership Commands in Linux 

File Permission and ownership commands in linux are used to control access to files and directories by defining who can read, write or execute them.

chmod -> Change File Permissions
chattr -> Modify file attributes
chown -> Change file owner
chgrp -> Change file group

## 1. `chmod` – Change file permissions
controls who can read, write, or execute a file for the owner, group and others.
* **Syntax:** `chmod <mode> <file>`
* `chmod 755 file.txt` – Owner: `rwx`, Group/Others: `r-x`
* `chmod +x script.sh` – Add execute permission
* `chmod -w file.txt` – Remove write permission

## 2. `chown` – Change file owner

* **Syntax:** `chown <owner> <file>`
* `chown user file.txt` – Change owner
* `chown user:group file.txt` – Change owner and group
* `sudo chown user file.txt` – Change ownership as administrator

## 3. `chgrp` – Change group ownership

* **Syntax:** `chgrp <group> <file>`
* `chgrp developers file.txt` – Change file's group
* `sudo chgrp developers project/` – Change group of a directory

## 4. `chattr` – Change special file attributes

* **Syntax:** `chattr <attribute> <file>`
* `sudo chattr +i file.txt` – Make file **immutable** (cannot modify/delete)
* `sudo chattr -i file.txt` – Remove immutable attribute

### Important Permission Notation

```text
r = read
w = write
x = execute

Owner   Group   Others
 rwx     r-x     r--
```

Example:

```bash
chmod 754 file.txt
```

* **7** = `rwx` → Owner
* **5** = `r-x` → Group
* **4** = `r--` → Others

```
how is 754 calculated ? 
r = 4 (read)
w = 2 (write)
x = 1 (execute)

7 = 4 + 2 + 1 = rwx => Read + Write + Execute
5 = 4 + 0 + 1 = r-x => Read + Execute
4 = 4 + 0 + 0 = r-- => Read only

Owner can read, write and execute
Group can read and execute, but cannot modify it
Others can read only
```


# User Management Commands in Linux

These commands are mainly used to **create, modify, delete, and manage user accounts and login information**. ([GeeksforGeeks][1])

* **`useradd`** – Creates a new user account.
  * Example: `useradd john`

* **`userdel`** – Deletes a user account.
  * Example: `userdel john`

* **`usermod`** – Modifies an existing user account.
  * Example: `usermod -aG sudo john` – Add user to `sudo` group.

* **`passwd`** – Changes a user's password.
  * Example: `passwd john`

* **`chage`** – Manages password expiry and aging settings.
  * Example: `chage -l john` – View password expiry information.

* **`chsh`** – Changes a user's default login shell.
  * Example: `chsh john`

* **`chfn`** – Changes user information such as full name.
  * Example: `chfn john`

* **`id`** – Displays UID, GID, and group membership.
  * Example: `id john`

* **`whoami`** – Displays the currently logged-in username.
  * Example: `whoami`

* **`who`** – Shows currently logged-in users and their login details.
  * Example: `who`

* **`users`** – Shows usernames of currently logged-in users.
  * Example: `users`

* **`finger`** – Displays detailed information about a user.
  * Example: `finger john`

* **`pinky`** – Shows brief user information; simpler than `finger`.
  * Example: `pinky john`

* **`chpasswd`** – Changes passwords for multiple users in batch mode; mainly used by administrators.
  * Example: `echo "john:password" | chpasswd`


# Group Management Commands in Linux

Group management commands are used to **create, modify, delete, and manage user groups** in Linux. ([GeeksforGeeks][1])

* **`groupadd`** – Creates a new group.
  * Example: `sudo groupadd developers`

* **`groupdel`** – Deletes an existing group.
  * Example: `sudo groupdel developers`

* **`groupmod`** – Modifies group properties, such as its name.
  * Example: `sudo groupmod -n devteam developers`

* **`groups`** – Shows the groups a user belongs to.
  * Example: `groups john`

* **`gpasswd`** – Manages group membership.
  * Example: `sudo gpasswd -a john developers` – Add `john` to `developers`.

* **`grpck`** – Checks the integrity of group configuration files.
  * Example: `sudo grpck`

* **`grpconv`** – Converts group information to shadow format for better security.
  * Example: `sudo grpconv`

# OTHER COMMANDS 
Process management commands : 
[Process management commands](https://www.geeksforgeeks.org/linux-unix/process-management-commands-in-linux/)

Terminal and Session Management Commands : 
[Terminal and session management commands](https://www.geeksforgeeks.org/linux-unix/terminal-and-session-management-commands-in-linux/)

Job Scheduling Commands : 
[Job scheduling commands](https://www.geeksforgeeks.org/linux-unix/job-scheduling-commands-in-linux/)

Disk and File System Commands : 
- Creating and managing Disk Partitions
- Mounting and unmounting file systems
[Disk and file system commands](https://www.geeksforgeeks.org/linux-unix/disk-and-file-system-commands-in-linux/)

Hardware and System Information Commands : 
[Hardware and System Information Commands](https://www.geeksforgeeks.org/linux-unix/hardware-and-system-information-commands-in-linux/)

Networking Commands : 
[Networking commands](https://www.geeksforgeeks.org/linux-unix/networking-commands-in-linux/)

Package Management Commands
[Package management commands](https://www.geeksforgeeks.org/linux-unix/package-management-commands-in-linux/)

Compression and Archiving Commands : 
[Compression and Archiving Commands](https://www.geeksforgeeks.org/linux-unix/compression-and-archiving-commands-in-linux/)

Text Processing and Formatting Commands : 
[Text processing commands](https://www.geeksforgeeks.org/linux-unix/text-processing-and-formatting-commands-in-linux/)

Checksum and File Integrity Commands : 
[Checksum and file integrity commands](https://www.geeksforgeeks.org/linux-unix/checksum-and-file-integrity-commands-in-linux/)

Shell Built-in and Scripting Commands : 
https://www.geeksforgeeks.org/linux-unix/shell-built-in-and-scripting-commands-in-linux/

Development and Build Automation Commands : 
[Development and build automation commands](https://www.geeksforgeeks.org/linux-unix/development-and-build-automation-commands-in-linux/)

Kernel and Module Management Commands : 
[Kernel and Module Management Commands](https://www.geeksforgeeks.org/linux-unix/kernel-and-module-management-commands-in-linux/)

System Control and Power Commands : 
[System control commands](https://www.geeksforgeeks.org/linux-unix/system-control-and-power-commands-in-linux/)

Logging and Monitoring Commands : 
[Logging & Monitoring commands](https://www.geeksforgeeks.org/linux-unix/logging-and-monitoring-commands-in-linux/)

Mail and User Communication Commands : 
[Mail and user communication commands](https://www.geeksforgeeks.org/linux-unix/mail-and-user-communication-commands-in-linux/)

Date and Time Commands : 
[Date and time commands](https://www.geeksforgeeks.org/linux-unix/date-and-time-commands-in-linux/)


# Linux File System 
The Linux file system is a structured method of storing and organizing data on a Linux Machine. 
It arranges files in a hierarchical directory format starting from the root directory. 

- Single root directory (/)
- Hierarchical Tree Structure, making navigation simple and logical.
- Different directories like /home, /etc, /bin and /var serve specific system purposes.
- Linux supports multiple file system types like ext4, XFS, Btrfs, each offering different features

Architecture of linux file system
```
-------------------
User Application
-------------------
		|
		V
-------------------
Logical File System
-------------------
		|
		V
-------------------
Virtual File System
-------------------
		|
		V
-------------------
Physical File System
-------------------
		|
		V
-------------------
    Partition 1,
    Partition 2,
    Partition 3
-------------------
```

##### Logical FIle System : 
- Acts as the interface between user application and file system
- Handles key operations like open, read, write and close
- Provides security checks, such as permissions and file access control

##### Virtual File System (VFS)
- Provides a common interface for different file systems (ext4, XFS, FAT32, NTFS, etc.)
- Allows Linux to use multiple file system types at the same time
- Acts as an abstraction layer, hiding the internal complexities of each file system

##### Physical File System
- Directly interacts with the hardware and disk storage
- Manages data blocks, nodes and physical memory allocation
- Responsible for writing data to the disk and retrieving it efficiently

# Linux File Hierarchy Structure

Linux File Hierarchy Structure or FIlesystem Hierarchy Standard (FHS) defines the directory structure and directory contents in Unix like operating systems.

- All files and directories appear under the root directory /, even if stored on different physical or virtual devices

```
/         : root
	/bin : Essential User Command Binaries
	/boot : Static Files of the boot loader
	/dev : Device Files
	/etc : Host specific system configuration
	/home : User home Directories
	/lib : Shared Libraries
	/media : Removable Media
	/mnt : Temporarily Mounting Filesystems
	/opt : Add on Application software package
	/sbin : System Binaries
	/srv : Data for service from system
	/tmp : Temporary Files 
	/usr : User Utilities and Applications
	/proc : Process Information
```

Root :
- Only the root user has permission to modify contents inside this directory
- Regular users cannot make change here.

/bin : 
- Contains essential commands and binaries needed by all users, including cp, ls, ssh and kill
- Contains binary executables

/boot : 
- Stores all files required for booting the system, 
- including GRUB bootloader configuration and essential kernel files that are loaded during startup

/dev : 
- Stores device files
- Files that act as interfaces between hardware and software
- ex : /dev/sda1 for disk partitions

/etc : 
- Editable Text Configuration 
- Contains configuration files for system applications, users, services and tools
- ex : user details like UID and local addresses are defined here

/home : 
- Every non root user has a personal directory inside /home
- ex. : /home/anshu
- Each user can create, delete or modify files only in their own home directory and access of another user's home depends on security configuration

/lib : 
- Applications require libraries to run, which are stored in /lib
- These include libraries needed during runtime
- ex:  Apache server libraries are available here

/media : 
- temporary mount directory for removable devices like USBs, CDs and pen drives.

/mnt : 
- Temporary mount directory where sysadmins can mount filesystems

`/media` is used for automatic mounting of removable devices by the system or desktop environment, while `/mnt` is used for manual, temporary mounting by a system administrator

/opt : 
- Third pary software and packages not part of the default system installation are stored in /opt. 
- Contains add-on applications from individual vendors

/sbin 
- Contains binary executables, just like /bin. BUT by system administrators, for system maintenance purposes
- Ex : iptables, reboot, fdisk, ifconfig, swapon

/srv : 
- service, : contains server specific services related data

/tmp : 
- Contains temporary files created by system and users
- Files under this directory are deleted when the system is rebooted

/usr : 
- Contains binaries, libraries, documentation and source code for second level programs
- /usr/bin contains binary files for user programs
- /usr/sbin for system system administrators binary files
- /usr/lib for library files
- /usr/local for programs that you install from source
- /usr/src holds Linux Kernel sources, header-files and documentation
- /usr/include contains standard header files used by C and other programming languages

/proc : 
- Provides detailed information about system processes
- Each process is assigned a unique ID and represented as a directory inside /proc


# Linux Directory Structure
Linux Directory follows the Filesystem Hierarchy Standard (FHS)

Types of files in linux system : 
1. General Files : or ordinary files. Images, videos, program or simple text file.
2. Directory Files : Warehouse of other file types
3. Device Files : in linux devices are represented as files. ex: /dev/sda1, /dev/sda2, etc.

the /etc contains some system configuration files that define system behaviour
ex : 
/etc/passwd : contains user account information such as usernames and user IDs (passwords are stored in the shadow file)
/etc/security : contains security related configuration files, like login access control


# User Management 
User management is a core function of Linux System administration.
Linux supports multi user environments
- Secures the system from unauthorized access
- Ensures users can perform their roles without interfering with others
- Helps in auditing and tracking user activity

UID 0 : Reserved for root (superuser)
UID 1 - 999 : System accounts used by services
UID 1000+ : Regular Users


##### Types of Users in Linux : 
1. Root (Superuser) : Full system control. Can install software, change config files and delete anything. 
2. Regular User : Limited Access. Can create files, run applications, but not modify system-level settings.
3. Sudo User : Regular User with temporary admin rights via the sudo command. Common in modern systems.
4. System/Service Account : Non human accounts used by services (e.g. mysql, nginx). Limited Privilages
5. Guest User : Temporary users with minimal privilages. Changes not saved after logout.

