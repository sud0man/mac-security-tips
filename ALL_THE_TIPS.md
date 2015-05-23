Note : if $ xxx => command xxx to launch, else => file or directory to dump.

## ** FORENSICS - SYSTEM INFO AND LEAK INFO ** ##
###  PROOF OF CONCEPT ###
  * Pac4Mac: https://code.google.com/p/pac4mac/
###  SYSTEM INFO ###
**General information
```
$ System_profiler
```**Owner  (name, address, tel, etc.)
```
/Users/<USERNAME>/Library/Preferences/AddressBookMe.plist
/Library/Preferences/AddressBookMe.plist
/private/var/db/.AppleSetupDone
```
**Kernel Version and state
```
/System/Library/PreferencePanes/Ink.prefPane/Contents/Info.plist
$ sysctl -A
```**OS version
```
/System/Library/PreferencePanes/Ink.prefPane/Contents/Info.plist
/System/Library/CoreServices/SystemVersion.plist
/System/Library/CoreServices/ServerVersion.plist (if server)
$ uname -an
```
**Timezone
```
/Library/Preferences/.GlobalPreferences.plist
/etc/localtime
```**

###  AUTHENTICATION DATA ###
**Usernames and password hashes
```
/Users/<USERNAME>
[10.6]/var/db/shadow/hash/
[10.7]/private/var/db/dslocal/nodes/Default/users/<USERNAME>.plist
[10.8]/private/var/db/dslocal/nodes/Default/users/<USERNAME>.plist
```**

**Administrators
```
/var/db/dslocal/nodes/Default/groups/admin.plist
```**

**Autologin password (XOR)
```
/private/etc/kcpassword
```**

**Last connected user
```
/Library/Preferences/com.apple.loginwindow.plist
```**Last Login Info + Hint master password + autologin user
```
/Library/Preferences/com.apple.loginwindow.plist
```
**Deleted Users
```
/Library/Preferences/com.apple.preferences.accounts.plist
```**

**User Keychain (contains a lot of passwords :))
```
/Users/<USERNAME>/Library/Keychains/login.keychain
```**

**System Keychain
```
/Library/Keychains/FileVaultMaster.keychain => contains the FileVault Recovery Key to use master password
/Library/Keychains/System.keychain
/Library/Keychains/applepushserviced.keychain
/var/db/SystemKey => contains key to decrpyt System.Keychain
```**


###  ALL LOGS ###
```
/var/log/system.log*
/var/log/windowserver.log*
/var/log/secure.log*
/var/log/kernel.log*
/private/var/log/install.log*
/private/var/log/appfirewall.log*
/var/audit/*
```



###  PERSISTENCE ###
**XPC Services
```
$ find /Applications/ -name XPCServices -exec ls -lsct {} \;
```**Launched XPC System
```
/System/Library/XPCServices/
```
**Launched Agents System
```
/System/Library/LaunchAgents/
```**Launched Agents Library
```
/Library/LaunchAgents/
```
**Launched Daemons System
```
/System/Library/LaunchDaemons/
```**Launched Daemons Library
```
/Library/LaunchDaemons/
```
**Launched LoginItems User
```
/Users/<USERNAME>/Library/Preferences/com.apple.loginitems.plist
```**Launched LoginItems Application
```
$ find /Applications/ -name LoginItems -exec ls -lsct {} \;
```
**Launched ScriptingAdditions
```
/System/Library/ScriptingAdditions/
/Library/ScriptingAdditions/
```**Launchd DB
```
/private/var/db/launchd.db/
$ find /private/var/db/launchd.db/ -name com.apple.launchd.peruser.* -exec ls -lsct {} \;/com.apple.launchd.peruser.*
```
**Loaded\_Drivers
```
$ kexstat
```**All Extensions
```
/System/Library/Extensions/
```
**Extra Extensions
```
/Extra/Extensions/
```**Crontab
```
$ crontab -u root -l , crontab -u <USERNAME> -l
```


