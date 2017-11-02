# Project Magpie

Welcome to Project Magpie.  The Project Magpie appliance contains various tools
to support operation and management of VMware NSX deployments.  It is provided 
free of charge and without warranty of any kind.

This git repository is a placeholder for Project Magpie.  Magpie will be 
released as opensource as soon as we sort out some architectural challenges and 
direction.

# Installation

- Download the OVA from releases.
- Deploy to ESXi, VMware workstation or Fusion.
- Accept the EULA.
- Assign the appliance network to an appropriate portgroup or network.
- Start the appliance and login as `root/changeme` and set the root password when prompted.
- Magpie is based on VMware Photon and basic Photon operation applies.  To check a dhcp assigned address use `ip addr show dev eth0`.  For manual interface configuration, see the VMware Photon documentation at https://github.com/vmware/photon/wiki.

# Functionality

Project Magpie v0.1 has functionality focussed only on providing a simple to install, multiuser PowerNSX appliance.  We have much bigger plans than that though, so check back regularly to see whats been added, as this is just the tip of the iceberg.

The following functionality is currently available:
## Access PowerNSX via a browser

Now you have no need to install PowerShell and PowerNSX/PowerCLI bits on multiple machines to support PowerNSX based operations.  Deploy the appliance once, create accounts for all users that require it and immediately access PowerNSX / PowerCLI from anywhere via a web browser.  Nothing to install on a client machine.

## View the latest PowerNSX documentation

As of build 3.0.1054 of PowerNSX, we have started autogenerating our documentation to make it available online.  The Magpie appliance puts that at your fingertips via its documentation tab where you can access searchable PowerNSX documentation.

## Access PowerNSX via SSH

You can also access the same PowerNSX / PowerCLI environment via ssh for the same experience in a terminal via ssh.

# Usage

## Access the Magpie browser interface

Point a browser at `https://<appliance ip>`.  That was easy.  The only valid account out of the box is the `powernsx` account (root credentials will not work here).  Login with its default password of `changeme` and you will be promtped to change it on first use.

## Access the PowerNSX / PowerCLI environment via ssh

Point an ssh client at port 2222 on the appliance.  Note that root logins will not work, only the powernsx and any other valid users created via the method below will work here.

## Create new users

To add new users to access PowerNSX / PowerCLI via web browser or ssh:

- Ssh to the Magpie appliance as root (port 22)
- Run `create_powernsx_user <username>`.  You will be prompted for a password.
- The user can now ssh/use web console to with their new username.

Changing a users password:
- The user can either login as per normal and use 'passwd' to set their password, or
- Ssh to appliance as root (port 22).
- Run `set_powernsx_user_pass <username>`.

# Known Issues

There are a couple of known issues / requirements.
- The PowerNSX documentation page retreives updated documentation live from the PowerNSX Github repo.  It requires internet connectivity for Powernsx Documentation page to load and will not display anything if access is not available.
- On first boot, occasionally the web console disconnects randomly (ssh is unaffected).  Reboot the appliance to resolve.
