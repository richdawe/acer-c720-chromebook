acer-c720-chromebook
====================

Setting Up The Keyboard Map
---------------------------

Based on
http://www.reddit.com/r/chrubuntu/comments/1rsxkd/list_of_fixes_for_xubuntu_1310_on_the_acer_c720/ch74rbg
, which makes the keyboard a lot more usable (Home, End, PgUp and PgDown!):

    git clone https://github.com/dnschneid/crouton.git
    cd targets
    sudo bash keyboard

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