###  APPLICATIONS ###
**Installation History
```
/Library/Receipts/InstallHistory.plist
```**Uninstallation History
```
sudo egrep --colour=auto -Ri 'uninstalld|removing Application' /var/log/*
sample : 
/var/log/commerce.log:Nov 26 15:42:35 amalard-3.mrc.cossi.internet storeassetd[413]: SoftwareMapSpotlightSource: removing Application <CKSoftwareProduct: 0x7ff538f2fda0>: (com.tastycocoabytes.CocoaPacketAnalyzer.mas, 1.31, 418357707:660823895 VPP:NO source:Spotlight /Applications/CocoaPacketAnalyzer.app) 
/var/log/system.log:Nov 26 15:42:30 amalard-3.mrc.cossi.internet uninstalld[2105]: Could not get Info.plist for /Applications/CocoaPacketAnalyzer.app
```
**Updates History
```
/Library/Preferences/com.apple.SoftwareUpdate.plist
```**Last launched applications
```
ls -lshtr /Library/Caches
ls -lshtr /Users/<USERNAME>/Library/Caches
```
**All installed Application and association files
```
$ /System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -dump | grep --after-context 1 "^bundle" | grep --only-matching "/.*\.app"$
$ /System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -dump -seed -all u,s,n,l
```**


###  USER ARTEFACTS ###
**Recent searches, Trash setting, view settings, recent folders
```
/Users/<USERNAME>/Library/Preferences/com.apple.finder.plist
```**

**Applications in the Dock
```
/Users/<USERNAME>/Library/Preferences/com.apple.dock.plist
```**

**folders and network shares in the Dock
```
/Users/<USERNAME>/Library/Preferences/com.apple.dock.plist
```**

**Desktop picture
```
/Users/<USERNAME>/Library/Preferences/com.apple.desktop.plist
```**

**recent documents, applications, and network connections
```
/Users/<USERNAME>/Library/Preferences/com.apple.recentitems.plist 
```**

**Preview files
```
/Users/<USERNAME>/Library/Preferences/com.apple.Preview.LSSharedFileList.plist
```**

###  USER SYSTEM HISTORY ###
**Concole Search History
```
/Users/<USERNAME>/Library/Preferences/com.apple.Console.plist
```**SQLite History
```
/Users/<USERNAME>/.sqlite_history
```
**BASH History
```
/Users/<USERNAME>/.bash_history
```**SH History
```
/Users/<USERNAME>/.sh_history
```
**Last logged users
```
$ last
```**Connected media history
```
/Users/<USERNAME>/Library/Preferences/com.apple.sidebarlists.plist
```


###  TOOL EVERYDAY INFO ###
**Address Book
```
/Users/<USERNAME>/Library/Application Support/AddressBook/MailRecents-v4.abcdmr
```**

**Calendar (through Spotlight)
```
/Users/<USERNAME>/Library/Calendars/Calendar\ Cache
```**

**User emails, only text (through Spotlight)
```
/Users/<USERNAME>/Library/Mail/V2/MailData/Envelope\ Index
```**

**User emails, full (through mBox files)
```
/Users/<USERNAME>/Library/Mail/V2/IMAP-username@mail.test.com/xxxx.mbox
```**

**Office documents restored by AutoRecovery service
```
/Users/<USERNAME>/Library/Application Support/Microsoft/Office/Office 2011 AutoRecovery
```**

**Recent printed documents
```
var/spool/cups/
[http://sud0man.blogspot.fr http://sud0man.blogspot.fr/2013/01/american-series-are-usefull-in.html]
```**

**Text notes taken with Stickies Widget (Widget available natively)
```
/Users/<USERNAME>/Library/Preferences/widget-com.apple.widget.stickies.plist
/Users/<USERNAME>/Library/StickiesDatabase
/Users/<USERNAME>/Library/Containers/com.apple.Notes/Data/Library/Notes/NotesV1.storedata-wal 
```**

**Evernotes text notes
```
/Users/<USERNAME>/Library/Application Support/Evernote/accounts/Evernote/xxxxxxxx/content/
```**


###  CHAT ###
**Skype messages history (stores conversations)
```
/Users/<USERNAME>/Library/Application\ Support/Skype/xxxxxxxx/main.db
```**

