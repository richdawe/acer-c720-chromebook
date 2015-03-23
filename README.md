acer-c720-chromebook
====================

TODO: Initial install of USB DVD drive
TODO: Kernel parameters for first boot
TODO: Kernel parameters in grub config
TODO: Mainline kernel usage
TODO: laptop-mode-tools install

Setting Up The Keyboard Map
---------------------------

Based on
http://www.reddit.com/r/chrubuntu/comments/1rsxkd/list_of_fixes_for_xubuntu_1310_on_the_acer_c720/ch74rbg
, which makes the keyboard a lot more usable (Home, End, PgUp and PgDown!):

    git clone https://github.com/dnschneid/crouton.git
    cd targets
    sudo bash keyboard

TODO: Incorporate keyboard2

Note that the "keyboard" script from Crouton will output
a number of alarming-looking commands to the terminal, setting various
default options, but those commands are not actually run!
The script should only run the commands necessary to set up the xkb file
for the "chromebook" keyboard.

Once that script has run, copy 98xkb-chromebook into place:

    cp 98xkb-chromebook /etc/X11/Xsession.d

and then reboot.

After that, the following keyboard combinations will work
(copied from http://www.reddit.com/r/chrubuntu/comments/1rsxkd/list_of_fixes_for_xubuntu_1310_on_the_acer_c720/ch74rbg ):

 * "search"+F1 = XF86Back
 * "search"+F2 = XF86Forward
 * "search"+F3 = XF86Reload
 * "search"+F4 = XF86Display
 * "search"+F5 = XF86ApplicationRight
 * "search"+F6 = XF86MonBrightnessDown
 * "search"+F7 = XF86MonBrightnessUp
 * "search"+F8 = XF86AudioMute
 * "search"+F9 = XF86AudioLowerVolume
 * "search"+F10 = XF86AudioRaiseVolume
 * "search"+1,2,3,4,5,6,7,8,9,0 = F1,F2,F3,F4,F5,F6,F7,F8,F9,F10 ( this is additional F keys, native F keys working as usual)
 * "search"+minus = F11
 * "search"+equal = F12
 * "search"+backspace = Del
 * "search"+period = Ins (second key from the right shift)
 * "search"+left = Home
 * "search"+right = End
 * "search"+up = PgUP
 * "search"+down = PgDown
 * "search"+Alt = Capslock

Set up compressed swap in RAM (zram)
------------------------------------

Having swap on an SSD is a bad idea in terms of wearing out the SSD.

There's a feature called "zram" where some of the RAM is dedicated
as swap memory, and the swap is compressed into RAM. This can also be faster
than storing swap on an HDD. The trade off is CPU vs. disk access time
for HDDs.

First install it:

    apt-get install zram-config

Then disable the swap disk in /etc/fstab. On my machine running
Ubuntu 14.04 LTS x86_64 I commented out:

    #/dev/mapper/ubuntu--vg-swap_1 none            swap    sw              0       0

Then reboot for everything to take effect.

After that you may wish to free up the Logical Volume (LV)
that is set up by the Ubuntu installer for swap. I did this online;
you may wish to do it via a rescue disk. You may find
https://help.ubuntu.com/community/ResizeEncryptedPartitions#Enlarge_an_encrypted_partition
helpful.

First, make a backup!

Then switch to the first virtual console using Ctrl+F1, and switch to root:

    sudo -s
    su -

See what Logical Volumes are defined:

    lvs

Remove the swap LV and resize your root LV to the whole of the Physical Volume.
Then resize the ext3 or ext4 filesystem ONLINE:

    lvremove ubuntu-vg/swap_1
    lvresize ubuntu-vg/root +100%FREE
    resize2fs /dev/mapper/ubuntu--vg--root
    touch /forcefsck

and then reboot.

Note: Online resizing of ext4 filesystems is supported in Linux 3.x.

TODO:
  Try shrinking the zram usage to 1/4 RAM (i.e.: 512 MB RAM) to see
  if that still works OK.

ChromeOS Touchpad Driver for Linux
==================================

Hugegreenbug ported the ChromeOS touchpad driver to Linux.
This gives a much greater responsiveness under Linux,
and is well worth installing. Links:

  https://github.com/hugegreenbug/xf86-input-cmt
  https://www.reddit.com/r/chrubuntu/comments/2zb9ao/news_chromeos_mouse_driver_for_linux_version_2/

Instructions to install it:

  sudo apt-add-repository ppa:hugegreenbug/cmt2
  sudo apt-get update
  sudo apt-get install xf86-input-cmt
  # From instructions in Github:
  sudo mv /usr/share/X11/xorg.conf.d/50-synaptics.conf /usr/share/X11/xorg.conf.d/50-synaptics.conf.out-of-way
  sudo sync && sudo sync && sudo reboot

