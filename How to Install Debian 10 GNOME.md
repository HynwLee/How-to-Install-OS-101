Example Scenario: Sole Windows user migrates from Windows 10 to Windows 10 + Debian GNOME

GNOME(dual boot) on laptop(Dell Inspiron 7559)

You don’t need to stick to this. For 1000 people, there are 1000 Debians. 
If more explanation is needed, please contribute or refer to Google search. Contribution is always welcome. 

Before: backup important files, Firefox bookmarks(library->bookmarks->show all bookmarks->import and backup->backup) check one more time that nothing you need/will need is left behind and removed forever 

(for dual-booting Windows) first install Windows and configure: UEFI, Disk Management 
1. Download Debian Linux GNOME from the official homepage and make the corresponding bootable USB with Rufus. 
2. Reboot and enter BIOS settings (e.g. Press F2). Check UEFI, Secure Boot. In this scenario, UEFI on & Secure Boot off. 
3. Install Debian Linux GNOME with the graphical installer. If wifi doesn't work, then use a LAN cable. [3]
1. If the computer won't boot-only letters in black screen-and you are using nvidia graphics card, then blacklist nouveau. add modprobe.blacklist=nouveau to "linux" entry when you boot. And boot using recovery mode, then add the same line to /etc/default/grub then update-grub (root privilege might be needed). After doing the next 2 steps, sudo apt install nvidia-detect, sudo nvidia-detect, sudo apt install nvidia-driver. Also, install firmware-iwlwifi(might be different from device to device) using Synaptic Package Manager.  
4. Software & Updates -> Other Software -> uncheck cdrom:~. ->Debian Software -> check all the 4 items(main, contrib, non-free, Source code) and use the fastest server by: Choose a Download Server -> Select Best Server. Don't Reload and Close. sudo apt update and check if there's an Err. If so, edit /etc/apt/sources.list -> deb http://security.debian.org/debian-security buster/updates main contrib non-free deb-src http://security.debian.org/debian-security buster/updates main contrib non-free. << change the 3rd, 4th lines. Finally, install Synaptic Package Manager for convenience.
1. add the user to sudoers by: sudo vim /etc/sudoers -> add: YourUserName   ALL=(ALL:ALL) ALL and then backup your Debian account’s ID, PW, and root PW
1. sudo apt install build-essential dkms linux-headers-$(uname --kernel-release)
5. $ ps -e | grep -Ei "x|way" and if Debian is using Wayland, then logout and select System X11 Default
6. (Disable user feedbacks for privacy) 
7. gufw: make sure gufw auto-start on startup[7]
8. time: Debian uses UTC, Windows uses RTC. Make Windows use UTC and hit the synchronize button of Windows. 
9. Firefox settings: (refer to privacytools.io, Youtube videos for privacy +) restore your bookmarks [9]
10. Libreoffice writer: set your preferences(+ disable autocorrect-word completion and delete all the history for potential privacy threat)
11. set uim for multilanguage input support. uim byeoru works well for Korean input. And install fonts.[11]
12. keyboard shortcuts: remove the unnecessary 
13. vim settings: refer to Youtube video like: add_ide_features_to_vim, etc.
14. Encrypted DNS Settings 
15. Restore files from your backup storage 
16. battery: laptop lid/power button actions/screen light-out/suspend: sleep, shutdown, disable hibernation[16]
17. brightness & night light(5000K~6000K might be your sweet spot) 
18. Go through your GNOME system settings[18]
19. Initial Setup is Done. You can change settings whenever you want!

appendix 1. how to use different keybind: 
a) xmodmap -pke > ~/.Xmodmap b) edit ~/.Xmodmap(use xev to figure out the keycodes) c) check ~/.xinitrc to ensure that X11 starts with the changed .Xmodmap(cf. usermodmap, sysmodmap, if [ -f "$usermodmap" ]; then xmodmap "$usermodmap" fi) 

if these don't work(my GNOME didn't work), then:
edit 
    /usr/share/X11/xkb/symbols/keypad
    /usr/share/X11/xkb/symbols/us
    /usr/share/X11/xkb/symbols/kr
    ...
instead of ~/.Xmodmap 

appendix 2. tldr, fortune would help a lot.

appendix 3. If you are going to use Joplin(open-source note-taking app), check https://github.com/laurent22/joplin and execute Joplin_install_and_update.sh

appendix 4. customize clipboard example: right-click clipboard -> configure clipboard -> uncheck Save clipboard contents on exit, check Prevent empty clipboard/Ignore images/Ignore selection and change Clipboard history size to 5.

[3] 
\# Manual Partitioning
EFI, swap, root, home
EFI: 537.9MB, ESP, B
root: 64GB, ext4, /, root
swap: 8.4GB(as you want), linuxswap, swap
home: All of the left, ext4, /home

\# after installation, go into BIOS settings and fix it.

[7] 
(sudo) systemctl enable ufw will create the symlink

[9]

\# Preferences

\# about:config
network.security.esni.enabled -> true
network.trr.bootstrapAddress -> 1.1.1.1
browser.urlbar.speculativeConnect.enabled -> false
privacy.resistFingerprinting -> true
privacy.firstparty.isolate -> true
media.peerconnection.enabled -> false

\# plugins
uBlock Origin
HTTPS Everywhere
Cookie Autodelete
Decentraleyes
...

[11]
for Hangul(Korean) input:

\# download uim

\# $ vim ~/.profile
export GTK_IM_MODULE=uim
export QT_IM_MODULE=uim
uim-xim &
export XMODIFIERS=@im=uim
\#\# below are useless if GNOME doesn't use xmodmap. Then, open dconf editor and go into
\#\# /org/gnome/desktop/input-sources/xkb-options. Replace Custom value with ['korean:ralt_hanja']
\#\# Finally, uim-pref-gtk -> Byeoru key bindings 1 -> [Byeoru] on / off -> add "hangul-hanja" and for [Byeoru] convert Hangul to Chinese characters, use "<Shift>hangul-hanja" instead.
xmodmap -e 'remove mod1 = Alt_R'
xmodmap -e 'keycode 108 = Hangul'
xmodmap -e 'remove control = Control_R'
xmodmap -e 'keycode 105 = Hangul_Hanja'

\# uim-pref-gtk
check Specify default IM, Default input method&Enabled input methods -> leave only Byeoru, "Byeoru" on each.
uncheck Enable IM switching by hotkey
uncheck Enable input method toggle by hot keys
in Byeoru key bindings 1 menu,
[Byeoru] on "hangul"
[Byeoru] off "hangul"
[Byeoru] convert Hangul to Chinese characters "hangul-hanja"

\# install fonts
noto-fonts-cjk
adobe-source-han-sans-kr-fonts
adobe-source-han-serif-kr-fonts

[16] 

\# install tlp, and then systemctl enable tlp --now (if tlp's version is 1.2.2 or lower, tlp needs tlp-sleep.service also.)

\# (sudo) vim /etc/systemd/sleep.conf
AllowHibernation=no
AllowSuspendThenHibernate=no

[18]
Workspace Behavior -> Activities -> Privacy: Keep history: "as you want"


**Further Helps Search: 
Things to do after installing Debian on Google, Youtube, ...
Privacy Settings: privacytools.io, Youtube videos, ...**