**Message history or new iChat (stores conversations)
```
/Users/<USERNAME>/Library/Messages/
```**

**iChat history (stores conversations)
```
/Users/<USERNAME>/Documents/iChats/
```**

**Adium history (stores conversations)
```
/Users/<USERNAME>/Library/Application\ Support/Adium\ 2.0/Users/Default/Logs/
```**

###  iDEVICES ###

**iDevice SMS (through iTunes backup)
```
/Users/<USERNAME>/Library/Application\ Support/MobileSync/Backup/<UUID>/3d0d7e5fb2ce288813306e4d4636395e047a3d28
```**

**iDevice Calendar (through iTunes backup)
```
/Users/<USERNAME>/Library/Application\ Support/MobileSync/Backup/<UUID>/2041457d5fe04d39d0ab481178355df6781e6858
```**

**iDevice Call history (through iTunes backup)
```
/Users/<USERNAME>/Library/Application Support/MobileSync/Backup/<UUID>/ff1324e6b949111b2fb449ecddb50c89c3699a78
```**

**iDevice SMS (through iTunes backup)
```
/Users/<USERNAME>/Library/Application Support/MobileSync/Backup/<UUID>/31bb7ba8914766d4ba40d6dfb6113c8b614be442
```**


###  WEB BROWSING ###
**Safari Browsing
```
[HISTORY]/Users/<USERNAME>/Library/Safari/History.plist]
[COOKIES]/Users/<USERNAME>/Library/Cookies/Cookies.plist
[COOKIES]/users/<USERNAME>/Library/Cookies/Cookies.binarycookies
[DOWNLOADS]/Users/<USERNAME>/Library/Safari/Downloads.plist
```**

**Safari Webpage Preview (stored Screenshot of your navigation):
```
/Users/<USERNAME>/Library/Caches/com.apple.Safari/Webpage Previews/
```**

**Firefox Browsing
```
[HISTORY]/Users/<USERNAME>/Library/Application\ Support/Firefox/Profiles/xxxxxxxx.default/places.sqlite
[COOKIES]/Users/<USERNAME>/Library/Application\ Support/Firefox/Profiles/xxxxxxxx.default/cookies.sqlite
[DOWNLOADS]/Users/<USERNAME>/Library/Application\ Support/Firefox/Profiles/xxxxxxxx.default/downloads.sqlite
```**

**Chrome Browsing
```
[HISTORY]/Users/<USERNAME>/Library/Application\ Support/Google/Chrome/Default/History
[COOKIES]/Users/<USERNAME>/Library/Application\ Support/Google/Chrome/Default/Cookies
[DOWNLOADS]/Users/<USERNAME>/Library/Application\ Support/Google/Chrome/Default/History
```**

**Opera Browsing
```
[HISTORY]/Users/<USERNAME>/Library/Application\ Support/com.operasoftware.Opera/History
[HISTORY]/Users/<USERNAME>/Library/Opera/global_history.dat
[COOKIES]/Users/<USERNAME>/Library/Application\ Support/com.operasoftware.Opera/Cookies
[COOKIES]/Users/<USERNAME>/Library/Opera/cookies4.dat
[DOWNLOADS]/Users/<USERNAME>/Library/Application\ Support/com.operasoftware.Opera/History
[DOWNLOADS]/Users/<USERNAME>/Library/Opera/download.dat
```**

**QuarantineEventsV (can contain Browser history and iChat)
```
/Users/<USERNAME>/Library/Preferences/com.apple.LaunchServices.QuarantineEventsV*
```**


###  DELETED/RECOVERED DATA ###
**Trashes
```
/Users/<USERNAME>/.Trash
/.Trashes
```**Recovery Office Files
```
/Users/<USERNAME>/Library/Application Support/Microsoft/Office/Office 2011 AutoRecovery
```


###  NETWORK HISTORY ###
**Bluetooth History
```
/Library/Preferences/com.apple.Bluetooth.plist
```**Network History
```
/Library/Preferences/SystemConfiguration/com.apple.network.identification.plist
```
**WiFI AP History
```
$ defaults read /Library/Preferences/SystemConfiguration/com.apple.airport.preferences|sed 's|\./|`pwd`/|g' | sed 's|.plist||g'|grep 'LastConnected' -A 3
```**Remote Desktop History
```
/Library/Preferences/com.apple.RemoteDesktop.plist
```


