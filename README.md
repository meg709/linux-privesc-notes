Day 1
Linux users belong to groups. Group membership can grant additional permissions and access to files or commands
ls -l , list permissions for files
touch file.txt , creates file
stat file.txt , shows information about file
Linux files have an owner and a group. Permissions are applied separately to the owner, group, and others. Misconfigured ownership can allow privilege escalation.
chmod 777 file.txt , gives permissions to allow everyone to do everything
chmod 600 file.txt , owner can only read or write permission
Too much permissions can expose sensitive files
echo "echo hello" > script.sh , attach text to script file and prints echo when executed.
chmod +x script.sh , gives permission to execute
The execute permission allows files to be run as programs. Writable and executable scripts owned by privileged users can lead to privilege escalation.

Sudo -l ,list what i'm able to run with sudo
What is sudo
Sudo allows a user to execute commands as another user, usually root.

How to enumerate

sudo -l


Why it is dangerous
If a user is allowed to run powerful binaries (e.g. vim, python) with sudo, they can escape into a root shell.

Impact
Full system compromise.

Mitigation
Restrict sudo permissions. Avoid NOPASSWD for interactive binaries.

That is a real security write‑up, not student fluff.


DAY2
2️⃣ How to find SUID files (this is muscle memory)
Run:
find / -perm -4000 -type f 2>/dev/null
What this means:
-4000 → SUID bit
-type f → files only
2>/dev/null → hide permission errors

5️⃣ Example: SUID find

If you see:
/usr/bin/find
Check GTFOBins → find → SUID
Exploit:
find . -exec /bin/bash -p \; -quit

GTFOBins (this is your cheat code)
GTFOBins =
“If this binary runs as root, here’s how to abuse it”

What to look out for when running this command /etc/cron*
1.writable scirpts run by root
2.scripts calling commands without full paths
3.scripts in writeable directories
1️⃣ YOUR output (GOOD configuration)
From your crontab -l
@reboot /bin/bash /root/Scripts/displayip.sh

Why this is GOOD

Let’s break it down piece by piece:

Part	Why it’s safe
/bin/bash	Absolute path → cannot hijack
/root/Scripts/displayip.sh	Located in /root
/root	Not writable by normal users
Script owner	root
Permissions	not world-writable
4️⃣ BAD variation #3 – Script in writable directory
GOOD (yours)
/root/Scripts/displayip.sh

BAD
/tmp/displayip.sh


Because /tmp is writable by everyone:

echo "/bin/bash" > /tmp/displayip.sh
