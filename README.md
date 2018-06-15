# Current version - v0.2 (June '18)

File is too large for GH releases.  GDrive link : 
https://drive.google.com/open?id=1T0mRjMQykZ_9Of1IfloBwGFDhoxmX33-

# Project Magpie

Welcome to Project Magpie.  The Project Magpie appliance contains various tools
to support operation and management of VMware NSX deployments.  It is provided 
free of charge and without warranty of any kind.

This git repository is a placeholder for Project Magpie.  We intend to release 
Magpie as opensource as soon as we sort out some architectural challenges and 
direction.

# Installation

- Download the OVA from releases.
- Deploy to ESXi, VMware workstation or Fusion.
- Accept the EULA.
- Assign the appliance network to an appropriate portgroup or network.
- Start the appliance and login as `root/changeme` and set the root password when prompted.
- Magpie is based on VMware Photon and basic Photon operation applies.  To check a dhcp assigned address use `ip addr show dev eth0`.  For manual interface configuration, see the VMware Photon documentation at https://github.com/vmware/photon/wiki.
- Login via a browser (https) with `admin/VMware1!`.  (You can change the default admin password with the account management icon in the navbar as well as add new users)

# Releases

## v0.2
The v0.2 release of Magpie exposes more of the framework approach that we intend Magpie to be.  In this release you will notice a few changes from v0.1.  Firstly, when pointing a browser at it, you will be now greeted with a familiar looking login screen.  The default credentials are `admin/VMware1!`.  After logging in, you will notice two applications now visible - the browser hosted PowerNSX session from v0.1 (Updated to PowerShell 6 GA, PowerCLI 10 and PowerNSX 3.0.1110) - and a new application, 'Object Explorer'.  See the Object Explorer section below for more info on this application.  You will also notice that the appliance exposes some settings.  This is still pretty basic we know, but again it should be a bit clearer where we are going with Magpie from whats initially exposed in here.  A bit more detail about the architecture of the appliance is discussed below.

## v0.1
Project Magpie v0.1 has functionality focussed only on providing a simple to install, multiuser PowerNSX appliance.  We have much bigger plans than that though, so check back regularly to see whats been added, as this is just the tip of the iceberg.

The following functionality is currently available:

# Functionality

Magpie is intended to be a modular appliance that we will add additional applications to over time.  As of v0.2, three such applications are exposed (there are several more that we are already prototyping as well).  PowerNSX from v0.1, Object Explorer, and the Settings application.


## Integrated User Accounts and Centralised NSX manager registration (settings)

As of Magpie v0.2, you can now register NSX manager details (ip/hostname, credentials) with Magpie.  Magpie includes a database and a simple user system (no RBAC yet!) to store and protect these details, which Magpie applications can then leverage.  (We intended PowerNSX to use this on v0.2 release but there were a few challenges to this so currently its user and NSX manager system is separate and still as it was in v0.1)

## NSX Object Explorer

From our experiences working with the VMware field organisation as well as our customers operations teams, one particular question frequently comes up when discussing operating NSX.  If I want to delete or modify an object in NSX, what other objects will it affect?  In large scale environments, this can be tough to answer with just the UI; enter Object Explorer.  Object Explorers reason for being is to allow you to search for NSX objects (Security Groups, Firewall Rules, IPSets etc) and to display _related objects_.  It is not intended to replace the UI or tools such as PowerNSX in showing you direct relationships (such as the static membership of a group, although it does do this) but also to show you in one place the effective membership of a security group (what VMs, VNICs, IPs, and MACs), _and_ the related objects (what rules and other groups the current group is used in!).  These details can be viewed as a list, and then iteratively searched on.  Object Explorer keeps a simple breadcrump of your iterative searchs, allowing you to backtrack and to build an understanding of the relationship of various objects.  While it is possible to piece all of this together with PowerNSX, now its possible in a simple UI.

## Access PowerNSX via a browser

Now you have no need to install PowerShell and PowerNSX/PowerCLI bits on multiple machines to support PowerNSX based operations.  Deploy the appliance once, create accounts for all users that require it and immediately access PowerNSX / PowerCLI from anywhere via a web browser.  Nothing to install on a client machine.  Now updated to PowerShell 6 GA, PowerCLI 10 and PowerNSX (latest, but as always you can update with `Update-Module PowerNSX -scope CurrentUser`)

## View the latest PowerNSX documentation

As of build 3.0.1054 of PowerNSX, we have started autogenerating our documentation to make it available online.  The Magpie appliance puts that at your fingertips via its documentation tab where you can access searchable PowerNSX documentation.

## Access PowerNSX via SSH

You can also access the same PowerNSX / PowerCLI environment via ssh for the same experience in a terminal via ssh.


# Usage

## Access the Magpie browser interface

Point a browser at `https://<appliance ip>`.  That was easy.  The default login is `admin/VMware1!`.  You can change it.

## Registering NSX Managers

Magpie allows you to register all your NSX managers if you wish.  For now at least, Magpie _does not allow for any modification_ of NSX, so this is effectively a read only scenario.  This will probably not remain the case in future versions though.

Registering an NSX manager allows the various apps that Magpie will play host to, to allow for the user to select a target environment from a dropdown.  Currently, only Object Explorer supports this but future applications will too.

A single NSX manager can be selected as the system wide default.

Register a New NSX Manager by clicking the gear in the RHS of the nav bar and select settings.

