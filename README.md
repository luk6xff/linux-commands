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
  - [Bash](#bash)
  - [Networking](#networking) TODO
  - [Netcat](#netcat) TODO
  - [Cron](#cron)
  - [SSH](#ssh)
  - [Python](#python)
  - [Tmux](#tmux)
  - [Git](#git)
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
docker images
docker image ls
```

* Search images online
```
docker search $IMAGE_NAME
```

* Run a container from an image
```
docker run --name CONTAINER_ID_OR_NAME [OPTIONS...] $IMAGE_NAME $COMMAND $ARGUMENTS
# useful options:
## -i Keep stdin open even if not attached
## -d Run container in background
## -t allocate a pseudo-TTY
```

* Attach to a running container
```
docker exec -it CONTAINER_ID_OR_NAME /bin/bash
```

* List running container
```
docker ps # -a for all
```

* Start and stop container
```
docker start/stop CONTAINER_ID_OR_NAME
```

* Kill all containers
```
docker kill $(docker ps -q)
```

* Remove container
```
docker rmi CONTAINER_ID_OR_NAME
```

* Remove all containers
```
docker rm $(docker ps -q -a)  # -f for force remove even if it is running
```

* Export (save) an image
```
docker save IMAGE_NAME > IMAGE_NAME.tar.gz
```

* Load (import) an image
```
docker load -i IMAGE_NAME.tar.gz
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



## Bash
* Adding aliases
```
# In your ~/.bashrc or ~/.bash_aliases
alias dev='ssh foo@dev.example.com -p 22000'
```

* Make bash history 10,0000
```
export HISTSIZE=100000 SAVEHIST=100000 HISTFILE=~/.bash_history
```

* Prompt for input in a bash script
```
read -p “Do you want to continue?” variable
```

* Cut off the first column in a text file
```
cat filename | cut -d" " -f1
```

* Redirection of output
```
&> for redirection, it redirects both the standard output and standard error
```

* Find what a command does
```
whatis
eg. whatis grep
```

* Navigation
```
ctrl-w - delete the last word
ctrl-u - delete start of the line
ctrl-l - clear the screen
cd -  : go back to previous working dir
option-left/right - move word by word
```

* Loop through folders
```
for d in */ ; do
    echo "$d"
    cd $d
    <<comand here>>
    cd ..
done
```

* Base64 Decode
```
echo "word" | base64 -d
```

* Set variable
```
FOO="bar"
```

* Unset variable
```
unset FOO
```

* Recalling your variable by prepending it with a dollar sign ($).
```
echo $FOO
```

* Preserves any special characters that might appear in the variable;
```
echo "${FOO}"
```

* Bash loop
```
for f in * ;
    do file $f ;
done

# Or 1 liner:

for f in * ; do convert $f -scale 33% tmp/$f ; done
```

* Redirects
```
# cmd 1> file                         # Redirect stdout to file.
# cmd 2> file                         # Redirect stderr to file.
# cmd 1>> file                        # Redirect and append stdout to file.
# cmd &> file                         # Redirect both stdout and stderr to file.
# cmd >file 2>&1                      # Redirects stderr to stdout and then to file.
# cmd1 | cmd2                         # pipe stdout to cmd2
# cmd1 2>&1 | cmd2                    # pipe stdout and stderr to cmd2
```

* Variables
```
MESSAGE="Hello World"                        # Assign a string
PI=3.1415                                    # Assign a decimal number
```

* Arguments
```
$0, $1, $2, ...                              # $0 is the command itself
$#                                           # The number of arguments
$*                                           # All arguments (also $@)
```

* Special Variables
```
$$                                           # The current process ID
$?                                           # exit status of last command
eg:
if [ $? != 0 ]; then
    echo "command failed"
fi
mypath=`pwd`
mypath=${mypath}/file.txt
echo ${mypath##*/}                           # Display the filename only
echo ${mypath%%.*}                           # Full path without extention
foo=/tmp/my.dir/filename.tar.gz
path = ${foo%/*}                             # Full path without extention
var2=${var:=string}                          # Use var if set, otherwise use string
                                             # Assign string to var and then to var2.
size=$(stat -c%s "$file")                    # Get file size in bourne script
filesize=${size:=-1}
```

* Constructs
```
# FOR LOOP
for file in `ls`
do
    echo $file
done

# WHILE LOOP
count=0
while [ $count -lt 5 ]; do
    echo $count
    sleep 1
    count=$(($count + 1))
done

# FUNCTIONS
myfunction() {
    # $1 is first argument of the function
    find . -type f -name "*.$1" -print
}
myfunction "txt"

```

* Assigning output of one command to variable
```
#!/bin/bash
for node in $(cat nodes.txt)
do
    node_name=$(echo $node | tr -d '"');
    echo $node_name
done
```

* Checking for existence of arguments
```
if [ $# -eq 0 ]; then
    echo "Please enter an argument"
    exit 1
fi
```

* Check for environment variable
```
if [ -z "${GITHUB_TOKEN}" ]; then
    echo "Missing GITHUB_TOKEN environment variable"
    exit 1
fi
```

* Checking the output of last command and prompt to continue
```
if [[ $? -ne 0 ]]; then
  echo "command failed"
  read ABCD
fi
```

* Iterate over a list
```
list=(a1 a2 a3)
for n in ${list[@]}; do
    echo "*** $n ***" ;
done
```

* !^
```
!^ maps to the first argument of your latest command.
```

* !$
```
!$ maps to the last argument of your latest command.
```


* Brace expansion

```
# Expanded into ~/test/pics , ~/test/sounds, ~/test/sprites
mkdir ~/test/{pics,sounds,sprites}

# Create noise-1.mp3 to noise-5.mp3
touch ~/test/sounds/noise-{1..5}.mp3

# pic1.jpg pic3.jpg pic5.jpg pic7.jpg pic9.jpg
touch ~/test/pics/pic{1..10..2}.jpg
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



## Cron
* Format
```
# [minute] [hour] [day] [month] [weekday] [command to be executed]
#    *       *      *      *        *
#    |       |      |      |        |
#    |       |      |      |        +------ day of week (0-6) (Sunday=0)
#    |       |      |      |
#    |       |      |      +--------------- month (1-12)
#    |       |      |
#    |       |      +---------------------- day of month (1-31)
#    |       |
#    |       +----------------------------- hour (0-23)
#    |
#    +------------------------------------- min (0-59)
```

* Crontab
```
# Adding tasks easily (@reboot - on every reboot)
echo "@reboot echo hello" | crontab

# Open in editor
crontab -e

# List tasks
crontab -l [-u user]
```


## SSH
* Generate SSH key
```
ssh-keygen -t rsa -b 4096 -C "luk6xff@gmail.com"
```

* Retrieve the public key from a SSH private key
```
pip install -U pip
```

* SSH Login with username and port
```
ssh username@10.159.10.11 -p 22
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



## Git

* Configure Git
```
# Set your name
git config --global user.name <YOUR NAME>

# Set your email
git config --global user.email <YOUR EMAIL ADDRESS>
```


* Remove a file (or folder)
```
git rm -r foo.txt
```

* Merge a branch into the active branch
```
git merge [branch name]
```

* Merge a branch into a target branch
```
git merge [source branch] [target branch]
```

* Delete a remote branch
```
git push origin --delete [branch name]
```

* Add a remote repository
```
git remote add origin ssh://git@github.com/[username]/[repository-name].git
```

* Set a repository's origin branch to SSH
```
git remote set-url origin ssh://git@github.com/[username]/[repository-name].git
```

* View changes (detailed)
```
git log --summary
```

* Undo your most recent commit and put those changes back into staging
```
git reset --soft HEAD~1
```

* Completely delete the commit and throw away any changes
```
git reset --hard HEAD~1
```

* Squash commits (Last 4 commits not pushed yet to be squashed into one)
```
git rebase -i HEAD~4
```

* Tagging
```
# Fetch all the tags
git fetch --tags

# Create
git tag -a v1.0.0 -m "Initial release"

# Push
git push --tags

# Delete
git tag -d v1.0.0

# Remote
git push origin :refs/tags/v1.0.0
```

* Removes and deletes untracked files from the working tree
```
git clean -f
```

* Remove all untracked directories
```
git clean -fd
```

* Reverts all commits after specified commit, while keeping local changes
```
git reset <commitID>
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