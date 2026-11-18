# Linux Disk Management (`df` & `du`) & 25 Hands-On Linux Team Projects

A comprehensive GitHub-ready guide covering Linux disk monitoring commands (`df` and `du`), real-world troubleshooting scenarios, shell scripting automation, and **25 hands-on Linux lab projects with complete solutions** for team training.

---

## Table of Contents

1. [Overview: `df` vs. `du`](#1-overview-df-vs-du)
2. [15 Examples of `df` (Disk Free)](#2-15-examples-of-df-disk-free)
3. [15 Examples of `du` (Disk Usage)](#3-15-examples-of-du-disk-usage)
4. [Real-World Troubleshooting Scenarios](#4-real-world-troubleshooting-scenarios)
5. [Shell Scripting & Monitoring with `df` and `du`](#5-shell-scripting--monitoring-with-df-and-du)
6. [25 Hands-On Team Lab Projects (with Solutions)](#6-25-hands-on-team-lab-projects-with-solutions)

---

## 1. Overview: `df` vs. `du`

* **`df` (Disk Free):** Reports available and used space across entire **mounted filesystems**. It reads filesystem superblock metadata to instantly show total capacity, used blocks, free blocks, percentage used, and mount points.
* **`du` (Disk Usage):** Calculates the exact disk space consumed by **specific files and directories**. It traverses directory trees and sums the disk blocks allocated to individual items.

> **Analogy:** `df` is checking the fuel gauge of your vehicle, while `du` is weighing each suitcase inside the trunk to see what is consuming the capacity.

---

## 2. 15 Examples of `df` (Disk Free)

1. `df` — Show disk space usage for all mounted filesystems in default 1K blocks.
2. `df -h` — Display sizes in human-readable binary format (`KiB`, `MiB`, `GiB`).
3. `df -H` — Display sizes in human-readable decimal SI format (`KB`, `MB`, `GB`).
4. `df -T` — Include the filesystem type (`ext4`, `xfs`, `tmpfs`, etc.).
5. `df -hT` — Combine human-readable sizes with filesystem types (standard sysadmin view).
6. `df -i` — Check **inode** usage instead of block space (crucial when a drive has millions of tiny files).
7. `df -h /var` — Show disk space details only for the filesystem hosting `/var`.
8. `df -a` — Include dummy, pseudo, and duplicate filesystems (such as `/proc` and `/sys`).
9. `df -t ext4` — Filter output to display only `ext4` filesystems.
10. `df -x tmpfs -x devtmpfs` — Exclude virtual memory filesystems from the output.
11. `df -l` — Limit the listing strictly to local filesystems, ignoring network shares (`NFS`/`SMB`).
12. `df --total -h` — Add a grand total summary row at the bottom of the output.
13. `df --output=source,pcent,target` — Print only specific columns (device, percentage used, mount point).
14. `df -BM` — Force all size outputs to display strictly in Megabytes.
15. `df -P` — Output in POSIX standard format (prevents long device names from wrapping onto two lines in scripts).

---

## 3. 15 Examples of `du` (Disk Usage)

1. `du` — Recursively list disk usage of every subdirectory in the current path in 1K blocks.
2. `du -h` — Show sizes in human-readable format (`K`, `M`, `G`).
3. `du -sh /var/log` — Display only the grand total (`-s`) size of `/var/log` in human-readable format.
4. `du -sh *` — Show the total size of each file and top-level folder in the current directory.
5. `du -h --max-depth=1 /home` — Show sizes of `/home` and its immediate child folders only.
6. `du -ah /etc` — Include individual files (`-a`) alongside directories in the output.
7. `du -ch *.log` — List sizes of all `.log` files and print a combined total at the end.
8. `du -sh --apparent-size /path` — Show actual data byte size rather than allocated disk block size.
9. `du -sh --exclude="*.iso" /data` — Calculate folder size while ignoring `.iso` files.
10. `du -shx /` — Calculate usage of `/` without crossing into other mounted drives (like `/mnt` or `/boot`).
11. `du -h --time /var/www` — Show the last modification timestamp next to each folder's size.
12. `du -h --threshold=1G /` — Display only directories consuming 1 GB or more.
13. `du -s --inodes /var` — Count the total number of inodes (files/folders) inside `/var`.
14. `du -sk * | sort -n` — Sort items in the current directory from smallest to largest (in KB).
15. `du -sh * | sort -hr | head -n 10` — Find the top 10 largest files or directories in the current path.

---

## 4. Real-World Troubleshooting Scenarios

### Scenario A: Server Disk Full Alert (`df -h` + `du -sh`)
When an application crashes with `"No space left on device"`:
1. Run `df -h` to identify which mount point is at `100%`.
2. Navigate to that mount point and run `du -sh * | sort -hr | head -n 10` to locate runaway logs, backups, or core dumps.

### Scenario B: Out of Inodes (`df -i`)
A server reports 50% free space in `df -h`, yet fails to create new files:
1. Run `df -i` to verify if `IUse%` is at `100%`.
2. Run `du -s --inodes /* 2>/dev/null | sort -nr | head -n 10` to find which folder holds millions of tiny session, mail, or cache files.

### Scenario C: Deleted File Still Consuming Space
`df -h` shows a partition is full even after deleting a huge file, while `du -sh` shows plenty of free space:
* **Cause:** A running process still holds an open file descriptor to the deleted file.
* **Fix:** Run `lsof +L1` to identify the process holding the deleted file and restart that service (or truncate the file descriptor).

### Scenario D: Pre-Migration & Backup Sizing
Before running `tar` or `rsync`:
1. Run `du -sh /source_dir` to measure total source data.
2. Run `df -h /backup_target` to confirm sufficient destination space.

---

## 5. Shell Scripting & Monitoring with `df` and `du`

> **Best Practice:** In scripts, avoid `-h` because suffixes like `M` or `G` break numeric comparisons. Use `-P` or `--output` for `df`, and `-sm` or `-sk` for `du`.

### Script 1: Disk Partition Threshold Alert (`df`)
```bash
#!/bin/bash
THRESHOLD=85
USAGE=$(df -P / | awk 'NR==2 {print $5}' | tr -d '%')

if [ "$USAGE" -ge "$THRESHOLD" ]; then
    echo "CRITICAL: Root partition is at ${USAGE}% capacity!"
else
    echo "OK: Root partition is at ${USAGE}% capacity."
fi
```

### Script 2: Directory Size Quota Check (`du`)
```bash
#!/bin/bash
LIMIT_MB=5000
TARGET_DIR="/var/log"
DIR_SIZE_MB=$(du -sm "$TARGET_DIR" | awk '{print $1}')

if [ "$DIR_SIZE_MB" -gt "$LIMIT_MB" ]; then
    echo "WARNING: $TARGET_DIR exceeds ${LIMIT_MB}MB (Current: ${DIR_SIZE_MB}MB)"
else
    echo "OK: $TARGET_DIR is within limit (${DIR_SIZE_MB}MB / ${LIMIT_MB}MB)"
fi
```

### Key Infrastructure Monitoring Use Cases
* **Capacity Planning & Growth Trending:** Collecting daily `df` metrics via Prometheus Node Exporter, Zabbix, or Nagios to forecast storage upgrades.
* **Inode Exhaustion Alerts:** Monitoring `df -i` to prevent container runtimes, mail queues, or `/tmp` directories from stalling.
* **Log & Container Bloat Detection:** Scheduled `du` audits on `/var/log` and `/var/lib/docker` to catch broken `logrotate` rules.
* **User Quota Audits:** Tracking `/home` usage across multi-user or shared development servers.

---

## 6. 25 Hands-On Team Lab Projects (with Solutions)

These 25 progressive projects cover essential Linux CLI skills: file operations, permissions, text processing (`grep`, `awk`, `sed`), process management, networking, disk checks (`df`, `du`), and bash scripting.

> **Setup Tip:** Have team members run all labs inside a dedicated workspace directory (`mkdir -p ~/linux_labs && cd ~/linux_labs`) to keep their environment safe and organized.

---

### Project 1: Workspace Hierarchy & Bulk File Creation
* **Objective:** Create a nested project directory structure and populate multiple files in a single command.
* **Task:** Create `project_alpha/{src,docs,logs,backup}` and generate `app1.log`, `app2.log`, `app3.log`, `app4.log`, and `app5.log` inside `logs/`.
* **Solution:**
  ```bash
  mkdir -p project_alpha/{src,docs,logs,backup}
  touch project_alpha/logs/app{1..5}.log
  ls -R project_alpha
  ```

---

### Project 2: File Searching & Pattern Filtering with `find`
* **Objective:** Locate files based on extension, modification time, and size.
* **Task:** Find all `.log` files inside `project_alpha` that are empty (`0 bytes`) and list their details.
* **Solution:**
  ```bash
  find project_alpha -type f -name "*.log" -empty -ls
  ```

---

### Project 3: Permissions & Ownership Hardening
* **Objective:** Secure sensitive configuration files using numeric (`chmod`) permissions.
* **Task:** Create `config.env`, set permissions so only the owner can read and write (`600`), and create a deploy script `deploy.sh` with `750` permissions.
* **Solution:**
  ```bash
  touch config.env deploy.sh
  chmod 600 config.env
  chmod 750 deploy.sh
  ls -l config.env deploy.sh
  ```

---

### Project 4: Soft Links vs. Hard Links Investigation
* **Objective:** Understand how inodes behave with hard links and symbolic links.
* **Task:** Create `master.txt` with text `"Version 1"`. Create a hard link `hard_master.txt` and a soft link `soft_master.txt`. Compare their inode numbers, delete `master.txt`, and test which link still works.
* **Solution:**
  ```bash
  echo "Version 1" > master.txt
  ln master.txt hard_master.txt
  ln -s master.txt soft_master.txt
  ls -li master.txt hard_master.txt soft_master.txt
  rm master.txt
  cat hard_master.txt   # Works (shares original inode)
  cat soft_master.txt   # Fails (broken symlink)
  ```

---

### Project 5: Log Analysis with `grep` & Regular Expressions
* **Objective:** Extract error and warning events from sample application logs.
* **Task:** Create `server.log` with mixed `INFO`, `WARN`, and `ERROR` lines. Count how many `ERROR` lines exist (case-insensitive) and display them with line numbers.
* **Solution:**
  ```bash
  cat << 'EOF' > server.log
  2026-11-01 INFO Service started
  2026-11-01 WARN High memory usage
  2026-11-01 ERROR Database connection failed
  2026-11-01 error Timeout waiting for upstream
  EOF

  grep -in "error" server.log
  grep -ic "error" server.log
  ```

---

### Project 6: Column Extraction & Aggregation with `awk`
* **Objective:** Parse structured log or CSV data and calculate totals.
* **Task:** Create `sales.csv` with `User,Department,BytesUsed`. Use `awk` to print users in the `DevOps` department and sum their total `BytesUsed`.
* **Solution:**
  ```bash
  cat << 'EOF' > sales.csv
  Alice,DevOps,1500
  Bob,HR,800
  Charlie,DevOps,2500
  Diana,Finance,1200
  EOF

  awk -F',' '$2=="DevOps" {print $1, $3; sum+=$3} END {print "Total DevOps Bytes:", sum}' sales.csv
  ```

---

### Project 7: Stream Editing & In-Place Config Updates with `sed`
* **Objective:** Automate configuration file edits without opening an interactive text editor.
* **Task:** Create `app.conf` containing `PORT=8080` and `DEBUG=true`. Use `sed` (with backup) to change `PORT` to `9090` and `DEBUG` to `false`.
* **Solution:**
  ```bash
  printf "PORT=8080
DEBUG=true
" > app.conf
  sed -i.bak -e 's/PORT=8080/PORT=9090/' -e 's/DEBUG=true/DEBUG=false/' app.conf
  cat app.conf
  ```

---

### Project 8: Sorting, Deduplicating & Counting Top Offenders
* **Objective:** Identify top IP addresses hitting a web server using CLI pipelines.
* **Task:** Create `access.txt` with repeated IP addresses. Produce a ranked list showing the hit count next to each unique IP, sorted from highest to lowest.
* **Solution:**
  ```bash
  printf "10.0.0.1
10.0.0.2
10.0.0.1
10.0.0.3
10.0.0.1
10.0.0.2
" > access.txt
  sort access.txt | uniq -c | sort -nr
  ```

---

### Project 9: Simulating & Locating Large Files with `dd` and `du`
* **Objective:** Generate dummy files of specific sizes and locate the largest consumers using `du`.
* **Task:** Create a folder `disk_test` containing a 5MB file, a 25MB file, and a 60MB file. Use `du` and `sort` to list them from largest to smallest in human-readable format.
* **Solution:**
  ```bash
  mkdir -p disk_test
  dd if=/dev/zero of=disk_test/file_5m.bin bs=1M count=5 status=none
  dd if=/dev/zero of=disk_test/file_25m.bin bs=1M count=25 status=none
  dd if=/dev/zero of=disk_test/file_60m.bin bs=1M count=60 status=none
  du -sh disk_test/* | sort -hr
  ```

---

### Project 10: Filesystem & Inode Audit with `df`
* **Objective:** Inspect mounted filesystems, filesystem types, and inode availability.
* **Task:** Display all local mounted filesystems excluding `tmpfs` and `devtmpfs`, showing filesystem types in human-readable sizes, then verify inode usage on `/`.
* **Solution:**
  ```bash
  df -hTl -x tmpfs -x devtmpfs
  df -ih /
  ```

---

### Project 11: Archiving & Compression with `tar`
* **Objective:** Bundle and compress directories while excluding temporary or log files.
* **Task:** Create a compressed `.tar.gz` archive of `project_alpha` named `alpha_backup.tar.gz`, excluding all `.log` files, then verify archive contents without extracting.
* **Solution:**
  ```bash
  tar --exclude="*.log" -czvf alpha_backup.tar.gz project_alpha
  tar -tzvf alpha_backup.tar.gz
  ```

---

### Project 12: Background Jobs & Process Control
* **Objective:** Run long-running commands in the background, inspect them, and terminate cleanly.
* **Task:** Start `sleep 300` in the background, verify its PID using `ps` or `jobs`, send a `SIGTERM` signal to stop it, and confirm it is gone.
* **Solution:**
  ```bash
  sleep 300 &
  jobs -l
  PID=$!
  kill "$PID"
  ps -p "$PID"
  ```

---

### Project 13: CPU & Memory Bottleneck Inspection
* **Objective:** Inspect system uptime, load averages, RAM/swap consumption, and top resource-consuming processes.
* **Task:** Print system load averages, display RAM usage in megabytes, and list the top 5 memory-consuming processes.
* **Solution:**
  ```bash
  uptime
  free -m
  ps aux --sort=-%mem | head -n 6
  ```

---

### Project 14: Network Connectivity & Port Diagnostics
* **Objective:** Check network interfaces, DNS resolution, and active listening ports.
* **Task:** Display local IP addresses, test DNS resolution for `github.com`, and list all active TCP/UDP listening sockets.
* **Solution:**
  ```bash
  ip -brief addr
  getent hosts github.com || nslookup github.com
  ss -tuln
  ```

---

### Project 15: Environment Variables & Shell Aliases
* **Objective:** Configure custom environment variables and command shortcuts.
* **Task:** Export an environment variable `APP_ENV="staging"`, verify it inside a child bash subshell, and create a temporary alias `ll` for `ls -alF`.
* **Solution:**
  ```bash
  export APP_ENV="staging"
  bash -c 'echo "Subshell APP_ENV is: $APP_ENV"'
  alias ll='ls -alF'
  ll
  ```

---

### Project 16: File Integrity Verification with Checksums
* **Objective:** Detect file tampering or corruption using SHA-256 hashes.
* **Task:** Generate a `sha256sum` manifest for `alpha_backup.tar.gz`, verify it passes, modify a single byte or create an altered copy, and observe verification failure.
* **Solution:**
  ```bash
  sha256sum alpha_backup.tar.gz > checksums.sha256
  sha256sum -c checksums.sha256
  echo "corrupt" >> alpha_backup.tar.gz
  sha256sum -c checksums.sha256
  ```

---

### Project 17: Comparing Configuration Files with `diff`
* **Objective:** Identify line-by-line differences between two versions of a configuration file.
* **Task:** Compare `app.conf` and `app.conf.bak` (from Project 7) in unified diff format.
* **Solution:**
  ```bash
  diff -u app.conf.bak app.conf
  ```

---

### Project 18: User & Group Account Inspection
* **Objective:** Audit local user accounts, UIDs, default shells, and group memberships.
* **Task:** List all non-system human users (UID >= 1000) from `/etc/passwd` and check groups for your current user.
* **Solution:**
  ```bash
  awk -F':' '$3 >= 1000 && $1 != "nobody" {print $1, "UID:", $3, "Shell:", $7}' /etc/passwd
  id
  groups
  ```

---

### Project 19: Automated Disk Space Alert Bash Script (`df` + `du`)
* **Objective:** Build a production-style bash script combining `df` threshold checks with `du` root-cause reporting.
* **Task:** Write `disk_watchdog.sh` that checks `/` usage. If usage exceeds a given threshold (pass `1` as an argument to test triggering), print the top 5 largest directories inside `/var`.
* **Solution:**
  ```bash
  cat << 'EOF' > disk_watchdog.sh
  #!/bin/bash
  THRESHOLD=${1:-80}
  CURRENT_USAGE=$(df -P / | awk 'NR==2 {print $5}' | tr -d '%')

  echo "Root Filesystem Usage: ${CURRENT_USAGE}% (Threshold: ${THRESHOLD}%)"

  if [ "$CURRENT_USAGE" -ge "$THRESHOLD" ]; then
      echo "ALERT: Disk threshold breached! Top 5 consumers in /var:"
      du -sh /var/* 2>/dev/null | sort -hr | head -n 5
  else
      echo "STATUS: Healthy."
  fi
  EOF

  chmod +x disk_watchdog.sh
  ./disk_watchdog.sh 1
  ```

---

### Project 20: Automated Old Log Cleanup Script (`find` + `mtime`)
* **Objective:** Prevent disk exhaustion by rotating/purging aged log files safely.
* **Task:** Create two old log files dated 10 days ago using `touch -d`, then write a command to find and delete `.log` files older than 7 days inside `project_alpha/logs`.
* **Solution:**
  ```bash
  touch -d "10 days ago" project_alpha/logs/old1.log project_alpha/logs/old2.log
  find project_alpha/logs -type f -name "*.log" -mtime +7 -print -delete
  ```

---

### Project 21: Real-Time Log Monitoring with `tail` & `tee`
* **Objective:** Monitor live log streams while simultaneously saving filtered output to an audit file.
* **Task:** Continuously write timestamped lines to `live.log` in the background, and use `tail -f` piped into `grep --line-buffered` and `tee` to capture errors live.
* **Solution:**
  ```bash
  (for i in {1..5}; do echo "$(date) INFO heartbeat $i"; echo "$(date) ERROR fail $i"; sleep 1; done > live.log) &
  timeout 6 tail -f live.log | grep --line-buffered "ERROR" | tee captured_errors.log
  ```

---

### Project 22: Scheduling Tasks with `crontab`
* **Objective:** Understand cron syntax and schedule automated health checks.
* **Task:** Write a valid cron expression that runs `disk_watchdog.sh` every 15 minutes during weekdays (Monday–Friday) and logs output to `/tmp/disk_cron.log`.
* **Solution:**
  ```bash
  # Inspect existing crontab:
  crontab -l 2>/dev/null || echo "No crontab configured"

  # Cron entry format (Min Hour Day Month Weekday Command):
  echo "*/15 * * * 1-5 $HOME/linux_labs/disk_watchdog.sh 85 >> /tmp/disk_cron.log 2>&1"
  ```

---

### Project 23: Parallel File Processing with `xargs`
* **Objective:** Execute commands efficiently across lists of files returned by `find`.
* **Task:** Find all `.txt` and `.conf` files in `~/linux_labs` and count their total lines using `xargs` safely (handling spaces in filenames via `-print0` and `-0`).
* **Solution:**
  ```bash
  find . -type f \( -name "*.txt" -o -name "*.conf" \) -print0 | xargs -0 wc -l
  ```

---

### Project 24: Systemd Service & Journal Log Inspection
* **Objective:** Inspect system services and query structured logs via `systemctl` and `journalctl`.
* **Task:** Check the status of the `ssh` (or `sshd`) service, list all failed systemd units, and view the last 10 kernel log entries.
* **Solution:**
  ```bash
  systemctl is-active ssh sshd 2>/dev/null
  systemctl --failed
  journalctl -k -n 10 --no-pager
  ```

---

### Project 25: Capstone Mini-Project — System Health & Storage Audit Report
* **Objective:** Combine system, network, memory, `df`, and `du` commands into a single reusable server diagnostic script.
* **Task:** Create `server_audit.sh` that generates a timestamped Markdown report (`audit_report.md`) containing Hostname, Uptime, Memory usage, Filesystem usage (`df -h`), Inode usage (`df -i`), and Top 5 largest folders in `/home`.
* **Solution:**
  ```bash
  cat << 'EOF' > server_audit.sh
  #!/bin/bash
  REPORT="audit_report_$(date +%F).md"

  {
      echo "# Server Health & Disk Audit Report"
      echo "- **Generated:** $(date)"
      echo "- **Hostname:** $(hostname)"
      echo "- **Uptime:** $(uptime -p)"
      echo ""
      echo "## Memory Usage"
      echo '```'
      free -h
      echo '```'
      echo ""
      echo "## Filesystem Disk Usage (df -h)"
      echo '```'
      df -hT -x tmpfs -x devtmpfs
      echo '```'
      echo ""
      echo "## Inode Usage (df -i)"
      echo '```'
      df -ih -x tmpfs -x devtmpfs
      echo '```'
      echo ""
      echo "## Top 5 Largest Items in $HOME (du)"
      echo '```'
      du -sh "$HOME"/* 2>/dev/null | sort -hr | head -n 5
      echo '```'
  } > "$REPORT"

  echo "Audit complete! Saved to $REPORT"
  cat "$REPORT"
  EOF

  chmod +x server_audit.sh
  ./server_audit.sh
  ```

---

### Cleanup After Completing All Labs
Once your team finishes all 25 projects, clean up the lab workspace safely:
```bash
cd ~ && rm -rf ~/linux_labs
```
