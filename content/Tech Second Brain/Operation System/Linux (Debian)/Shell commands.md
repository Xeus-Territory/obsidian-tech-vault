---
title: Shell Command Collections
tags:
  - awesome
  - linux
  - command
  - helpful
  - usage
---
>[!info]
>Place to archive and snapshot the incredible command or pipe command with Linux OS platform such as Debian, Ubuntu, CentOS, ...

# In-house Commands
## `echo` command

Decode string with specify unicode-escaped with `-e` flag, read more at: [StackOverFlow - How to convert \uXXXX unicode to UTF-8 using console tools in *nix](https://stackoverflow.com/questions/8795702/how-to-convert-uxxxx-unicode-to-utf-8-using-console-tools-in-nix)

>[!note]
>You can use `uni2ascii` for instead if you want to integrate with 3rd party

```bash
echo -e "unicode-string"
```

## `journalctl` command

Documentation: [How To Use Journalctl to View and Manipulate Systemd Logs](https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs)

**Capture and logged full events of service**

```bash
journalctl -u service-name.service
```

**To see only log messages for the current boot**

```bash
journalctl -u service-name.service -b
```

**Find your boots in list**

```bash
journalctl --list-boots
```

**See the error log with command**

```bash
journalctl -p err -b 
```

>[!info]
>You can exchange -p option with pram
>- 0: emerg
>- 1: alert
>- 2: crit
>- 3: err
>- 4: warning
>- 5: notice
>- 6: info
>- 7: debug
## `grep` command

Use grep with exclude by `-v` flag

```bash
grep -v "dotnet" .
```

To grep include multiple word

>[!info]
>Use `-i` flag to execute that. Especially add with `\|` symbol between two words. Read more at: [How to Grep for Multiple Strings, Patterns or Words](https://phoenixnap.com/kb/grep-multiple-strings), extending with multiple situations **(HELPFUL)**

```bash
grep -i "Hostname\|Port"
```

## `awk` command

Skip first line Usually header when you use `awk` to print column variables

```bash
awk 'NR>1 {print $3}'
```

Get the last param when seperate by `/` or any symbol, you can use `F` and `$NF` to get the result

```bash
awk -F/ '{print $NF}'
```
## `tree` command

Print the sub-directory of folder with configuration level

```bash
tree -d -L 2 .
```

Print the sub file and folder with filter not include smt with `-I` option. Explore at [StackOverFlow - tree command for multiple includes and excludes](https://unix.stackexchange.com/questions/61074/tree-command-for-multiple-includes-and-excludes)

```bash
# With only
tree -a -L 1 -I .git

# With multiple
tree -a -L 1 -I '.git|.terraform.lock.hcl'
```

Print tree with combine full path, include and exclude pattern

```bash
tree -f -I "bin|unitTest" -P "*.[ch]|*.[ch]pp." your_dir/
```

## `find` command

Find the folder with find base on the regex format

```bash
find . -maxdepth 1 -type d -regex '.*/azp/_work/\d+$'
```

Find directory in current location but expose that in format `ls`

```bash
find . -type d -ls
```

Find the file or directory to provide you last in path of file and directory

```bash
find . -maxdepth 2 -type d | awk -F/ '{print $NF}'
```
## `iptables` command

Learn more about `iptables` commands from links down below

- [Github - iptables cheatsheet](https://gist.github.com/davydany/0ad377f6de3c70056d2bd0f1549e1017)
- [Prompt generate Iptables](https://bashsenpai.com/resources/cheatsheets/iptables)
- [Hacktricks - Suricata & Iptables cheatsheet](https://book.hacktricks.xyz/generic-methodologies-and-resources/basic-forensic-methodology/pcap-inspection/suricata-and-iptables-cheatsheet)
- [How to list all iptables rules with line numbers on Linux](https://www.cyberciti.biz/faq/how-to-list-all-iptables-rules-in-linux/)
- [How can I remove specific rules from iptables?](https://stackoverflow.com/questions/10197405/how-can-i-remove-specific-rules-from-iptables)

Allow only traffic from external IP to host via port

```bash
sudo iptables -A INPUT -s <source> -p <tcp/udp> --dport <destination-port> -j ACCEPT
```

Block all traffic to specify port in host

```bash
sudo iptables -A INPUT -p <tcp/udp> --dport <destination-port> -j DROP
```

List all rule and table rule

```bash
# List all rules
sudo iptables -S

# list all tables rules
sudo iptables -L -v -n | more

# list all rules for INPUT tables
sudo iptables -L INPUT -v -n
```

Delete rule in iptables

```bash
# Basic command to delete
sudo iptables -F

# To specify you want
# Find your rule base on number
iptables -L INPUT --line-numbers
# Remove that base on number of line
iptables -D INPUT <specific-line-number>

# IYKYN, use `-D` flag for same command `-A` to remove that rule
```

## `tr` command

Use `tr` to delete with `-d` flag

```bash
tr -d "HostName:Port" # If find 2 word, seperate with space
tr -d "HostNamePort" # If find 2 word, no space add-on
```

Use `tr` to change space to colon, ` ` --> `:`

```bash
tr -s "[:blank:]" ":"
```

## `ssh` command

Use tunneling mode of `ssh` to reverse shell from remote to your local host

>[!info]
>Command below to port-forward from port `127.0.0.1:8080` from remote host and send the traffic to port `:8080` inside your host

```bash
ssh -N -L 8080:127.0.0.1:8080 -i /path/to/your/private_key <user>@<remote-host> -p <port-ssh> # Default ssh via port 22, use -p if you need to specific
```

## `jq` command

List of articles relate `jq` with helpful solution

- [5 Useful jq Commands to Parse JSON on the CLI](https://www.fabian-kleiser.de/blog/5-useful-jq-commands-parse-json-cli/)
- [zendesk  - jq cheatsheet](https://www.fabian-kleiser.de/blog/5-useful-jq-commands-parse-json-cli/)
- [cheat.sh/jq](https://cheat.sh/jq)
- [jq 1.7 Manual](https://jqlang.github.io/jq/manual/)

You can use `jq` to select multiple variable

```bash
cat app.json | jq -r '.expo | .name, .version' 
```

You can use `jq` to select multiple variable and concat that to one string

```bash
cat app.json | jq -r '(.expo.name + "." + .expo.version)'
```

You can use `jq` with variable to pass through from command or define to your jq

```bash
curl -H "PRIVATE-TOKEN: $PRIVATE_GLAB_TOKEN" "https://gitlab.com/api/v4/users/$GLAB_USER_ID/contributed_projects" | jq --arg REPO_CHECKED_NAME "$REPO_CHECKED_NAME" '.[] | select(.name == $REPO_CHECKED_NAME) | .id'
```

`jq` support for another arg like `json`, you can try to concat object this one with your existence object. Explore more at [Add an object to existing JSON using jq](https://www.petermekhaeil.com/til/jq-append-json/) and [Append JSON Objects using jq](https://stackoverflow.com/questions/51147753/append-json-objects-using-jq)

```bash
cat ~/config-bk.json | jq -r --argjson addon "$(cat ~/.docker/config.json | jq -r ".auths")" '.auths+=$addon'
```

Convert json to string for multiple purpose

```bash
cat file.json | jq -c | jq -R
```

Get first keys in list object with `jq`

```bash
cat config-bk.json | jq  'keys[]'
```

Select the keys if value of a field is "auto". Explore at [Select the keys if value of a field is "auto"](https://unix.stackexchange.com/questions/719877/select-the-keys-if-value-of-a-field-is-auto)

```bash
# Get the object with value = auto
jq 'map_values(select(.value == "auto"))' file
# Get key with same situation
jq -r 'map_values(select(.value == "auto"))|keys[]' file
```
## `du` command 

You can use `du` command for list all size inside your directory

```bash
# List folder only
du -csh xeusnguyen.xyz

# List file inside
du -csh xeusnguyen.xyz/*
```

## `lsblk` command

If you want to take the look with your storage device like HDD or SSD, you can use `lsblk` to see what format of those devices

```bash
# View information about your disk
lsblk -o NAME,HCTL,SIZE,MOUNTPOINT

# View output info about filesystems
lsblk -f
```

|                 |                         |                                                        |                                             |
| --------------- | ----------------------- | ------------------------------------------------------ | ------------------------------------------- |
| **File System** | **Supported File Size** | **Compatibility**                                      | **Ideal Usage**                             |
| **FAT32**       | up to 4 GB              | Windows, Mac, Linux                                    | For maximum compatibility                   |
| **NTFS**        | 16 EiB – 1 KB           | Windows, Mac (read-only), most Linux distributions     | For internal drives and Windows system file |
| **Ext4**        | 16 GiB – 16 TiB         | Windows, Mac, Linux (requires extra drivers to access) | For files larger than 4 GB                  |
## `mkfs` command

You can use `mkfs` command to formatting your device. Read more at [How to Use the mkfs Command on Linux](https://www.howtogeek.com/443342/how-to-use-the-mkfs-command-on-linux/)

```bash
mkfs [options] [-t type fs-options] device [size]
```

## `fdisk` command

Documentation: [What is FDISK and how does it work?](https://www.techtarget.com/whatis/definition/FDISK)

Use `fdisk` when you want to hangout with your hard dkkkkkkkkkkkkkisk drive, like integrate multiple way for formatting or partitioning a [hard disk drive](https://www.techtarget.com/searchstorage/definition/hard-disk-drive), or to delete different portions of it. FDISK is an external utility. It is most commonly used to prepare and [partition](https://www.techtarget.com/searchstorage/definition/partition) a hard drive

```bash
# to view details of available disk partitions.
sudo fdisk -l
# to view the partitions on a specific disk.
sudo fdisk -l /dev/sda
# to create a hard disk partition.
sudo fdisk /dev/sda
# to view the partition size.
sudo fdisk -s /dev/sda
```

And when you want to hit to interaction mode you can try with

```bash
sudo fdisk /dev/sda
```

And when you hit keyboard with `m`, you can see the helper

```bash
Command (m for help): m

Help:

  GPT
   M   enter protective/hybrid MBR

  Generic
   d   delete a partition
   F   list free unpartitioned space
   l   list known partition types
   n   add a new partition
   p   print the partition table
   t   change a partition type
   v   verify the partition table
   i   print information about a partition

  Misc
   m   print this menu
   x   extra functionality (experts only)

  Script
   I   load disk layout from sfdisk script file
   O   dump disk layout to sfdisk script file

  Save & Exit
   w   write table to disk and exit
   q   quit without saving changes

  Create a new label
   g   create a new empty GPT partition table
   G   create a new empty SGI (IRIX) partition table
   o   create a new empty DOS partition table
   s   create a new empty Sun partition table

```

## `scp` command

Documentation: [SCP Command in Linux {13 Examples}](https://phoenixnap.com/kb/linux-scp-command)

`scp` is protocol which permit use copy and transfer file from remote and local machine with bi-direction, or cp from remote to remote

```bash
# From local to remote
scp /path/file/local user@ip:/path/file/remote

# From remote to local
scp user@ip:/path/file/remote /path/file/local

# From remote to remote
scp user1@ip1:/path/file/remote1 user2@ip2:/path/file/remote2

# From remote to remote (but your machine is mediate)
scp -3 user1@ip1:/path/file/remote1 user2@ip2:/path/file/remote2
```

In some special case, you can integrate with option with your scp command to specific

**Different Port**: Usually `scp` use SSH (Port 22) to mediate help you secure transfer data through that port, but in other situation SSH not work in Port 22, you can use `-p` to specific

```bash
scp -p 2222 /path/file/local user@ip:/path/file/remote
```

**Recursive**: To copy whole folder, usually we use recursive mode and `scp` does have with `-r`

```bash
scp -r /path/folder/ user@ip:/path/file/remote
```

## `sed` command

Documentation: [Sed cheatsheet](https://quickref.me/sed.html) and [How to Use Sed to Find and Replace a String in a File](https://phoenixnap.com/kb/sed-replace)

To replace a string in file with `sed`, you can use command with format

```bash
#Replace in file (Global)
sed -i 's/OLD/NEW/g' path/file #Replace string inside a file
```

To replace in the string, you can control action with

```bash
echo "[MASKED]" | sed -e "s/\[MASKED\]/123456789/g"
```

## `systemctl` command

Documentation: [Redhat - Systemd cheat sheet](https://access.redhat.com/node/1199213/40/0/13122931)

Use `systemctl` command to check available service inside your host with state running

```bash
sudo systemctl list-units --type=service --state=running
```

Use one of option Disable/Enable/Restart/Stop/Start with service inside host for changing state

```bash
sudo systemctl disable/enable/restart/stop/start <name_of_service>
```

Check configure or state of service with `systemctl` command

```bash
sudo systemctl show/status <name_of_services>
```
# External Commands
## Caddy server

Use the documentation to get valid command to handling [Caddy Server](https://caddyserver.com/docs/command-line#caddy-reload)

To reload configuration use `reload` command. Doc: [caddy reload](https://caddyserver.com/docs/command-line#caddy-reload)

```bash
caddy reload --config /location/caddy/file
```

To check and validate configuration from `caddyfile` use `validate` command. Doc: [caddy validate](https://caddyserver.com/docs/command-line#caddy-validate)

```bash
caddy validate --config /location/caddy/file
```

>[!note]
>With `Caddy V2`, mostly `websocket` automatically serve and do not need to configuration like `v1` . Check about that in [Caddy Proxy](https://caddyserver.com/docs/v2-upgrade#proxy)

## Vagrant

If you have problems when `vagrant` can't authentication for `ssh`. You will need concern to add this setting. Read more at: [Vagrant ssh authentication failure](https://stackoverflow.com/questions/22922891/vagrant-ssh-authentication-failure)

```bash title="Vagrantfile"
config.ssh.forward_agent = true
```

## Pip3 of Python

With Python3 from version 3.12, there isn't gonna easy for us to install package, so if you want to force install with `python3-pip`, you can add the optional `--break-system-packages` after the pip command

```bash
pip3 install numpy --break-system-packages
```

With read from file, we can do same way

```bash
pip3 install -r requirements.txt --break-system-packages
```

## Keytool of Java

Explore more about `keytool` through [Common Java Keytool Commands](https://knowledge.digicert.com/de/de/quovadis/ssl-certificates/ssl-general-topics/common-java-keytool-commands)
### Get keystroke information

To view and check information which store inside keystroke, which generate from `keytool` - key generator integrate into Java

```bash
keytool -list -v -keystore /path/to/your/keystore-file.keystore -alias your-key-alias -storepass your-keystore-password -keypass your-key-password
```