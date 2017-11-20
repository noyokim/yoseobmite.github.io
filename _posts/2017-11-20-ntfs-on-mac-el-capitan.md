---
title :  'Write to NTFS Partitions in OS X 10.11 El Capitan'
date : 2017-11-20 17:00:00 +07:00
tags : [mac]
categories : [scrap]
---

NTFS support in OS X is disappointing. 
You plug in a USB flash drive from a co-worker who uses Windows to simply copy over a file, only to realize that you can't actually write to NTFS-formatted drives on Mac out of the box.
That's kind of lame, to be honest. It's 2016, Apple, wake up! People who work together in the same office, on different operating systems, should be able to exchange files via USB flash drive with ease, without having to worry about formatting their drives with a cross-platform filesystem, like exFAT.
Installing OSXFUSESo, what is OSXFUSE anyway?FUSE for OS X allows you to extend OS X's native file handling capabilities via third-party file systems. As a user, installing the OSXFUSE software package will let you use any third-party file system written atop OSXFUSE or MacFUSE, if you choose to install the MacFUSE compatibility layer.So basically, it's a way for developers to extend OS X's native file handling APIs to other file systems. It's required by NTFS-3G, so let's go ahead and install the latest OSXFUSE (3.x.x) from here: https://github.com/osxfuse/osxfuse/releases

Verify that you have Homebrew installed by running:
---c
brew -v
---
If you don't, install it using this one-liner:
---c
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
---


Next, let's finally install NTFS-3G via Homebrew:

---c
brew install homebrew/fuse/ntfs-3g
---

Replacing Mount_NTFSTo get NTFS-3G to work, we need to replace the built-in /sbin/mount_ntfs binary, which is linked to Apple's NTFS driver, with NTFS-3G's mount_ntfs.This was a pretty easy thing to do before OS X 10.11 El Capitan, but due to System Integrity Protection, it is now slightly harder.System Integrity Protection is a security technology in OS X El Capitan that's designed to help prevent potentially malicious software from modifying protected files and folders on your Mac. It restricts the root account and limits the actions that the root user can perform on protected parts of OS X.Therefore, we can't simply swap out the binary by using sudo and running ln. That is, not until we disable System Integrity Protection.Disabling SIPReboot your Mac, and hold Cmd + R to enter Recovery mode. Within the Terminal, enter the following command to disable SIP and reboot back into OS X:

---c
csrutil disable
reboot
---

Replacing the Binary LinkBoot into OS X, and open a Terminal window. Within the Terminal, enter the following commands to swap out the link to the native mount_ntfs binary with a link to NTFS-3G's mount_ntfs binary:


--c
sudo mv /sbin/mount_ntfs /sbin/mount_ntfs.original
sudo ln -s /usr/local/sbin/mount_ntfs /sbin/mount_ntfs

---

### That's It!
NTFS-3G will automatically mount your NTFS volumes in read-and-write mode once you reboot back into OS X. Go ahead and copy some files over to an NTFS volume to make sure it works. Well done!

