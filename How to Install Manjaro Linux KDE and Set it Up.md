Example Scenario: Sole Windows user migrates from Windows 10 to Windows 10 + Manjaro

KDE(dual boot) on laptop You don’t need to stick to this. For 1000 people, there are 1000 Manjaros. 
If more explanation is needed, please contribute or refer to Google search. Contribution is always welcome. 

Before: backup important files, Firefox bookmarks(library->bookmarks->show all bookmarks->import and backup->backup) check one more time that nothing you need/will need is left behind and removed forever 

(for dual-booting Windows) first install Windows and configure: UEFI, Disk Management 
1. Download Manjaro Linux KDE from the official homepage and make the corresponding bootable USB with Rufus. 
2. Reboot and enter BIOS settings (e.g. Press F2). Check UEFI, Secure Boot. In this scenario, UEFI on & Secure Boot off. 
3. Install Manjaro Linux KDE with the graphical installer. [2]
4. Update & upgrade system up-to-date, check pamac settings, and use pamac as much as possible(to make sure the system is up-to-date(Manjaro: rolling release)), if you use pacman and you don’t know well about Manjaro, use -Syu option!
5. Backup your Manjaro account’s ID, PW, and root PW 
6. (Disable user feedbacks for privacy) 
7. gufw: make sure gufw auto-start on startup[7]
8. time: Manjaro uses UTC, Windows uses RTC. Make Windows use UTC and hit the synchronize button of Windows. 
9. Firefox settings: (refer to privacytools.io, Youtube videos for privacy +) restore your bookmarks 
10. Libreoffice writer: set your preferences(+ disable autocorrect-word completion and delete all the history for potential privacy threat)
11. set uim for multilanguage input support. uim byeoru works well for Korean input. And install fonts.[11]
12. keyboard shortcuts: remove the unnecessary 
13. vim settings: refer to Youtube video like: add_ide_features_to_vim, etc.
14. Encrypted DNS Settings 
15. Restore files from your backup storage 
16. battery: laptop lid/power button actions/screen light-out/suspend: sleep, shutdown, disable hibernation[16]
17. brightness & night color(5000K~6000K might be your sweet spot) 
18. Go through your KDE system settings[18]
19. Initial Setup is Done. You can change settings whenever you want!

appendix 1. how to use different keybind: a) backup ~/.Xmodmap b) edit ~/.Xmodmap(use xev to figure out the keycodes) c) check ~/.xinitrc to ensure that X11 starts with the changed .Xmodmap(cf. usermodmap, sysmodmap, if [ -f "$usermodmap" ]; then xmodmap "$usermodmap" fi) 
appendix 2. tldr, fortune would help a lot.
appendix 3. If you are going to use Joplin(open-source note-taking app), check https://github.com/laurent22/joplin and execute Joplin_install_and_update.sh

[2] 
\# Manual Partitioning
EFI, swap, root, home
EFI: 512MiB, FAT32, /boot/efi, boot
swap: 8602MiB(as you want), linuxswap, swap
root: 65536MiB, ext4, /, root
home: ext4, /home
\# after installation, before reboot, type efibootmgr command into the terminal and check BootOrder. If not correct, go into BIOS settings and fix it.

[7] 
(sudo) systemctl enable ufw will create the symlink

[11]
for Hangul(Korean) input:
\# download uim
\# $ vim ~/.xprofile
export GTK_IM_MODULE=uim
export QT_IM_MODULE=uim
uim-xim &
export XMODIFIERS=@im=uim
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
Things to do after installing Manjaro on Google, Youtube, ...
Privacy Settings: privacytools.io, Youtube videos, ...**

