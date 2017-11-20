---
layout: post
title :  'Write to NTFS Partitions in OS X 10.11 El Capitan'
date : 2017-11-20 17:00:00 +07:00
tags : [mac]
categories : [scrap]
---

NTFS support in OS X is disappointing. You plug in a USB flash drive from a co-worker who uses Windows to simply copy over a file, only to realize that you can't actually write to NTFS-formatted drives on Mac out of the box.
### Flash Drive
That's kind of lame, to be honest. It's 2016, Apple, wake up! People who work together in the same office, on different operating systems, should be able to exchange files via USB flash drive with ease, without having to worry about formatting their drives with a cross-platform filesystem, like exFAT.
### Solutions
There are a few ways to enable NTFS write support on OS X.> 1.Using the built-in NTFS drivers - Writing to NTFS drives is a functionality that's been built into OS X for some time. However, it's disabled by default for NTFS volumes, and for good reason. It's extremely buggy and corrupts entire volumes in certain situations. I tried copying 4GB worth of data to an NTFS volume this way, and the transfer just failed half-way, rendering the disk inaccessible and my computer unbootable, until I manually disconnected the SATA cable from the hard drive. Luckily, hot-plugging the drive to Windows 7 and running dskchk fixed the issue, but other people reported losing all of their data because of these faulty drivers. Therefore, I strongly advise against this method. 
> 2.Purchasing Tuxera/Paragon NTFS for Mac - A viable option, albeit quite expensive (Tuxera - $26.50, Paragon - $19.95), at least for something that should already be included with the OS. I shouldn't have to pay good money to be able to write to an NTFS volume. There has to be another way. 
> 3.Downloading and installing OSXFUSE and NTFS-3G - The winning option. NTFS-3G is an NTFS read/write driver that is free and open-source, and there don't seem to be any corruption issues arising from using it.
### Installing OSXFUSE
So, what is OSXFUSE anyway?
FUSE for OS X allows you to extend OS X's native file handling capabilities via third-party file systems. As a user, installing the OSXFUSE software package will let you use any third-party file system written atop OSXFUSE or MacFUSE, if you choose to install the MacFUSE compatibility layer.
So basically, it's a way for developers to extend OS X's native file handling APIs to other file systems. 
It's required by NTFS-3G, so let's go ahead and install the latest OSXFUSE (3.x.x) from here: [https://github.com/osxfuse/osxfuse/releases](https://github.com/osxfuse/osxfuse/releases)
Download the latest ` osxfuse-3.x.x.dmg ` attachment, mount it, and install it, as with any other ` .dmg` .
** Note **: Make sure to select the ** MacFUSE Compatibility Layer ** in the installation options. NTFS-3G depends on it.
### Installing NTFS-3G
Once that's done, we can go ahead and install NTFS-3G. But not so fast, we need Homebrew for that. 
Verify that you have Homebrew installed by running:
```no-highlight
brew -v
```
If you don't, install it using this one-liner:
```no-highlight
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
In any case, go ahead and update the Homebrew formula:
```no-highlight
brew update
```
Next, let's finally install NTFS-3G via Homebrew:
```no-highlight
brew install homebrew/fuse/ntfs-3g
```
### Replacing Mount_NTFS
To get NTFS-3G to work, we need to replace the built-in `/sbin/mount_ntfs` binary, which is linked to Apple's NTFS driver, with NTFS-3G's `mount_ntfs`.
This was a pretty easy thing to do before OS X 10.11 El Capitan, but due to System Integrity Protection, it is now slightly harder.
> System Integrity Protection is a security technology in OS X El Capitan that's designed to help prevent potentially malicious software from modifying protected files and folders on your Mac. 
It restricts the root account and limits the actions that the root user can perform on protected parts of OS X.
Therefore, we can't simply swap out the binary by using `sudo` and running `ln`. That is, not until we disable System Integrity Protection.
### Disabling SIP
Reboot your Mac, and hold ** Cmd + R ** to enter Recovery mode. Within the Terminal, enter the following command to disable SIP and reboot back into OS X:
```no-highlight
csrutil disablereboot
```
### Replacing the Binary Link
Boot into OS X, and open a Terminal window. Within the Terminal, enter the following commands to swap out the link to the native `mount_ntfs` binary with a link to NTFS-3G's `mount_ntfs` binary:
```no-highlight
sudo mv /sbin/mount_ntfs /sbin/mount_ntfs.original
sudo ln -s /usr/local/sbin/mount_ntfs /sbin/mount_ntfs
```
Terminal Mount NTFS Commands
### Re-enabling SIP
Reboot into Recovery mode again, this time, to re-enable SIP. Within the Terminal, enter the following command to enable SIP and reboot back into OS X:
```no-highlight
csrutil enable
reboot
```
### That's It!
NTFS-3G will automatically mount your NTFS volumes in read-and-write mode once you reboot back into OS X. Go ahead and copy some files over to an NTFS volume to make sure it works. Well done!
