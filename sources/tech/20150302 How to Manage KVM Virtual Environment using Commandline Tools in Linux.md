Translating by ZTinoZ
How to Manage KVM Virtual Environment using Commandline Tools in Linux
================================================================================
In this 4th part of our [KVM series][1], we are discussing KVM environment management using CLI. We use ‘virt-install’ CL tool to create and configure virtual machines, virsh CL tool to create and configure storage pools and qemu-img CL tool to create and manage disk images.

![KVM Management in Linux](http://www.tecmint.com/wp-content/uploads/2015/02/KVM-Management-in-Linux.jpg)
KVM Management in Linux

There is nothing new concepts in this article, we just do the previous tasks using command line tools. There is no new prerequisite, just the same procedure, we have discussed in previous parts.

### Step 1: Configure Storage Pool ###

Virsh CLI tool is a management user interface for managing virsh guest domains. The virsh program can be used either to run one command by giving the command and its arguments on the shell command line.

In this section, we will use it to create storage pool for our KVM environment. For more information about the tool, use the following command.

    # man virsh

**1. Using the command pool-define-as with virsh to define new storage pool, you need also to specify name, type and type’s arguments.**

In our case, name will be Spool1, type will be dir. By default you could provide five arguments for the type:

- source-host
- source-path
- source-dev
- source-name
- target

For (Dir) type, we need the last argumet “target” to specify the path of storage pool, for the other arguments we could use “-” to unspecific them.

    # virsh pool-define-as Spool1 dir - - - - "/mnt/personal-data/SPool1/"

![Create New Storage Pool](http://www.tecmint.com/wp-content/uploads/2015/02/Create-New-Storage-Pool.png)
Create New Storage Pool

**2. To check the all storage pools you have in the environment, use the following command.**

    # virsh pool-list --all

![List All Storage Pools](http://www.tecmint.com/wp-content/uploads/2015/02/List-All-Storage-Pools.png)
List All Storage Pools

**3. Now it’s time to build the storage pool, which we have defined above with the following command.**

    # virsh pool-build Spool1

![Build Storage Pool](http://www.tecmint.com/wp-content/uploads/2015/02/Build-Storage-Pool.png)
Build Storage Pool

**4. Using the virsh command pool-start to active/enable the storage pool we have just created/built above.**

    # virsh pool-start Spool1

![Active Storage Pool](http://www.tecmint.com/wp-content/uploads/2015/02/Active-Storage-Pool.png)
Active Storage Pool

**5. Check the status of environment storage pools using the following command.**

    # virsh pool-list --all

![Check Storage Pool Status](http://www.tecmint.com/wp-content/uploads/2015/02/Check-Storage-Pool-Status.png)
Check Storage Pool Status

You will notice that the status of Spool1 converted to active.

**6. Configure Spool1 to start by libvirtd service every time automaticlly.**

    # virsh pool-autostart Spool1

![Configure KVM Storage Pool](http://www.tecmint.com/wp-content/uploads/2015/02/Configure-Storage-Pool.png)
Configure KVM Storage Pool

**7. Finally lets display information about our new storage pool.**

    # virsh pool-info Spool1

![Check KVM Storage Pool Information](http://www.tecmint.com/wp-content/uploads/2015/02/Check-Storage-Pool-Information.png)
Check KVM Storage Pool Information

Congratulations, Spool1 is ready to be used lets try to create storage volumes using it.

### Step 2: Configure Storage Volumes/Disk Images ###

Now it is disk image’s turn, using qemu-img to create new disk image from Spool1. For more details about qemy-img, use the man page.

    # man qemu-img

**8. We should specify the qemu-img command “create, check,….etc”, disk image format, the path of disk image you want to create and the size.**

    # qemu-img create -f raw /mnt/personal-data/SPool1/SVol1.img 10G

![Create Storage Volume](http://www.tecmint.com/wp-content/uploads/2015/02/Create-Storage-Volumes.png)
Create Storage Volume

**9. By using qemu-img command info, you could get information about your new disk image.**

![Check Storage Volume Information](http://www.tecmint.com/wp-content/uploads/2015/02/Check-Storage-Volume-Information.png)
Check Storage Volume Information

**Warning**: Never use qemu-img to modify images in use by a running virtual machine or any other process; this may destroy the image.

Now its time to create virtual machines in the next step.

### Step 3: Create Virtual Machines ###

10. Now with the last and latest part, we will create virtual machines using virt-istall. The virt-install is a command line tool for creating new KVM virtual machines using the “libvirt” hypervisor management library. For more details about it, use:

    # man virt-install

To create new KVM virtual machine, you need to use the following command with all the details like shown in the below.

- Name: Virtual Machine’s name.
- Disk Location: Location of disk image.
- Graphics : How to connect to VM “Usually be SPICE”.
- vcpu : Number of virtual CPU’s.
- ram : Amount of allocated memory in megabytes.
- Location : Specify the installation source path.
- Network : Specify the virtual network “Usually be vibr00 bridge”.

    # virt-install --name=rhel7 --disk path=/mnt/personal-data/SPool1/SVol1.img --graphics spice --vcpu=1 --ram=1024 --location=/run/media/dos/9e6f605a-f502-4e98-826e-e6376caea288/rhel-server-7.0-x86_64-dvd.iso --network bridge=virbr0

![Create New Virtual Machine](http://www.tecmint.com/wp-content/uploads/2015/02/Create-New-Virtual-Machines.png)
Create New Virtual Machine

**11. You will find also a pop-up virt-vierwer window appears to communicate with virtual machine through it.**

![Booting Virtual Machine](http://www.tecmint.com/wp-content/uploads/2015/02/Booting-Virtual-Machine.jpeg)
Booting Virtual Machine

![Installation of Virtual Machine](http://www.tecmint.com/wp-content/uploads/2015/02/Installation-of-Virtual-Machine.jpeg)
Installation of Virtual Machine

### Conclusion ###

This is the latest part of our KVM tutorial, we haven’t covered everything of course. It a shot to scratch the KVM environment so its your turn to search and keep hands dirty using this nice resources.

- [KVM Getting Started Guide][2]
- [KVM Virtualization Deployment and Administration Guide][3]

--------------------------------------------------------------------------------

via: http://www.tecmint.com/kvm-management-tools-to-manage-virtual-machines/

作者：[Mohammad Dosoukey][a]
译者：[译者ID](https://github.com/译者ID)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创翻译，[Linux中国](http://linux.cn/) 荣誉推出

[a]:http://www.tecmint.com/author/dos2009/
[1]:http://www.tecmint.com/install-and-configure-kvm-in-linux/
[2]:https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Virtualization_Getting_Started_Guide/index.html
[3]:https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Virtualization_Deployment_and_Administration_Guide/index.html
