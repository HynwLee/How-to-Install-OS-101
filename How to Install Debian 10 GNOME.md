Example Scenario: Sole Windows user migrates from Windows 10 to Windows 10 + Debian GNOME

GNOME(dual boot) on laptop(Dell Inspiron 7559)

You don’t need to stick to this. For 1000 people, there are 1000 Debians. 
If more explanation is needed, please contribute or refer to Google search. Contribution is always welcome. 

Before: backup important files, Firefox bookmarks(library->bookmarks->show all bookmarks->import and backup->backup) check one more time that nothing you need/will need is left behind and removed forever 

(for dual-booting Windows) first install Windows and configure: UEFI + GPT, Disk Management
 
1. Download Debian Linux GNOME from the official homepage and make the corresponding bootable USB with Rufus. 
2. Reboot and enter BIOS settings (e.g. Press F2). Check UEFI, Secure Boot. In this scenario, UEFI on & Secure Boot off. 
3. Install Debian Linux GNOME with the graphical installer. If wifi doesn't work, then use a LAN cable. [3]
4. If the computer won't boot-only letters in black screen(e.g. NMI watchdog: BUG: soft lockup - CPU#n stuck for 23s!) then blacklist nouveau. GRUB editor -> add nouveau.modeset=0 to the end of the line which begins with "Linux" -> F10 to boot -> install nvidia driver(7.). (Dell Inspiron 7559)If you want to make wifi work, install firmware-iwlwifi.
5. Software & Updates -> Other Software -> uncheck cdrom:~. ->Debian Software -> check all the 4 items(main, contrib, non-free, Source code) and use the fastest server by: Choose a Download Server -> Select Best Server. Don't Reload and Close. sudo apt update and check if there's an Err. If so, edit /etc/apt/sources.list -> deb http://security.debian.org/debian-security buster/updates main contrib non-free deb-src http://security.debian.org/debian-security buster/updates main contrib non-free. << change the 3rd, 4th lines. Finally, install Synaptic Package Manager for convenience.
6. add the user to sudoers by: visudo /etc/sudoers -> add: YourUserName   ALL=(ALL:ALL) ALL and then backup your Debian account’s ID, PW, and root PW
7. sudo apt install nvidia-detect -> sudo nvidia-detect -> sudo apt install nvidia-driver -> assert the system reboots.
8. sudo apt install build-essential dkms linux-headers-$(uname --kernel-release)
9. sudo apt install synaptic
10. $ ps -e | grep -Ei "x|way" and if Debian is using Wayland, then logout and select System X11 Default(Wayland doesn't support Synaptic Package Manager at the time of writing this)
11. (Disable user feedbacks for privacy) [11]
12. install Intel/AMD microcode
13. (g)ufw: make sure (g)ufw auto-start on startup[13]
14. install VLC
15. time: Debian uses UTC, Windows uses RTC. Make Windows use UTC and hit the synchronize button of Windows. 
16. Firefox settings: restore your bookmarks [16]
17. Libreoffice writer: set your preferences(+ disable autocorrect-word completion and delete all the history for potential privacy threat)
18. set uim for multilanguage input support. uim byeoru works well for Korean input. And install fonts.[18]
19. keyboard shortcuts: remove the unnecessary 
20. vim settings: customize .vimrc and use great plugins!
21. Encrypted DNS Settings 
22. Restore files from your backup storage[22]
23. battery: laptop lid/power button actions/screen light-out/suspend: sleep, shutdown, disable hibernation[23]
24. brightness & night light(5000K~6000K might be your sweet spot): 00:00 to 23:59 for always -> dconf-editor -> /org/gnome/settings-daemon/plugins/color/night-light-temperature -> change Custom value
25. Go through your GNOME system settings[25]
26. Install fd, rg and add alias fd=fdfind in .bashrc file, add welcoming messages[26]
27. To disable bluetooth on startup, install rfkill -> create /lib/systemd/system/disablebluetooth.service -> 
[Unit]
Description=Disable Bluetooth

[Service]
Type=oneshot
ExecStart=/usr/sbin/rfkill block bluetooth

[Install]
WantedBy=multi-user.target
-> sudo systemctl enable disablebluetooth.service
28. add terminal shortcut: Settings -> Devices -> Keyboard -> Command gnome-terminal, Shortcut Ctrl + Alt + T (e.g.)
29. 3rd-party programs: use programs in the official repository as much as possible, and if you can't avoid it, then at least isolate in your home directory. For stability, use programs in the official repositories. If you need newer ones or missing ones that you can't get from there, consider: 1) Debian Backports 2) Flatpaks 3) AppImages 4) isolate 3rd-party programs in your home directory
30. (Install 7-zip, veracrypt, ...) 
31. (Install a password manager)
32. Remove bloats: I mean, GNOME games.
33. Initial Setup is Done. You can change settings whenever you want! For stability, search on the internet for the latest information.

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

