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

# Test port reachability with a timeout
nc -zv -w 3 192.168.1.50 8080

# Capture raw HTTP network traffic on port 80
tcpdump -i eth0 -nn -A 'tcp port 80'

# List active connections aggregated by foreign address
ss -ant | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr

# Find and terminate all processes matching a specific name
pgrep -f "python_worker" | xargs kill -15

# Monitor high-memory processes in real-time, refreshed every 2 seconds
watch -n 2 'ps aux --sort=-%mem | head -n 10'

# Dynamically change the priority (niceness) of a running process
renice -n 10 -p 12345

# View systemd service logs since last boot with live tailing
journalctl -u nginx.service -b -f

# List all active SSH connections with remote IP and logged-in user
who -u | awk '{print $1, $5}'

# Find files with SUID permission enabled (potential security risk)
find / -perm -4000 -type f -ls 2>/dev/null

# Block an IP address immediately using iptables
iptables -A INPUT -s 192.168.1.100 -j DROP

# Audit recent failed login attempts via SSH
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr

# Monitor page faults and RAM stats every 1 second (5 iterations)
vmstat 1 5

# Check open file descriptors used by a specific process ID
ls -l /proc/12345/fd

# Display kernel shared memory segments and semaphores
ipcs -a

# Measure exact system time, user time, and memory overhead of a command
/usr/bin/time -v tar -czf backup.tar.gz /var/www/html

# Monitor kernel buffer ring logs in real-time (useful for USB/hardware events)
dmesg -wH

# List all PCI devices with their active kernel drivers
lspci -k

# Display detailed CPU architecture, core counts, and flags
lscpu

# Inspect SMART status and health pass/fail state for a storage disk
smartctl -H /dev/sda
