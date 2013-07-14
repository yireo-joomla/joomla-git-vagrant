Joomla! development with Vagrant
================================

Introduction
------------
This project offers a way for using Vagrant and VirtualBox to quickly setup
Joomla! development-versions. VirtualBox is a free program that allows you to run
virtual machines on a server or a desktop, while Vagrant offers a scripted method to 
quickly setup such a virtual machine.

That's great. But why this project? Here's why: During Joomla! bug-squashing
sessions, we found that numerous Joomla! users were willing to debug
Joomla!, but had a hard time to setup Joomla! using the GitHub procedure. 
They are test-users, not developers.
To simplify the process, this project allows for setting up a new virtual machine in
minutes, so they can access Joomla! without setting up anything.

The __remote__ folder contains files that are copied by Vagrant
to the new virtual machine. You are in control of what is placed inside it.
By adding the Git-sources of the Joomla! CMS GitHub project
to this folder, those sources only need to be downloaded once, while numerous
users can re-use those sources for testing. This allows for Joomla! bug-squashing
in places where internet is slow or even absent.

Benefits
--------
* Setup numerous virtual machines easily
* Save bandwidth by providing Joomla! files / Git-sources
* Make it easy for Joomla! testers to test Joomla!

For whom is this
----------------
This project is ment for Joomla! developers, who are familiar with
Linux, and want to setup a bunch of virtual machines quickly so that
end-testers can use them to test Joomla!. Ideally a Linux laptop or
desktop is used with a minimum of 2Gb RAM available. Each VM takes about
128Mb to run, which allows you to run numerous virtual machines
simultaneously. 
Additionally, each VM can be hosting multiple Joomla! installations.

Getting started
---------------
To use the files here, make sure to install Vagrant and VirtualBox
first. If you are new to VirtualBox, make sure to setup a simple virtual
machine first, using Linux of some kind.

Download the file __package.box__ from this URL (480Mb).
    https://s3-eu-west-1.amazonaws.com/yireo-vagrant/package.box

Once Vagrant and VirtualBox are installed, copy the files of this
project plus the file __package.box__ to a local folder (__/example/__):
    /example/package.box
    /example/README.md
    /example/remote/
    /example/Vagrantfile
   
Open a shell. Set the working directory to
this folder, and run the command __vagrant up__:

    cd /example/
    vagrant up

With this command, Vagrant will try to setup a new virtual machine,
based upon the files of this project. After the virtual machine has been
created, the IP-address can be used to access the webroot.

    http://x.x.x.x/

The virtual machine
-------------------
This virtual machine is based on VirtualBox, so to run this machine on
your own desktop, you will need VirtualBox as well. The machine is
designed to run with 128Mb, which allows you to run multiple virtual
machines at the same time. Networking is configured for DHCP.

Suggested procedure for Joomla! bug-squashing
=============================================
One computer (laptop, desktop) is used as host for various VirtualBox guest-VMs.
Each guest-VM is capable of running one or more Joomla! sites. Note that each 
guest-VM is configured with 128MB RAM. Running up to 8 Joomla! sites should be workable.

To prepare for the session, go to the __remote__ folder and checkout the GitHub sources: 
    git clone git://github.com/joomla/joomla-cms.git

Move this folder to a new folder like this, and create duplicates:
    mv joomla-cms joomla1
    cp -R joomla1 joomla2
    cp -R joomla1 joomla3
    cp -R joomla1 joomla4

Assign each folder to a specific user. See the FAQ below on how these folders 
are used to actually install Joomla!. Each user is able to either simply install
Joomla! and start testing. After testing has been completed (for instance, once a 
specific bug has been confirmed, or a fix has been tested), an advanced user (or
the Vagrant admin) can login through SSH, and reset all local changes:
    git reset --hard HEAD

Optionally, new commits can be fetched:
    git pull

Hint: You can also include other files in the __remote__ folder, for instance the 
official Joomla! stable-package, or additional extensions.

Frequently Asked Questions
==========================

FAQ: What is a virtual machine?
-------------------------------
A virtual machine is actually just a set of files, the most importat
file being a VMDK-file, that represent the hard disk of the virtual
machine. Using these files, the VirtualBox-program can simulate a
physical computer on your own desktop.

This is useful when wanting to test Joomla!, because Joomla! requires a
combination of an operating system, database-server, webserver and PHP:
Commonly this is a combination of Linux, Apache, MySQL and PHP - in
short referred to as LAMP. But setting up LAMP on your local desktop
might require a lot of effort. With a virtual machine, all the hard work
has already been done for you.

FAQ: Why not use a generic virtual machine?
-------------------------------------------
Instead of just offering a virtual machine, this project uses Vagrant.
This gives the benefit of not just offering to run a single virtual
machine, but actually 

FAQ: What are the virtual machine details?
------------------------------------------
The virtual machine referred to in this project is based on VirtualBox.
The RAM assigned to the VM is 128Mb, allowing you to create multiple
VMs easily on a modern-day desktop or laptop.

The VM is based upon CentOS.

FAQ: How to install Joomla!? 
----------------------------
You can simply upload files through SSH or SFTP. By logging in as
Linux-user "joomla" with password "joomla", you gain access to the
webroot of the virtual machine. It is recommended to create a new
subfolder the webroot, for example "joomla-test".

Once you run the Joomla! Installer, you will be asked for
database-details: Just enter as MySQL-user "root", and as 
MySQL-password "root". As MySQL-database-name, you can just enter any
name. Joomla! will create the database for you.

FAQ: How to become root?
------------------------
Becoming root is only ment for experts. Password is "root".

FAQ: How to administer the database?
------------------------------------
The administrative user for MySQL is "root" with password "root".

FAQ: Where is phpMyAdmin?
-------------------------
phpMyAdmin is not installed by default.

FAQ: Can I use this virtual machine for a production environment?
-----------------------------------------------------------------
Yes, you can. But you should not. This VM is designed to be insecure.

Vagrant administration
======================
Basic commands
--------------
You can use the following command to create a new VM:
    vagrant up

To administer the VM itself, either SSH to the VM directly, or use the
__vagrant__ command:
    vagrant ssh

To remote the VM again:
    vagrant destroy

Hints for creating your own box
-------------------------------
Instead of using our prepped box (VirtualBox VMDK-file plus some extra
files), you can simply create your own VirtualBox machine. Make sure to
setup SSH with the Vagrant public-key authentication, and make sure to
install the VirtualBox Guest Additions, so that folders can be synced by
Vagrant.

When using CentOS, make sure to clean up the VM before offering the
VMDK-file to the public:
    yum clean all

When you're done with setting up the VM, shut it down. Make sure
VirtualBox as a program is still running, and run this from your host:
    vagrant package --base joomla-cms

This will turn your VM into an actual Vagrant package.
