---
description: Instructions to fetch / store VMs
---

# Virtual Machine Settings

## Import VM from NAS

A network attached storage (NAS) is used to store all iso images, scripts, as well as your backups.

```
[username@clemson11 ~]$ sftp username@192.168.10.5:VMs/xxx.ova .
username@192.168.10.5's password: 
Connected to 192.168.10.5.
Fetching /VMs/Assign1_VM.ova to ./xxx.ova
/VMs/xxx.ova              100% 2352MB  72.0MB/s   00:32    
sftp> exit
[username@clemson11 ~]$ vboxmanage import xxx.ova 
```

For students in Charleston, use "sftp username@192.168.20.5".

## Import VM from home directory

For Command Line Interface, use

```
vboxmanage import /home/username/VMs/xxx.ova
==OR==
vboxmanage registervm /home/username/VMs/xxx.vbox
```

For GUI

File -> Import Appliance -> select OVA file.

## Back up VM to NAS

For Clemson Main Campus student, use 192.168.10.5 for NAS

For Charleston Campus student, use 192.168.20.5 for NAS

```
[username@clemson10 ~]$ sftp username@192.168.10.5
username@192.168.10.5's password: 
Connected to 192.168.10.5.
sftp> ls
VMs   home  
sftp> cd home/  
sftp> put -r VirtualBox\ VMs/Assign1_VM/
Uploading VirtualBox VMs/Assign1_VM/ to /home/Assign1_VM
Entering VirtualBox VMs/Assign1_VM/
```

To delete a file:

```
sftp> cd home/
sftp> ls
Assign1_VM   tmp          
sftp> rm tmp 
Removing /home/tmp
```

To delete all files in a folder:

```
sftp> rm Assign1_VM/*
Removing /home/Assign1_VM/Assign1_VM-disk001.vmdk
```

To delete a folder:

```
sftp> rmdir Assign1_VM/
```

To make a folder:

```
sftp> mkdir foldername
```

Get your back up folder:

```
sftp> get -r foldername/
```

or file:

```
sftp> get filename
```

You can download your back up from NAS on any machines in the lab.

## Log out

Please logout of your account before leave the lab, which will protect your account. Do not power off the machine.

<figure><img src=".gitbook/assets/WeChat Image_20180828160850.jpg" alt=""><figcaption></figcaption></figure>
