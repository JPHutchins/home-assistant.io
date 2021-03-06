---
title: "Installation on Vagrant"
description: "Instructions to run Home Assistant on a Vagrant VM."
redirect_from: /getting-started/installation-vagrant/
---

A `Vagrantfile` is available into `virtualization/vagrant` folder for quickly spinning up a Linux virtual machine running Home Assistant. This can be beneficial for those who want to experiment with Home Assistant and/or developers willing to easily test local changes and run test suite against them. In the same `virtualization/vagrant` folder there's also a `provision.sh` shell script which provides an easy way to interact with the Home Assistant instance running within the Vagrant VM. For Windows, use the batch script `provision.bat`.

<div class='note'>
Vagrant is intended for testing/development only. It is NOT recommended for permanent installations.
</div>

## Install Vagrant

You must have [Vagrant](https://www.vagrantup.com/downloads.html) and [Virtualbox](https://www.virtualbox.org/wiki/Downloads) installed on your workstation. Vagrant and Virtualbox support all the main platforms, including Windows, MacOS and Linux.

Limited support is available for Hyper-V on Windows, see below.

## Get Home Assistant source code

Download the Home Assistant source code by either downloading the .zip file from [GitHub releases page](https://github.com/home-assistant/home-assistant/releases) or by using [Git](https://git-scm.com/)

```bash
$ git clone https://github.com/home-assistant/home-assistant.git
$ cd home-assistant/virtualization/vagrant
```

<div class='note'>

The following instructions will assume you changed your working directory to be `home-assistant/virtualization/vagrant`. This is mandatory because Vagrant will look for information about the running VM inside that folder and won't work otherwise

</div>

<div class='note'>

When using Vagrant on Windows, change git's `auto.crlf` to input before cloning the Home Assistant repository. With input setting git won't automatically change line endings from Unix LF to Windows CRLF. Shell scripts executed during provision won't work with Windows line endings.

</div>

```bash
$ git config --global core.autocrlf input
```

## Create the Vagrant VM and start Home Assistant

```bash
$ ./provision.sh setup
```

This will download and start a virtual machine using Virtualbox, which will internally setup the development environment necessary to start Home Assistant. The whole process might take up to 30 minutes to complete, depending on Internet connection speed and workstation resources. After the VM has started successfully, the Home Assistant frontend will be accessible locally from your browser at [http://localhost:8123](http://localhost:8123)

## Stopping Vagrant

To shutdown the Vagrant host:

```bash
$ ./provision.sh stop
```

To start it again:

```bash
$ ./provision.sh start
```

## Restarting Home Assistant process to test changes

The root `home-assistant` directory on your workstation will be mirrored with `/home-assistant` inside the VM. In `virtualization/vagrant` there's also a `config` folder that you can use to drop configuration files (Check the [Configuration section](/docs/configuration/) in the docmentation for more information about how to configure Home Assistant).

Any changes made to the local directory on your workstation will be available from the Vagrant host, so to apply your changes to the Home Assistant process, just restart it using the provided `provision.sh` wrapper script:

```bash
$ ./provision.sh restart
```

<div class='note'>
This command will only restart the Home Assistant process inside the Vagrant VM, it will not reboot the virtual machine. If that's what you want, the right command is <code>vagrant reload</code>
</div>

## Run test suite (Tox)

To run tests against the local version of Home Assistant code:

```bash
$ ./provision.sh tests
```

## Cleanup

To completely remove the VM

```bash
$ ./provision.sh destroy
```

To completely remove the VM **and** setup a fresh new environment:

```bash
$ ./provision.sh recreate
```

## Windows

On Windows, Vagrant is launched through an elevated `PowerShell`. Use the batch script `provision.bat` instead of the shell script `provision.sh`.

## Hyper-V

It is possible to use Hyper-V instead of Virtualbox on Windows, with some limitations.

Samba is used for the virtual machine to access files, for which the Windows credentials are needed when the machine is created.
As Hyper-V does not allow for port forwarding, NAT is used by default for the network. Through creating an external network switch in Hyper-V it is possible to access the machine on the network.
The IP address is visible on creation, and through the Hyper-V manager.