###  NETWORK CONFIGURATION ###
**Firewall
```
/Library/Preferences/com.apple.alf.plist
```**Wireless
```
/Library/Preferences/SystemConfiguration/com.apple.airport.preferences.plist
```
**NAT
```
/Library/Preferences/SystemConfiguration/com.apple.nat.plist
```**SMB Server
```
/Library/Preferences/SystemConfiguration/com.apple.smb.server.plist
```
**Interfaces (10.8)
```
/Library/Preferences/SystemConfiguration/NetworkInterfaces.plist
```**Interfaces
```
/Library/Preferences/SystemConfiguration/com.apple.NetworkInterfaces.plist
/Library/Preferences/SystemConfiguration/com.apple.preferences.plist
/Library/Preferences/SystemConfiguration/preferences.plist
```

###  MEMORY ###
**Hibernate file
```
/private/var/vm/sleepimage
```**Swap file
```
/private/var/vm/swapfile0
```


<br>
<br>



<h2><b>  FORENSICS - EVENTS  </b></h2>
<h3> PROOF OF CONCEPT</h3>
<ul><li>CheckOut4Mac: <a href='https://code.google.com/p/checkout4mac/'>https://code.google.com/p/checkout4mac/</a></li></ul>

<h3> STARTUP ACTIVITIES</h3>
<h4>Startup dates/hours on July 8</h4>
<pre><code>[On Lion and Mountain Lion] $sudo grep -i 'BOOT_TIME' /var/log/system.log|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
$sudo bzgrep -i 'BOOT_TIME' /var/log/system.log.*|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
</code></pre>
<h4>Stopping dates/hours on July 8</h4>
<pre><code>[On Lion and Mountain Lion] $sudo grep -i 'SHUTDOWN_TIME' /var/log/system.log|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
$sudo bzgrep -i 'SHUTDOWN_TIME' /var/log/system.log.*|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
</code></pre>
<h4>Hibernation dates/hours on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i 'hibernate_setup(0) took' /var/log/system.log|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
$sudo bzgrep -i 'hibernate_setup(0) took' /var/log/system.log.*|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
[On Lion] $sudo grep -i 'PMScheduleWakeEventChooseBest' /var/log/system.log|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
$sudo bzgrep -i 'PMScheduleWakeEventChooseBest' /var/log/system.log.*|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
</code></pre>
<h4>Out of hibernation dates/hours on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i 'Wake reason' /var/log/system.log|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
$sudo bzgrep -i 'Wake reason' /var/log/system.log.*|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
[On Lion] $sudo syslog -T utc+2 -F raw -f /var/log/asl/2013.07.08.*|grep 'Message Wake'|grep -i 'Jul  8'|cut -d ] -f 2|sed -e 's/\ \[Time/g'<br>
</code></pre>


