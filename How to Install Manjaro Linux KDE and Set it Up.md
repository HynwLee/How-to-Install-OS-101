Example Scenario: Sole Windows user migrates from Windows 10 to Windows 10 + Manjaro

KDE(dual boot) on laptop You don’t need to stick to this. For 1000 people, there are 1000 Manjaros. 
If more explanation is needed, please contribute or refer to Google search. Contribution is always welcome. 

Before: backup important files, Firefox bookmarks(library->bookmarks->show all bookmarks->import and backup->backup) check one more time that nothing you need/will need is left behind and removed forever 

(for dual-booting Windows) first install Windows and configure: UEFI, Disk Management 
1. Download Manjaro Linux KDE from the official homepage and make the corresponding bootable USB with Rufus. 
2. Reboot and enter BIOS settings (e.g. Press F2). Check UEFI, Secure Boot. In this scenario, UEFI on & Secure Boot off. 
3. Install Manjaro Linux KDE with the graphical installer. 
4. Update & upgrade system up-to-date, check pamac settings, and use pamac as much as possible(to make sure the system is up-to-date(Manjaro: rolling release)), if you use pacman and you don’t know well about Manjaro, use -Syu option!
5. Backup your Manjaro account’s ID, PW, and root PW 
6. (Disable user feedbacks for privacy) 
7. gufw: make sure gufw auto-start on startup[1]
8. time: Manjaro uses UTC, Windows uses RTC. Make Windows use UTC and hit the synchronize button of Windows. 
9. Firefox settings: (refer to privacytools.io, Youtube videos for privacy +) restore your bookmarks 
10. Libreoffice writer: set your preferences(+ disable autocorrect-word completion and delete all the history for potential privacy threat)
11. set uim for multilanguage input support. uim byeoru works well for Korean input. 
12. keyboard shortcuts: remove the unnecessary 
13. vim settings: refer to Youtube video like: add_ide_features_to_vim, etc.
14. Encrypted DNS Settings 
15. Restore files from your backup storage 
16. battery: laptop lid/power button actions/screen light-out/suspend: sleep, hibernate, shutdown 
17. brightness & night light(5000K~6000K might be your sweet spot) 
18. appearances: wallpapers, resolution, font size 19. Go through your KDE system settings 
19. Initial Setup is Done. You can change settings whenever you want!

[1] (sudo) systemctl enable ufw will create the symlink

*Further Helps Search: 
Things to do after installing Manjaro on Google, Youtube, ...
Privacy Settings: privacytools.io, Youtube videos, ...
