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

