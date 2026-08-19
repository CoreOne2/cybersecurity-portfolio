cat : Displays the entire content of a file. (cat /var/log/auth.log)
less : Opens logs in an interactive viewer (scroll up/down with arrow keys, press q to quit). {less /var/log/syslog}
pwd : Print Working Directory (shows where you are)
ls : List files and folders
ls -la : List all files (including hidden ones) with details
cd [folder] : Change directory

File Operations	cp, mv, rm

File Viewing	cat, less, tail

Search & Filter	grep, find

Permissions	chmod, chown

Process Control	ps, top, kill

# Find and replace text in multiple files recursively
sed -i 's/old_value/new_value/g' *.conf

# Extract specific columns (e.g., username and shell) from /etc/passwd
awk -F: '{print $1, $7}' /etc/passwd

# Filter log entries between specific timestamps
awk '$0 >= "2026-08-19 10:00:00" && $0 <= "2026-08-19 12:00:00"' /var/log/syslog

# Find files modified in the last 24 hours and compress them
find . -type f -mtime -1 -exec tar -rvf recent_files.tar {} +

# Extract unique IP addresses from an access log and count hits
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -n 10
