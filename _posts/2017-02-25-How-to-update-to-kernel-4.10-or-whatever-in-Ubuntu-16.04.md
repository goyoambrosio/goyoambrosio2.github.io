---
title: "How to update to kernel 4.10 or whatever in Ubuntu 16.04"
related : true
categories:
  - HowTo
tags: 
  - ubuntu
  - kernel
  - install
---

The first question is whether it's worth upgrading to this version of the kernel,
and the answer is that unless there is something that you need from the new
version, then not really.

But if you continue with the effort to do so, your life will remain happy
because you could return to your earlier version if something fails.

To update the kernel follow these instructions

### Short path

Check the latest or desired version in 

    http://kernel.ubuntu.com/~kernel-ppa/mainline/

Now, install it. For example, if your selected version is 4.10.0:

    sudo apt-get install linux-image-4.10.0
    
And that's it for the short version of these instructions. 

### Long path

For the long version, and more understandable,  follow next steps instead.

Download the next packages according to your OS type (*i386* for 32-bit, *amd64* for 64-bit)

        wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10/linux-headers-4.10.0-041000_4.10.0-041000.201702191831_all.deb
        wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10/linux-headers-4.10.0-041000-generic_4.10.0-041000.201702191831_amd64.deb
        wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10/linux-image-4.10.0-041000-generic_4.10.0-041000.201702191831_amd64.deb
        

Install them

        sudo dpkg -i *.deb
        
Update grub

        sudo update-grub

Restart your computer after installation to apply changes.

        sudo reboot

You can easily switch back to the previous kernel by restart your machine and
select boot with old kernel version (available in Advanced options). Then use
Ubuntu-Tweak or follow [this guide](http://ubuntuhandbook.org/index.php/2016/05/remove-old-kernels-ubuntu-16-04/) to remove Kernel 4.10.
