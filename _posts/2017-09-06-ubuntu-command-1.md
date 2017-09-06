---
layout: post
title:  "ssh, compress, scp"
---

## ssh config for server
[SSH/OpenSSH/Configuring](https://help.ubuntu.com/community/SSH/OpenSSH/Configuring)

## ssh connect
[SSH/OpenSSH/ConnectingTo](https://help.ubuntu.com/community/SSH/OpenSSH/ConnectingTo)

### From Unix-like systems (including Mac OS X)
 To login to your computer from a Unix-like machine, go to a command-line and type:
 ```
  ssh <username>@<computer name or IP address>
 ```
 For example:
```
ssh joe@laptop
```
or:
```
ssh mike@192.168.1.1
```

You should get the usual password prompt (or be told you can't log in, if passwords are disabled).
See [ssh keys](https://help.ubuntu.com/community/SSH/OpenSSH/Keys) if you want to authenticate using keys instead of passwords.

## [compress/extract file](https://help.ubuntu.com/community/FileCompression)
### Extracting a tar.bz2 file

- Extract <filename>tar.bz2
```bash
tar jxvf <filename>.tar.bz2
```
>This will show you what it extracts, and in most cases will create a directory named `<filename>`.

- To extract a `tar.gz`, **simply put z in place of j in the command line**. For more information please refer to `man tar`.

### Creating a tar.bz2 file
- 
```
  tar jcvf <filename>.tar.bz2 dir1 dir2 file1 file2 ...
```
>`<filename>.tar.bz2` is the name of the tar file we wish to create. dir# and file# are the names of the directories or files we wish to include in the tar.bz2 archive.

- To use `gzip` compression instead of `bz2`, **simply put z in place of j in the command line**.

For more information please refer to man tar.

## Rar

>Rar v3.0 archives require the use of unrar.

### unrar-free
- Extract <filename>.rar
```
 unrar-free <filename>.rar
```

>If extracting fails with a message like the following, try the non-free version.
```
Extracting  file1.ext Failed
Extracting  file2.ext Failed
2 Failed
```

### unrar-nonfree
- Extract <filename>.rar
```
unrar x <filename>.rar
```

## zip and unzip

- To create a zip file containing dir1, dir2, ... :
```
zip -r <filename>.zip dir1 dir1 ...
```

- To extract <filename>.zip:
```
unzip <filename>.zip
```

## [scp](https://help.ubuntu.com/community/SSH/TransferFiles)

>Another important function of SSH is allowing secure file transfer using SCP and SFTP.

### Secure Copy (scp)
To copy a file from your computer to another computer with ssh, go to a command-line and type:
```
scp <file> <username>@<IP address or hostname>:<Destination>
```
For example, to copy your TPS Reports to Joe's Desktop:

```
scp "TPS Reports.odw" joe@laptop:/home/joe/Desktop/
```

To copy the pictures from your holiday to your website, you could do:
```
scp -r /media/disk/summer_pics/ mike@192.168.1.1:"/var/www/Summer 2008/"
```

The -r (recursive) option means to copy the whole folder and any sub-folders. You can also copy files the other way:

```
scp -r catbert@192.168.1.103:/home/catbert/evil_plans/ .
```

The '.' means to copy the file to the current directory. Alternatively, you could use secret_plans instead of '.', and the folder would be renamed.

## Secure FTP (sftp)
Finally, if you want to look around the remote machine and copy files interactively, you can use SFTP:
```
sftp linus@kernel.org
```
This will start an SFTP session that you can use to interactively move files between computers.