appendix 3. chrome-sandbox issue(Electron): 
If you are going to use Joplin(open-source note-taking app): ./Joplin.AppImage --appimage-extract -> replace the included chrome-sandbox with a symlink to computer's chrome-sandbox(/usr/lib/chromium/chrome-sandbox) -> download appimagetool and chmod u+x -> repack Joplin by ARCH=x86_64 ./appimagetool-x86_64.AppImage to-repack-dir ~/.joplin/Joplin.AppImage
If the icon for an AppImage does not show up on the GNOME task bar, then one solution can be making an icon, which executes the execution command on a terminal.
\[procedure]: 
1) vim ~/.local/share/applications/{appname}.desktop
2) write something like this:
\[Desktop Entry]
Name={appname} Execution
Icon=~/.local/share/icons/{appname}
Exec=bash -c '~/.bitwarden/{appname}.AppImage'
Terminal=false
Type=Application
3) move the wanted Icon file with this command: ~/.local/share/icons/{appname}
This is necessary because of Electron, which is used by Joplin and so on. 

appendix 4. customize clipboard example: right-click clipboard -> configure clipboard -> uncheck Save clipboard contents on exit, check Prevent empty clipboard/Ignore images/Ignore selection and change Clipboard history size to 5.

appendix 5. Pain-in-the-ass problem: Dual-booting scheme is highly likely to be screwed up if you reinstall Windows 10 for some reason. GRUB will disappear! 
Cause: Windows 10 formatted the EFI partition, and thus, its UUID has changed. 
Solution: Step 1(not sure is this a must): make a Debian installation(NetInst version is fine) medium on a USB -> boot with the USB -> Graphical Rescue Mode -> select your system's root -> open a shell terminal ->
sudo mount /dev/sd[root] /mnt
sudo mount /dev/sd[efi] /mnt/boot/efi
for i in /dev /dev/pts /proc /sys /run; do sudo mount --bind $i /mnt$i; done
sudo chroot /mnt
grub-install /dev/sd[efi]
update-grub
Step 2(it's a MUST): update the UUID of the EFI partition. After step 1, you can boot Debian, but [DEPEND] Dependency failed for /boot/efi error will show up. And then there will be a tty for you. Right there, you should do the followings:
\# blkid stands for block identification
sudo blkid
Jot down the UUID of the EFI partition
\# fstab stands for file systems table
sudo nano /etc/fstab
-> update the very UUID
Hopefully, it will work again.

appendix 6. Sometimes, headphones do not work (maybe due to kernel issues). In that case, try (sudo) alsactl init. It might work well again.

[3] 
\# Manual Partitioning Example
EFI, swap, root, home
EFI: 537.9MB, ESP, B
root: 64GB, ext4, /, root
swap: 8.4GB(as you want), linuxswap, swap
home: All of the left, ext4, /home

\# after installation, go into BIOS settings and fix it.

[11]
Workspace Behavior -> Activities -> Privacy: Keep history: "as you want"

[13] 
(sudo) systemctl enable ufw will create the symlink

[16]
\# Preferences

\# about:config
network.security.esni.enabled -> true
network.trr.bootstrapAddress -> 1.1.1.1
browser.urlbar.speculativeConnect.enabled -> false
privacy.resistFingerprinting -> true
privacy.firstparty.isolate -> true
media.peerconnection.enabled -> false

\# plugins
uBlock Origin -> *$font,third-party
HTTPS Everywhere -> on
Decentraleyes
...

cf. privacytools 

[18]
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
noto-fonts-cjk(-extra)?
fonts-baekmuk
fonts-alee
adobe-source-han-sans-kr-fonts
adobe-source-han-serif-kr-fonts
and many more

[22]
backup your system manually or using backup program(s)

[25] 
\# install tlp, and then systemctl enable tlp --now (if tlp's version is 1.2.2 or lower, tlp needs tlp-sleep.service also.)

\# (sudo) vim /etc/systemd/sleep.conf
AllowHibernation=no
AllowSuspendThenHibernate=no

[26]
\# welcome messages example
echo "Exercise/Stretch/Blink/..." for your health