## Creating Users

Magpie has a simple auth system that protects both the UI and API.  There is not (yet) any RBAC, but we are planning on it in future versions.  When creating a new user, you can choose a default NSX manager that will be used in preference to the system default for that user only. (For the Nick works against test environment, Dale against prod scenario.)

Create a new user by clicking the gear in the RHS of the nav bar and select settings.

## Object Explorer Usage

Once you have registered an NSX manager, you can use the Object Explorer.  Its usage is pretty intuitive (we hope).  Enter a search term, type (contains, exact, regex), what type of object (the more you select here, the longer the initial uncached query takes), the property to search within (just name and objectid for now - this list will expand in time), and select the manager to execute the query against (if not the default)

Search results are shown in a series of 'collapsed' cards on the LHS of the window.  Click on a specific card for more info.  Counts of relationships will be visible for most object types (say Where Used: Security Groups: 5), and the count will be a link that you can click to display a list of the specific objects.

In most cases, the list that is displayed will contain objects that can also be searched for iteratively.  If this is the case, their name will be a link that you can click, that will then execute a search for that specific object, and the result displayed as a single expanded card.  You can then repeat the process ad infinitum.

You will notice that as you drill into relationships like this, that Object Explorer builds a breadcrumb of where you have been.  You can click the link of any of the previous objects you traversed to 'go back' to that point.

Regex support and Case Sensitivity can be leveraged as well to do some pretty powerful searching.

## PowerNSX Usage

The PowerNSX app is not yet integrated with Magpies user/manager system.  Like v0.1, you login to PowerNSX in the browser hosted shell.  The only valid account out of the box is the `powernsx` account (root credentials will not work here).  Login with its default password of `changeme` and you will be promtped to change it on first use.

### Access the PowerNSX / PowerCLI environment via ssh

Point an ssh client at port 2222 on the appliance.  Note that root logins will not work, only the powernsx and any other valid users created via the method below will work here.

### Create new users

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
- On first boot, occasionally the PowerNSX web console disconnects randomly (ssh is unaffected).  Reboot the appliance to resolve.
- Usernames are case sensistive.  Whoops.  Sorry about that, it wasn't intended, and will be resolved in v0.3.



# Architecture

Now that v0.2 is out, we can talk a bit about the intended architecture of Magpie.  These features were partly present in v0.1 in prototype form, but disabled during the build to keep you all guessing! :)

Magpie is intended to provide a few key things that we frequently found ourselves needing to do when working with PowerNSX and other NSX automation tools:

## Caching

Some of our customers are big.  So big that when you throw tools like PowerNSX at their environments (`Get-NsxSecurityGroup` for instance), which just do a blind get against the NSX API, they can take tens of minutes to return.  If you have to do that repeatedly then its painfully slow and in the worst cases, unusable.  

Magpie caches every call it does to the NSX API so that when you innevitably want to reuse those results to look for something else, response is returned from cache almost instantly.  Are there downsides? Sure. Magpie has no way to detect if changes have occured on the backend currently so stale results will occur.  If you know you are looking for something recently created, you can invalidate the cache from the UI.  The next search will then take some time while the cache is repopulated.

## API layer

To implement the caching layer, as well as a number of other normalisations and reporting style views of NSX, and to integrate the the user and NSX maanger registration sytem, we implemented a simple API abstraction layer within Magpie.  (Warning: This is not (yet) intended to be consumed directly by Magpie users.  Here be dragons.  If you do this, we dont apologise for completely rewriting it in future versions! It's currently pretty rudimentary and should not be considered a public API, but if you wanna dig here, knock yer socks off!)

This is written in python (flask) and allows us to keep all the NSX specific stuff hidden away from the UI.  Which is special.

We will continue to expand this API layer to cater for future Magpie applications.

## About Containers and Python and Angular and Clarity...

The curious amongst you (1% of the 8 people watching the repo! :)) might have noticed that v0.1 was just a photon instance with a docker container or two running on it.  With the included API layer and database now, Magpie consists of 4 separate containers running on the Photon appliance that implement the API, DB, UI (and rev proxy), and PowerNSX sandbox.  This is meaningless to the average user, but hey.  Containers right...! :)

The UI is written entirely in Angular (using the 'so awesome it even makes us CLI hacks look capable of 'designing' a UI' VMware Clarity framework) because its freakin cool.  And we needed a UI.  And Dale and I (Who had only heard of Angular an hour earlier) had a competition on the way home from VMworld Barcelona last year to see who could manage to actually make something work in it before we got back to Melbourne, which we both won, so we figured it was a good choice! :).  Thanks to Grant Orchard for putting us on the path to enlightenment there!

Oh yeah.  Python.  Once we worked out that we needed a caching layer, we realised that it was an obvious choice to write our own API and consume it from the UI.  Python was an obvious choice for us as we were already familiar with integrating with NSX using Python and could reuse a fair bit of code from elsewhere (actually, this ended up being almost completely untrue, but it was a valid justification back then!)  
For those that care, we are using Flask for caching and as the API framework, SQLAlchemy for db modelling and JWT for auth.  Massive shout out to Dale for his work on the SQLAlchemy, and JWT auth backend.  Magpie wouldnt exist without it!  

Finally, PowerNSX is just running in a cutdown Ubuntu environment and the whole Magpie app is fronted by an NGINX proxy in the UI container.