<h3> SESSION ACTIVITIES</h3>
<h4>Locked session dates/hours on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i 'Application App:"loginwindow"' /var/log/system.log|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
$sudo bzgrep -i 'Application App:"loginwindow"' /var/log/system.log.*|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
[On Lion] $sudo grep -i 'loginwindow' /var/log/windowserver.log|grep -i 'Jul  8'|awk '{print$1,$2,$3}'<br>
</code></pre>
<h4>Attempt to unlocked session without success on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i -B 9 'The authtok is incorrect.' /var/log/system.log|grep -i 'Jul  8'|grep 'Got user'|awk '{print$1,$2,$3,$9,$10}'<br>
$sudo bzgrep -i -B 9 'The authtok is incorrect.' /var/log/system.log.*|grep -i 'Jul  8'|grep 'Got user'|awk '{print$1,$2,$3,$9,$10}'<br>
[On Lion] $sudo grep -i -B 9 'The authtok is incorrect.' /var/log/secure.log|grep -i 'Jul  8'|grep 'Got user'|awk '{print$1,$2,$3,$9,$10}'<br>
$sudo bzgrep -i -B 9 'The authtok is incorrect.' /var/log/secure.log.*|grep -i 'Jul  8'|grep 'Got user'|awk '{print$1,$2,$3,$9,$10}'<br>
</code></pre>
<h4>Unlocked session with success on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i -A 1 'Establishing credentials' /var/log/system.log|grep -i 'Jul  8'|grep 'Got user'|awk '{print$1,$2,$3,$9,$10}'<br>
$sudo bzgrep -i -A 1 'Establishing credentials' /var/log/system.log.*|grep -i 'Jul  8'|grep 'Got user'|awk '{print$1,$2,$3,$9,$10}'<br>
[On Lion] $sudo grep -i -A 1 'Establishing credentials' /var/log/secure.log|grep -i 'Jul  8'|grep 'Got user'|awk '{print$1,$2,$3,$9,$10}'<br>
$sudo bzgrep -i -A 1 'Establishing credentials' /var/log/secure.log.*|grep -i 'Jul  8'|grep 'Got user'|awk '{print$1,$2,$3,$9,$10}'}}}<br>
</code></pre>

