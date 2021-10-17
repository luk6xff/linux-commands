# LINUX COMMANDS by LUK6XFF
All the commands I use on my daily basis
===

## Table Of Contents
- [LINUX COMMANDS by LUK6XFF](#linux-commands-by-luk6xff)
  - [Table Of Contents](#table-of-contents)
  - [User](#user)
  - [Docker](#docker)
  - [Hardware](#hardware)
  - [File System](#file-system)
  - [Performance](#performance)
  - [Command Line](#command-line) TODO
  - [Bash](#bash) TODO
  - [Networking](#networking) TODO
  - [Netcat](#netcat) TODO
  - [Cron](#cron) TODO
  - [Python](#python)
  - [Tmux](#tmux)
  - [Git](#git) TODO
  - [FFMpeg](#ffmpeg) TODO




## User
* Become a system admin
```
sudo -s
sudo su
```

* Switch user
```
su - $OTHER_USER
```

* Change a current password
```
passwd
```

* Lock user password
```
usermod -L USERNAME
```

* Define default attributes for new users (UID, Password Expiriation time, HomeDir)
```
vim /etc/login.defs
```

* Kill a process
```
# 1 HUP (hang up)
# 2 INT (interrupt)
# 3 QUIT (quit)
# 6 ABRT (abort)
# 9 KILL (non-catchable, non-ignorable kill)
kill -9 PID
```

* Kill all users processes
```
killall -u USERNAME
```

* Kill all processes by name
```
killall firefox
pkill -9 firefox
```

* Get process id
```
pgrep bash
```

* Reload process
```
sudo kill -HUP PID
```

* Display users using FILE/FOLDER
```
fuser -u FILE/FOLDER
```

* Kill processes using FILE/FOLDER
```
fuser -k FILE/FOLDER
```

* Add a new user
```
adduser USERNAME
```

* Show last logged in
```
last
last Log
last reboot  # shows last reboot
```

* Show all the groups of a current user
```
groups $USER
```

* Add a current user to Sudo
```
usermod -a -G sudo $USER
```

* Add path to system path
```
export PATH=$PATH:/usr/local/bin
```

* Print usernames of logged in users:
```
users
```

* Show info of the current user
```
id
```

* Show all users and host where logged in from
```
who -umH
```



## Docker
* List all images
```
sudo docker images
sudo docker image ls
```

* Search images online
```
sudo docker search $IMAGE_NAME
```

* Run a container from an image
```
sudo docker run --name $CONTAINER_NAME [OPTIONS...] $IMAGE_NAME $COMMAND $ARGUMENTS
# useful options:
## -i Keep stdin open even if not attached
## -d Run container in background
## -t allocate a pseudo-TTY
```

* Attach to a running container
```
sudo docker exec -it $CONTAINER_NAME /bin/bash
```

* List running container
```
sudo docker ps # -a for all
```

* Start and stop container
```
sudo docker start/stop $CONTAINER_NAME
```

* Kill all containers
```
sudo docker kill $(docker ps -q)
```

* Remove all containers
```
sudo docker rm $(docker ps -q -a) # and -f for force remove even if it is running
```



## Hardware
* Print full date and time:
```
date
```

* Print the hostname of this machine:
```
echo $HOSTNAME
```

* Print information about current linux distro:
```
lsb_release -a
cat /etc/*-release
cat /proc/version
```

* Print linux kernel version:
```
uname -a
```

* Print information about kernel modules:
```
lsmod
```

* Configure kernel modules:
```
modprobe
```

* Look for messages from drivers:
```
dmesg
```

* View Installed packages:
```
dpkg --get-selections
```

* Print environment variables:
```
printenv
```

* List hardware connected via PCI ports:
```
lspci
```

* List hardware connected via USB ports:
```
lsusb
```

* Copy a full hardware disk content to the second one (eg. Windows10 from HDD to SSD).
```
sudo dd if=/dev/sdb of=/dev/sda bs=64K status=progress conv=noerror,sync
```

* Make a bootable Linux USB from ISO
```
sudo umount /dev/sdc
sudo dd if=linuxmint-20-cinnamon-64bit.iso of=/dev/sdc bs=1M`
```

* Modify kernel parameters
```
vim /etc/sysctl.conf
```

* Backup & Restore MBR
```
# BACKUP
dd if=/dev/sda of=/tmp/mbr.img_backup bs=512 count=1
# RESTORE
dd if=/tmp/mbr.img of=/dev/sda bs=512 count=1
```

* Sync NTP time
```
sudo service ntp stop
sudo ntpdate -s time.nist.gov
sudo service ntp start
```

* Show Memory information
```
cat /proc/meminfo
```

* Show number of cores
```
lscpu
```

* Hardware Info
```
cat /proc/cpuinfo                  # CPU model
cat /proc/meminfo                  # Hardware memory
grep MemTotal /proc/meminfo        # Display the physical memory
watch -n1 'cat /proc/interrupts'   # Watch changeable interrupts continuously
free -m                            # Used and free memory (-m for MB)
cat /proc/devices                  # Configured devices
lspci -tv                          # Show PCI devices
lsusb -tv                          # Show USB devices
lshal                              # Show a list of all devices with their properties
dmidecode                          # Show DMI/SMBIOS: hw info from the BIOS
```

* Print your hardware specification in html format
```
sudo lshw -html > ~/hw_specs.html && firefox ~/hw_specs.html
```



## File System
* Show inodes of files and folders
```
ls -i
stat
```

* Find where a commmand is executed from
```
which python
```

* List directories and recurse into subdirectories
```
ls -r
```

* Find files bigger than 100m
```
find . -size +100M
```

* Find largest directories in current directory
```
du -hs */ | sort -hr | head
```

* Find files created within last 7 days
```
find . -mtime -7
```

* Find files accessed within last 7 days
```
find . -atime -7
```

* Find disk usage by a directory
```
du -sh /home/*
```

* Check for bad blocks on the disk
```
sudo badblocks -s /dev/sda
```

* Read speed test
```
sudo hdparm -tT /dev/sda
```

* Display mountpoints
```
lsblk
findmnt
sudo fdisk -l
df -h
df -h --output=source,target
```

* Zero Out all blocks
```
dd if=/dev/zero of=/dev/xvdf bs=1M
```


* Mount a new file system
```
fdisk /dev/hda1  # Create new partision
mkfs /dev/hda1   # Create file system
mount -a         # Causes all filesystems mentioned in fstab to be mounted
```

* Copy Files from Remote Machine to Local Machine
```
scp root@10.159.10.11:/path/to/foo /home/user/
```

* Copy Local directory to remote machine
```
scp -rp directory root@10.159.10.11:/path
```

* Copy Remote directory to local path
```
scp -r root@10.159.10.11:/path/to/foo /home/user/
```

* Copy a file from local computer to remote home directory
```
scp foo root@10.159.10.11:~/
```

* Compress a directory
```
tar -zcvf archive-name.tar.gz directory-name
    # -c = create
    # -f = following is archive name
    # -v = verbose
    # -z = gzip
```

* To append file to archive
```
tar rvf archive_name.tar new file.txt
```

* Uncompress file
```
unzip filename.zip
```

* Open a compressed .tgz or .tar.gz file:

```
tar -xvf [target.tgz]
tar -xvf —strip-components 1
tar -xvf -C  /path/to/directory
```

* Find all files containing `.cpp` in current directory
```
find . -type f -name *.cpp*
```

* Find all files not with permissions 644:
```
find . \! -perm 644 root -print
```

* Find files matching [filename]:
```
locate [filename]
```

* Show a file type
```
file foo.jpg
```

* Show uncommented items in config files
```
grep -v "#" foo.conf
```

* Search for a given string in all files recursively
```
grep -r "foo" *
```

* View the differences between two files:
```
diff foo1 foo2
```

* Change File Permissions
```
chmod 775 filename
chmod o+r file.txt
    # o=other +=add r=read
    # 7 = Read + Write + Execute
    # 6 = Read + Write
    # 5 = Read + Execute
    # 4 = Read
    # 3 = Write + Execute
    # 2 = Write
    # 1 = Execute
    # 0 = All access denied
    # First number is for the owner, second for the group, and third for everyon
```

* Check disk space
```
df -H
```

* Config Files
```
/etc/login.def - default settings template for new user accounts
/etc/motd - message of the day
/etc/inittab - defines default runlevel
```

* System Startup Files
```
/etc/rc.d  - scripts run from this subdir
/etc/init.d - hard location for startup scripts. Linked to /etc/rc.d/rc0.d ..etc.
/etc/rc.d/rc - file responsible for starting stopping services
/etc/rc0.d  - contains files with links to /etc/init.d/.
```

* Show Current Runlevel
```
runlevel
who -r
```

* Check file system consistency
```
Goto single user mode:
# init 1
Unmount file system:
# umount /dev/sdb1
Now run fsck command:
# fsck /dev/sdb1
```

* Generate md5
```
md5 filename
```

* Generate SHA256
```
openssl sha -sha256 filename
```

* Create symbolic Links
```
ln -s /path/to/original /path/to/symlink
```

* System wide limits
```
# View all system limits
sysctl -a
# View max open files limit
sysctl fs.file-max
# Change max open files limit
sysctl fs.file-max=102400
# How many file descriptors are in use
cat /proc/sys/fs/file-nr
```



## Performance
* Show running processes
```
ps –ax
ps –eaf
pstree
ps aux
```

* Show running processes with open network connections
```
lsof -i
```

* Show what files a process has open
```
lsof -p $PID
```

* Show open tcp sockets
```
lsof -nPi tcp
```

* Show all python-related PIDs and processes
```
pgrep -fl python
```

* Whats a process doing?
```
strace -f -p $PID
```

* How much memory is left
```
free -m
```



## Python
* Update pip:
```
pip install -U pip
```

* Create a python virtual environment under `venv` directory
```
python3 -m venv venv --no-site-packages
```

* Connect to a python virtual environment
```
source venv/bin/activate
```
* Disconnect from a python environment:
```
deactivate
```

* Install package into python virtual environment from outsie:
```
pip install [packagename]==[version_number] -E venv
```

* Export python virtual environment into a shareable format:
```
pip freeze -E venv > requirements.txt
```

* Import python virtual environment from a requirements.txt file:
```
pip install -E venv -r requirements.txt
```

* Share all files in current folder over port 8080
```
python -m SimpleHTTPServer 8080
```


## Tmux
* Commands
```
tmux ls = list sessions
tmux new = new session
tmux attach = attach to old session, keep existing sessions open
tmux attach -b =  attach to session, disconnect other connections
tmux kill-session
```

* Keys
```
Ctrl-b-d = detach from current session
Ctrl-b-n  = creates new window
Ctrl-b-0 = go to window 0
Ctrl-b-tab = toggle between windows
Ctrl-b-c = create new window
Ctrl-b-? = see bindings
Ctrl-b-x = close current window/pane
Ctrl-b-o = switch to other pane
Ctrl-b-q = show panes
Ctrl-b-V = new vertical pane
Ctrl-b-arrow = switch pane
```