<h3> PHYSICAL CONNECTION ACTIVITIES</h3>
<h4>USB connections (last loading dates of USB extensions) on July 8</h4>
<pre><code>[On Mountain Lion and Lion] $sudo stat -f '%Sa %N' /System/Library/Extensions/*|external_bin/grep_gnu_lion -i 'Jul  8'|external_bin/grep_gnu_lion 2013|egrep -i 'IOUSBFamily.kext|IOUSBMassStorageClass.kext'<br>
$sudo ls -lu /System/Library/Extensions/|grep -i '8 Jul'|egrep 'IOUSBFamily.kext|IOUSBMassStorageClass.kext'| awk '{print $7,$6,$8,$9}'<br>
</code></pre>
<h4>USB plugged devices on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i 'USBMSC' /var/log/system.log|grep -i 'Jul  8'|awk '{print$1,$2,$3" =&gt; New plugged USB Device - USBMSC Identifier: "$10"(vendor)",$11"(Device) - To identify the plugged device : http:/www.linux-usb.org/usb.ids"}'<br>
$sudo bzgrep -i 'USBMSC' /var/log/system.log.*|grep -i 'Jul  8'|awk '{print$1,$2,$3" =&gt; New plugged USB Device - USBMSC Identifier: "$10"(vendor)",$11"(Device) - To identify the plugged device : http:/www.linux-usb.org/usb.ids"}'<br>
[On Lion] $sudo grep -i 'USBMSC' /var/log/kernel.log|grep -i 'Jul  8'|awk '{print$1,$2,$3" =&gt; New plugged USB Device - USBMSC Identifier: "$10"(vendor)",$11"(Device) - To identify the plugged device : http:/www.linux-usb.org/usb.ids"}'<br>
$sudo bzgrep -i 'USBMSC' /var/log/kernel.log.*|grep -i 'Jul  8'|awk '{print$1,$2,$3" =&gt; New plugged USB Device - USBMSC Identifier: "$10"(vendor)",$11"(Device) - To identify the plugged device : http:/www.linux-usb.org/usb.ids"}'<br>
</code></pre>
<h4>File system events(USB, mounting, etc.) on July 8</h4>
<pre><code>[On Lion and Mountain Lion] $sudo grep -i 'fsevents' /var/log/system.log|grep -i 'Jul  8'<br>
$sudo bzgrep -i 'fsevents' /var/log/system.log.*|grep -i 'Jul  8'<br>
</code></pre>
<h4>Firewire connections with an other machine or storage media (last loading dates of Firewire extensions)</h4>
<pre><code>[On Lion and Mountain Lion] $sudo stat -f '%Sa %N' /System/Library/Extensions/*|external_bin/grep_gnu_lion -i 'Jul  8'|external_bin/grep_gnu_lion 2013|egrep -i 'IOFireWireFamily.kext|IOFireWireIP.kext'<br>
$sudo ls -lu /System/Library/Extensions/|grep -i '8 Jul'|egrep 'IOFireWireFamily.kext|IOFireWireIP.kext'| awk '{print $7,$6,$8,$9}'<br>
</code></pre>
<h4>Firewire connections with an other machine or storage media (activation of 'fw' interface)</h4>
<pre><code>[On Lion and Mountain Lion] $sudo grep -i 'fw' /var/log/system.log|grep -i 'Jul  8'|grep 'network changed'|awk '{print$1,$2,$3}'<br>
$sudo bzgrep -i 'fw' /var/log/system.log.*|grep -i 'Jul  8'|grep 'network changed'|awk '{print$1,$2,$3}'<br>
</code></pre>
<h4>Firewire connections to dump RAM (last loading dates of extensions IOFireWireSBP2/iPodDriver) just a supposition</h4>
<pre><code>[On Lion and Mountain Lion] $sudo stat -f '%Sa %N' /System/Library/Extensions/*|external_bin/grep_gnu_lion -i 'Jul  8'|external_bin/grep_gnu_lion 2013|egrep -i 'iPodDriver.kext|IOFireWireSBP2.kext'<br>
$sudo ls -lu /System/Library/Extensions/|grep -i '8 Jul'|egrep 'iPodDriver.kext|IOFireWireSBP2.kext'| awk '{print $7,$6,$8,$9}'<br>
</code></pre>


<h3> ESCALATION PRIVILEGES ACTIVITIES</h3>
<h4>Opened/Closed TTY terminals on July 8</h4>
<pre><code>[On Lion and Mountain Lion] $sudo grep -i 'ttys' /var/log/system.log|grep -i 'Jul  8'| egrep 'USER_PROCESS|DEAD_PROCESS'|sed -e 's/USER_PROCESS/OPENING TERMINAL/g' |sed -e 's/DEAD_PROCESS/CLOSING TERMINAL/g'| awk '{print $1,$2,$3,$6,$7,$9}'<br>
$sudo bzgrep -i 'ttys' /var/log/system.log.*|grep -i 'Jul  8'| egrep 'USER_PROCESS|DEAD_PROCESS'|sed -e 's/USER_PROCESS/OPENING TERMINAL/g' |sed -e 's/DEAD_PROCESS/CLOSING TERMINAL/g'| awk '{print $1,$2,$3,$6,$7,$9}'<br>
</code></pre>
<h4>ROOT commands executed with success on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i 'sudo\[' /var/log/system.log|grep -i 'Jul  8'<br>
$sudo grep -i 'sudo\[' /var/log/system.log.*|grep -i 'Jul  8'<br>
[On Lion] $sudo grep -i 'sudo\[' /var/log/secure.log|grep -i 'Jul  8'<br>
$sudo grep -i 'sudo\[' /var/log/secure.log.*|grep -i 'Jul  8'<br>
</code></pre>
<h4>Attempt to execute commands with SUDO without success on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i 'incorrect password attempts' /var/log/system.log|grep -i 'Jul  8'<br>
$sudo bzgrep -i 'incorrect password attempts' /var/log/system.log.*|grep -i 'Jul  8'<br>
[On Lion] $sudo grep -i 'incorrect password attempts' /var/log/secure.log|grep -i 'Jul  8'<br>
$sudo bzgrep -i 'incorrect password attempts' /var/log/secure.log.*|grep -i 'Jul  8'<br>
</code></pre>
<h4>User, password modification and creation on July 8</h4>
<pre><code>[On Lion and Mountain Lion] $sudo praudit -xn /var/audit/current|egrep 'create user|modify password|delete user' -A 3|grep -i 'Jul  8' -A 3|sed 's/\&amp;apos\;/"/g'<br>
</code></pre>

<h3> APPLICATIONS ACTIVITIES</h3>
<h4>Opened applications (last access dates) on July 8</h4>
<pre><code>[On Lion and Mountain Lion] <br>
$ls -lshtr /Users/&lt;USER&gt;/Library/Caches | grep 'Jul 8'<br>
$sudo find /Applications -maxdepth 3 -type f -exec ls -lu {} \; |grep Info.plist |grep  -i '8 Jul'|grep -v root|awk '{$7=""}1'<br>
$sudo stat -f '%Sa %N' /Applications/*/*/* |external_bin/grep_gnu_lion -i 'Jul  8'<br>
$sudo find /Applications/ -name "Info.plist" -type f -exec stat -f '%Sa %N' {} \;|grep 'Jul  8'<br>
</code></pre>

<h3> FILES ACTIVITIES</h3>
<h4>Modified files (like autorun App, LaunchAgents or LaunchDaemons) on July 8</h4>
<pre><code>[On Lion and Mountain Lion] $sudo find /path_to_file -type f -exec stat -f '%Sm %N' '{}' + |grep -i 'Jul  8'|grep 2013<br>
for example, path_to_file=["/System/Library/XPCServices/","/System/Library/LaunchAgents/","/Library/LaunchAgents/","/Users/&lt;USERNAME&gt;/Library/LaunchAgents/","/System/Library/LaunchDaemons/","/Library/LaunchDaemons/"]<br>
</code></pre>
<h4>Added files (like trojan or malware App) on July 8</h4>
<pre><code>[On Lion and Mountain Lion] $sudo find /path_to_directory -type f -exec stat -f '%SB %N' '{}' + |grep -i 'Jul  8'|grep 2013<br>
for example, path_to_directory=["/Users/&lt;USERNAME&gt;/Library/Preferences/com.apple.loginitems.plist","/etc/passwd"]<br>
</code></pre>
<h4>Accessed files (like your secret files) on July 8</h4>
<pre><code>[On Lion and Mountain Lion] $sudo find /path_to_directory -type f  -exec stat -f '%Sa %N' '{}' + |grep -i 'Jul  8'|grep 2013<br>
for example, path_to_directory=["/Users/&lt;USERNAME&gt;","/Volume/Supersecret"]<br>
</code></pre>
<h4>Accessed Mails (last access dates) on July 8</h4>
<pre><code>[On Lion and Mountain Lion] grep /Users/&lt;USERNAME&gt;/Library/Mail/V2/IMAP-YYYY\@mail.XXXX.fr/INBOX.mbox/ -type f -name *.emlx -exec stat -f '%Sa %N' '{}' + |grep -i 'Jul  8'|grep 2013<br>
</code></pre>

<h3> NETWORK ACTIVITIES</h3>
<h4>Network connections (based on DNS queries) on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i 'DNS+' /var/log/system.log|grep -i 'Jul  8'|grep 'network changed'|awk '{print$1,$2,$3}'<br>
$sudo bzgrep -i 'DNS+' /var/log/system.log.*|grep -i 'Jul  8'|grep 'network changed'|awk '{print$1,$2,$3}'<br>
</code></pre>
<h4>Network disconnections (based on DNS queries) on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i 'DNS-' /var/log/system.log|grep -i 'Jul  8'|grep 'network changed'|awk '{print$1,$2,$3}'<br>
$sudo bzgrep -i 'DNS-' /var/log/system.log.*|grep -i 'Jul  8'|grep 'network changed'|awk '{print$1,$2,$3}'<br>
</code></pre>
<h4>Ethernet/WiFI connections (activation of 'enX' interface) on July 8</h4>
<pre><code>[On Mountain Lion] $sudo grep -i 'en' /var/log/system.log|grep -i 'Jul  8'|grep 'network changed'|awk '{print$1,$2,$3}'<br>
$sudo bzgrep -i 'en' /var/log/system.log.*|grep -i 'Jul  8'|grep 'network changed'|awk '{print$1,$2,$3}'<br>
[On Lion] $sudo egrep -i 'frequent transitions|network configuration changed' /var/log/system.log|grep -i 'Jul  8'<br>
$sudo bzegrep -i 'frequent transitions|network configuration changed' /var/log/system.log.*|grep -i 'Jul  8'<br>
</code></pre>
<h4>WiFI access points (last connection dates) / warning to the time zone on July 8</h4>
<pre><code>[On Lion and Mountain Lion] $sudo defaults read /Volumes/Macintosh\ HD/Library/Preferences/SystemConfiguration/com.apple.airport.preferences.plist| sed 's|\./|`pwd`/|g' | sed 's|.plist||g'|grep 'LastConnected' -A 3 |grep -A 3 2013-07-08<br>
</code></pre>
<br>
<br>

<h2><b>  WIFI  </b></h2>
<h3> My WiFI Scripts</h3>
<ul><li><a href='https://code.google.com/p/fun-scripts/source/browse/#svn%2Ftrunk%2FWiFI_Check_SSID_like_PASS'>https://code.google.com/p/fun-scripts/source/browse/#svn%2Ftrunk%2FWiFI_Check_SSID_like_PASS</a>
</li><li><a href='https://code.google.com/p/fun-scripts/source/browse/#svn%2Ftrunk%2FWiFI_Aircrack_Help%253Fstate%253Dclosed'>https://code.google.com/p/fun-scripts/source/browse/#svn%2Ftrunk%2FWiFI_Aircrack_Help%253Fstate%253Dclosed</a></li></ul>

<h3> WiFI tricks</h3>
<b>How to display available WiFI networks:<br>
<pre><code>$sudo /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport en1 -s<br>
<br>
                 FreeWifi_secure 16:10:18:47:f2:4d -83  5       Y  -- WPA(802.1x/AES/AES) <br>
                    Livebox-eaXX 00:1d:6a:45:06:eb -79  6       Y  FR WPA(PSK/AES,TKIP/TKIP) WPA2(PSK/AES,TKIP/TKIP) <br>
                  Freebox-4862XX f4:ca:e5:e1:ec:ac -88  8       Y  -- WPA(PSK/AES/AES) <br>
                        FreeWifi 22:48:94:aa:8d:e2 -84  11      Y  -- NONE<br>
                        FreeWifi f4:ca:e5:8b:46:91 -85  11      Y  -- NONE<br>
           Réseau Wi-Fi de toto 5c:96:9d:69:36:92 -85  60,+1   Y  FR WPA2(PSK/AES/AES) <br>
           Réseau Wi-Fi de toto 5c:96:9d:69:36:91 -66  11      Y  FR WPA2(PSK/AES/AES) <br>
                        FreeWifi f4:ca:e5:e1:ec:ad -86  8       Y  -- NONE<br>
                 FreeWifi_secure 00:24:d4:ca:02:5e -85  7       Y  -- WPA2(802.1x/AES,TKIP/TKIP) <br>
 2 IBSS networks found:<br>
                            SSID BSSID             RSSI CHANNEL HT CC SECURITY (auth/unicast/group)<br>
                        HP01C65B f6:3f:43:f9:3f:92 -85  1       N  EU NONE<br>
                        HP0142F9 02:2d:8d:e6:9f:e0 -65  10      N  EU NONE<br>
</code></pre></b>

<b>How to join WiFI networks (or test pre-shared key) :<br>
<pre><code>$/usr/sbin/networksetup -setairportnetwork en1 "yellowstay" "P@ssword8888" <br>
    ==&gt; good pre-shared key (no error message)<br>
</code></pre></b>

<pre><code>$/usr/sbin/networksetup -setairportnetwork en1 "yellowstay" "P@ssword12345" <br>
Failed to join network yellowstay.<br>
    ==&gt; bad pre-shared key (error message)<br>
</code></pre>

<b>How to disassociate you of a WiFI network :<br>
<pre><code>$sudo /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport en1 -z<br>
</code></pre></b>

<b>WiFI history (last connection, date, SSID, etc.):<br>
<pre><code>defaults read /Library/Preferences/SystemConfiguration/com.apple.airport.preferences| sed 's|\./|`pwd`/|g' | sed 's|.plist||g'|grep 'LastConnected' -A 3<br>
</code></pre></b>

<h2><b>  MISC  </b></h2>
<b>How to take a screenshot every second and store images (during 30s in this example):<br>
<pre><code>for i in $(seq 1 30);  do sleep 1 &amp;&amp; /usr/sbin/screencapture /tmp/screen$i.png;done &gt; /dev/null 2&gt;&amp;1<br>
</code></pre></b><br>
<br>