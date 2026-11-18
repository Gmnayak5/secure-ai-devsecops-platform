
# Linux Grep Hands-On Lab – 44+ Practical Examples for DevOps & SRE

## 1. Introduction

`grep` stands for **Global Regular Expression Print**.

It is a Linux command-line utility used to search text, identify patterns, filter logs, inspect configuration files, and troubleshoot production systems.

In DevOps and SRE environments, grep is commonly used for:

- Searching application logs.
- Identifying errors and warnings.
- Troubleshooting failed deployments.
- Finding failed SSH login attempts.
- Searching source code and configuration files.
- Filtering command outputs.
- Monitoring production logs.
- Investigating security-related events.

### Lab Objectives

By completing this lab, you will learn:

1. Basic grep commands.
2. Case-sensitive and case-insensitive searches.
3. Regular expressions.
4. Pattern matching and filtering.
5. Searching multiple files and directories.
6. Filtering Linux system logs.
7. Troubleshooting production environments.
8. Searching application code and configuration files.

---

# 2. Lab Environment Setup

### Step 1: Create the practice directory

```bash
mkdir -p ~/linux-grep-lab
cd ~/linux-grep-lab
```

### Step 2: Create file.txt

This file contains Linux users, application logs, IP addresses, file paths, configuration entries, errors, and debugging information.

```bash
cat > file.txt <<'EOF'
# Linux Grep Practice Lab
# Created for DevOps and SRE command practice
# End of file

user: alice
user: bob
user: charlie
user: guest
user: admin
User: David
USER: OLDUSER

username: olduser
account: old
status: active
status: inactive
status: failed
status: success

shell: /bin/bash
shell: /bin/sh
shell: /usr/bin/zsh

login: alice from 192.168.1.50
login: bob from 10.0.0.25
login: admin from 172.16.1.100
login: guest from 192.168.1.75

application: nginx
application: apache
application: python3

2024: Application deployment started
2025: Database migration completed
2026: Kubernetes cluster upgraded
2027: Backup scheduled

ERROR: Database connection failed
ERROR: Authentication failed
error: Disk space running low
WARNING: CPU usage above 85 percent
INFO: Application started successfully
DEBUG: Checking database connection
DEBUG: Loading application configuration

failed login attempt for user alice
failed login attempt for user admin
successful login for user bob
authentication failed for user guest

admin has root privileges
alice has standard user privileges
bob has standard user privileges

File path: /etc/nginx/nginx.conf
File path: /var/log/syslog
File path: /home/alice/scripts/deploy.sh
File path: /opt/application/config/settings.yaml

TODO: Configure monitoring alerts
TODO: Implement automated backup
FIXME: Resolve database timeout issue

# SSH configuration examples
# PermitRootLogin prohibit-password
PermitRootLogin no
PasswordAuthentication yes
PubkeyAuthentication yes

# Application configuration
APP_ENV=production
APP_PORT=8080
DB_HOST=192.168.1.100
DB_PORT=5432
LOG_LEVEL=debug

# Deployment information
deployment: production
deployment: staging
deployment: development

service nginx status: running
service docker status: running
service ssh status: active

nginx access: 192.168.1.50 GET /index.html 200
nginx access: 192.168.1.50 GET /login 200
nginx access: 192.168.1.75 POST /login 500
nginx access: 10.0.0.25 GET /api/users 404
nginx access: 192.168.1.50 GET /dashboard 500

SSH Failed password for invalid user hacker from 203.0.113.10
SSH Failed password for alice from 192.168.1.75
SSH Accepted password for bob from 10.0.0.25

kernel: system boot completed
kernel: memory initialized
kernel: panic detected in test environment
kernel: disk I/O warning

process: nginx
process: docker
process: python3
process: sshd

port: 22 ssh
port: 80 nginx
port: 443 https
port: 8080 application
port: 5432 postgres

package: python3
package: openssl
package: nginx
package: docker

# End of practice data
EOF
```

### Step 3: Verify the file

```bash
ls -l file.txt
wc -l file.txt
cat file.txt
```

---

# 3. Basic Searching and Matching

## Example 1: Search for an exact string

```bash
grep "user" file.txt
```

**Explanation:**

Searches for lines containing the string `user`.

**DevOps use case:**

Search application logs for a specific username or user-related activity.

## Example 2: Case-insensitive search

```bash
grep -i "old" file.txt
```

**Explanation:**

The `-i` option ignores uppercase and lowercase differences.

It matches OLD, Old, and old.

**DevOps use case:**

Search logs when error messages have inconsistent capitalization.

## Example 3: Match whole words only

```bash
grep -w "old" file.txt
```

**Explanation:**

Matches the complete word `old`, not partial matches such as `olduser`.

**DevOps use case:**

Avoid unwanted partial matches when searching configuration values or log messages.

## Example 4: Match an exact full line

```bash
grep -x "# End of file" file.txt
```

**Explanation:**

The `-x` option matches the entire line.

**DevOps use case:**

Identify exact configuration entries or markers.

## Example 5: Search multiple patterns

```bash
grep -e "error" -e "debug" file.txt
```

**Explanation:**

Searches for either error or debug.

**DevOps use case:**

Identify errors and debugging information together.

---

# 4. Inverting and Counting Matches

## Example 6: Invert the match

```bash
grep -v "user" file.txt
```

**Explanation:**

The `-v` option displays lines that do not contain `user`.

**DevOps use case:**

Exclude unwanted log entries.

## Example 7: Filter out comments and empty lines

```bash
grep -v -e "^#" -e "^$" file.txt
```

**Explanation:**

- `^#` matches lines starting with a comment.
- `^$` matches empty lines.
- `-v` excludes both.

**DevOps use case:**

Display meaningful configuration entries without comments or blank lines.

## Example 8: Count matching lines

```bash
grep -c "user" file.txt
```

**Explanation:**

Counts the number of lines containing `user`.

**DevOps use case:**

Count occurrences of a specific event in application logs.

## Example 9: Display only matched text

```bash
grep -o "[0-9]\+" file.txt
```

**Explanation:**

The `-o` option displays only the matching portion.

The expression identifies numeric sequences.

**DevOps use case:**

Extract port numbers, IDs, status codes, and numeric values from logs.

## Example 10: Stop after the first two matches

```bash
grep -m 2 "user" file.txt
```

**Explanation:**

Stops searching after two matching lines.

**DevOps use case:**

Quickly inspect a small sample of a large log file.

---

# 5. Line Numbers and Context

## Example 11: Display line numbers

```bash
grep -n "failed" file.txt
```

**Explanation:**

The `-n` option displays line numbers alongside matching lines.

**DevOps use case:**

Identify the exact location of an error in a configuration or log file.

## Example 12: Show two lines after a match

```bash
grep -A 2 "ERROR" file.txt
```

**Explanation:**

`-A` means After.

Displays two lines following every matching line.

**DevOps use case:**

Inspect application events immediately following an error.

## Example 13: Show two lines before a match

```bash
grep -B 2 "status" file.txt
```

**Explanation:**

`-B` means Before.

Displays two preceding lines.

**DevOps use case:**

Inspect events leading up to a failure.

## Example 14: Show one line before and after

```bash
grep -C 1 "DEBUG" file.txt
```

**Explanation:**

`-C` means Context.

Displays one line before and one line after the matching line.

**DevOps use case:**

Understand the sequence of application events around a problem.

## Example 15: Highlight matching text

```bash
grep --color=always "admin" file.txt
```

**Explanation:**

Highlights matching text in the terminal output.

**DevOps use case:**

Quickly identify important usernames, errors, and security events.

---

# 6. Regular Expressions – Anchors and Patterns

## Example 16: Match lines starting with a specific word

```bash
grep "^user" file.txt
```

**Explanation:**

`^` matches the beginning of a line.

**DevOps use case:**

Identify configuration entries beginning with a particular keyword.

## Example 17: Match lines ending with a specific word

```bash
grep "bash$" file.txt
```

**Explanation:**

`$` matches the end of a line.

**DevOps use case:**

Identify users configured with a particular shell.

## Example 18: Match any four-digit number

```bash
grep -E "[0-9]{4}" file.txt
```

**Explanation:**

- `[0-9]` matches a digit.
- `{4}` requires four occurrences.
- `-E` enables extended regular expressions.

**DevOps use case:**

Extract years, four-digit identifiers, or similar numeric patterns.

## Example 19: Match either alice or bob

```bash
grep -E "alice|bob" file.txt
```

**Explanation:**

The `|` operator means OR.

**DevOps use case:**

Search logs for multiple users simultaneously.

## Example 20: Match file paths

```bash
grep -E "/[a-z]+/[a-z]+" file.txt
```

**Explanation:**

Searches for text containing path-like structures.

**DevOps use case:**

Find Linux filesystem paths in configuration files and logs.

---

# 7. Searching Multiple Files and Directories

First create additional files:

```bash
cat > users.txt <<'EOF'
alice
bob
admin
guest
EOF

cat > application.txt <<'EOF'
Application started
ERROR: Application failed
Application restarted
EOF

cat > security.txt <<'EOF'
SSH login successful
Failed password for alice
Failed password for admin
EOF
```

## Example 21: Display filenames containing a pattern

```bash
grep -li "failed" *.txt
```

**Explanation:**

- `-l` displays filenames only.
- `-i` makes the search case-insensitive.
- `*.txt` searches all matching text files.

For a case-sensitive search, use:

```bash
grep -l "failed" *.txt
```

**DevOps use case:**

Identify which log files contain failures.

## Example 22: Display filenames without a pattern

```bash
grep -L "failed" *.txt
```

**Explanation:**

Displays filenames that do not contain the specified pattern.

**DevOps use case:**

Identify log files that do not contain a particular error.

## Example 23: Search recursively

```bash
grep -r "alice" .
```

**Explanation:**

The `-r` option searches recursively through directories.

**DevOps use case:**

Search an entire project or configuration directory for a username or setting.

## Example 24: Suppress error messages

```bash
grep -s "alice" file.txt missing.txt
```

**Explanation:**

The `-s` option suppresses error messages about missing or unreadable files.

**DevOps use case:**

Run scripts that search multiple optional log files without displaying missing-file warnings.

## Example 25: Filter output using pipes

```bash
cat file.txt | grep "user" | grep -v "guest"
```

**Explanation:**

1. `cat` reads the file.
2. First `grep` selects lines containing user.
3. Second `grep` excludes guest.

**DevOps use case:**

Combine multiple filters to narrow down application logs.

A more efficient alternative is:

```bash
grep "user" file.txt | grep -v "guest"
```

---

# 8. Linux and Production Troubleshooting

These examples use actual Linux system commands and logs rather than `file.txt`.

## Example 26: Find whether Nginx is running

```bash
ps aux | grep '[n]ginx'
```

**Explanation:**

- `ps aux` displays running processes.
- `grep` filters Nginx-related processes.
- `[n]ginx` avoids matching the grep command itself.

**DevOps use case:**

Verify whether an Nginx process is running.

## Example 27: Check which process is listening on port 8080

```bash
ss -tulnp | grep 8080
```

**Explanation:**

- `-t`: TCP sockets.
- `-u`: UDP sockets.
- `-l`: Listening sockets.
- `-n`: Numeric addresses and ports.
- `-p`: Process information.

**DevOps use case:**

Troubleshoot applications that cannot bind to a port.

## Example 28: Check whether a package is installed on Ubuntu

```bash
dpkg -l | grep python3
```

**Explanation:**

Searches the installed Debian package listing.

**DevOps use case:**

Verify whether required software packages are installed.

## Example 29: Check whether a package is installed on RHEL/CentOS

```bash
rpm -qa | grep openssl
```

**Explanation:**

- `rpm -qa` lists installed RPM packages.
- `grep` filters the results.

**DevOps use case:**

Verify installed dependencies on Red Hat-based servers.

## Example 30: Check connected USB devices

```bash
lsusb | grep -i "keyboard"
```

**Explanation:**

Lists USB devices and filters for keyboard-related entries.

**DevOps use case:**

Hardware troubleshooting on Linux systems.

## Example 31: Find errors in system logs

```bash
grep -i "error" /var/log/syslog
```

**Explanation:**

Searches system logs for error messages regardless of capitalization.

**DevOps use case:**

Investigate system-level errors.

## Example 32: Monitor live Nginx logs

```bash
tail -f /var/log/nginx/access.log | grep --line-buffered "500"
```

**Explanation:**

- `tail -f` continuously monitors the log.
- `grep` filters HTTP 500 responses.
- `--line-buffered` displays matching lines without waiting for a full output buffer.

**DevOps use case:**

Monitor application failures in real time.

## Example 33: Find failed SSH login attempts

```bash
grep "Failed password" /var/log/auth.log
```

**Explanation:**

Searches authentication logs for failed password attempts.

**DevOps use case:**

Investigate suspicious SSH login activity.

## Example 34: Count requests from a particular IP

```bash
grep -c "192.168.1.50" /var/log/nginx/access.log
```

**Explanation:**

Counts matching log lines containing the specified IP.

**DevOps use case:**

Investigate traffic from a particular client IP.

Note: This counts matching lines, not necessarily individual HTTP requests if a log format contains multiple records per line.

## Example 35: Find kernel panic messages with context

```bash
grep -C 5 "panic" /var/log/kern.log
```

**Explanation:**

Displays five lines before and after the matching message.

**DevOps use case:**

Investigate kernel-related failures.

---

# 9. Advanced DevOps Codebase Searching

Create a sample project:

```bash
mkdir -p project/src project/config project/.git
```

Create a Python file:

```bash
cat > project/src/auth.py <<'EOF'
def authenticateUser(username, password):
    # TODO: Add secure authentication
    return username == "admin"

def validateUser(username):
    return True
EOF
```

Create a configuration file:

```bash
cat > project/config/app.conf <<'EOF'
APP_ENV=production
DB_HOST=192.168.1.100
API_HOST=10.10.10.20
SERVER_IP=172.16.0.15
EOF
```

## Example 41: Find a specific function in a project

```bash
grep -rnw project -e 'authenticateUser'
```

**Explanation:**

- `-r`: Recursive search.
- `-n`: Show line numbers.
- `-w`: Match whole words.
- `-e`: Specify search pattern.

**DevOps use case:**

Find where a function is defined or referenced in a codebase.

## Example 42: Find TODO comments while excluding directories

```bash
grep -rn --exclude-dir={.git,node_modules} "TODO" project
```

**Explanation:**

Searches recursively while excluding `.git` and `node_modules`.

**DevOps use case:**

Find pending development work without searching unnecessary directories.

## Example 43: Search only Python files

```bash
grep -rn --include="*.py" "import requests" project
```

**Explanation:**

Searches only files ending in `.py`.

**DevOps use case:**

Identify dependencies and imports in Python applications.

## Example 44: Find IP addresses across a codebase

```bash
grep -rnE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' project
```

**Explanation:**

Searches for IPv4-like patterns.

**DevOps use case:**

Identify hardcoded IP addresses in source code and configuration files.

**Important:** This is a basic discovery regex, not strict IPv4 validation. It can also match invalid octets such as 999.

---

# 10. Reading Linux Configuration and System Information

## Example 45: View active SSH configuration

```bash
grep -vE "^#|^$" /etc/ssh/sshd_config
```

**Explanation:**

Excludes comments and empty lines.

**DevOps use case:**

Inspect effective-looking configuration entries quickly. Note that included configuration files and `Match` blocks may affect actual SSH settings.

## Example 46: Check CPU model

```bash
grep "model name" /proc/cpuinfo | head -n 1
```

**Explanation:**

Displays the first CPU model-name entry.

**DevOps use case:**

Collect hardware information during server troubleshooting.

## Example 47: Check total RAM

```bash
grep "MemTotal" /proc/meminfo
```

**Explanation:**

Displays total system memory information.

**DevOps use case:**

Collect memory information when diagnosing resource constraints.

## Example 48: Search for an application port

```bash
grep "APP_PORT" file.txt
```

**Explanation:**

Searches the sample configuration data for the application port.

**DevOps use case:**

Identify application configuration values.

---

# 11. Additional Practical Challenges

Try these commands without looking at the explanations.

### Challenge 1: Find all uppercase ERROR entries

```bash
grep "ERROR" file.txt
```

### Challenge 2: Find all error entries regardless of case

```bash
grep -i "error" file.txt
```

### Challenge 3: Find all lines containing IP addresses

```bash
grep -E '[0-9]{1,3}(\.[0-9]{1,3}){3}' file.txt
```

### Challenge 4: Find all HTTP 500 responses

```bash
grep -E ' 500$' file.txt
```

### Challenge 5: Find all lines containing nginx or docker

```bash
grep -E 'nginx|docker' file.txt
```

### Challenge 6: Find all configuration entries starting with APP

```bash
grep '^APP' file.txt
```

### Challenge 7: Find all TODO and FIXME entries

```bash
grep -E 'TODO|FIXME' file.txt
```

### Challenge 8: Display only IP addresses

```bash
grep -oE '[0-9]{1,3}(\.[0-9]{1,3}){3}' file.txt
```

### Challenge 9: Count failed login entries

```bash
grep -ic "failed login" file.txt
```

### Challenge 10: Find lines containing production

```bash
grep -in "production" file.txt
```

---

# 12. Production Troubleshooting Scenarios

| Scenario | Command | Objective |
|---|---|---|
| Application returns errors | `grep -i error application.log` | Find application errors |
| Deployment failed | `grep -in failed deploy.log` | Find deployment failures |
| SSH login failures | `grep "Failed password" /var/log/auth.log` | Investigate failed logins |
| HTTP 500 responses | `grep ' 500 ' access.log` | Identify server errors |
| Database connection problems | `grep -i "database" application.log` | Find database-related events |
| High CPU warnings | `grep -i "CPU" system.log` | Identify CPU-related warnings |
| Find TODO entries | `grep -rn "TODO" .` | Identify unfinished tasks |
| Search configuration | `grep -n "APP_PORT" app.conf` | Locate application settings |
| Find hardcoded IPs | `grep -rnE '[0-9]{1,3}(\.[0-9]{1,3}){3}' .` | Discover IP-like patterns |
| Monitor HTTP errors | `tail -f access.log \| grep --line-buffered ' 500 '` | Watch live failures |

---

# 13. Important grep Options – Quick Reference

| Option | Meaning | Example |
|---|---|---|
| `-i` | Ignore case | `grep -i error file.txt` |
| `-v` | Invert match | `grep -v error file.txt` |
| `-w` | Match whole word | `grep -w user file.txt` |
| `-x` | Match entire line | `grep -x admin file.txt` |
| `-n` | Show line numbers | `grep -n error file.txt` |
| `-c` | Count matching lines | `grep -c error file.txt` |
| `-o` | Show only matched text | `grep -o '[0-9]+' file.txt` |
| `-l` | Show filenames with matches | `grep -l error *.log` |
| `-L` | Show filenames without matches | `grep -L error *.log` |
| `-r` | Recursive search | `grep -r error .` |
| `-E` | Extended regular expressions | `grep -E 'error\|failed' file.txt` |
| `-A` | Lines after match | `grep -A 2 error file.txt` |
| `-B` | Lines before match | `grep -B 2 error file.txt` |
| `-C` | Context around match | `grep -C 2 error file.txt` |
| `-m` | Maximum matching lines | `grep -m 2 error file.txt` |
| `-s` | Suppress error messages | `grep -s error file.txt` |
| `-e` | Specify pattern | `grep -e error -e failed file.txt` |
| `--include` | Include selected filenames | `grep -r --include="*.py" TODO .` |
| `--exclude-dir` | Exclude directories | `grep -r --exclude-dir=.git TODO .` |

---

# 14. GitHub Repository Structure

Recommended structure:

```text
linux-grep-lab/
│
├── README.md
├── file.txt
├── users.txt
├── application.txt
├── security.txt
│
├── project/
│   ├── src/
│   │   └── auth.py
│   │
│   └── config/
│       └── app.conf
│
└── screenshots/
    ├── grep-basic.png
    ├── grep-regex.png
    ├── grep-logs.png
    └── grep-codebase.png
```

---

# 15. Hands-on Learning Checklist

Track your progress as you execute the commands.

- [ ] Create the Linux grep lab.
- [ ] Create and inspect file.txt.
- [ ] Complete basic search examples.
- [ ] Practise case-insensitive searches.
- [ ] Practise whole-word matching.
- [ ] Practise exact-line matching.
- [ ] Practise inverted searches.
- [ ] Practise counting matching lines.
- [ ] Practise regular expressions.
- [ ] Practise line numbers and context.
- [ ] Search multiple files.
- [ ] Search directories recursively.
- [ ] Practise include and exclude options.
- [ ] Filter Linux command outputs.
- [ ] Inspect running processes.
- [ ] Inspect listening ports.
- [ ] Search Linux system logs.
- [ ] Monitor live application logs.
- [ ] Search source code.
- [ ] Search configuration files.
- [ ] Complete the troubleshooting challenges.
- [ ] Capture terminal screenshots.
- [ ] Push the completed lab to GitHub.

---

# 16. How to Showcase This on LinkedIn

Once you complete the lab, document your learning with:

1. The GitHub repository URL.
2. Screenshots of actual command executions.
3. Examples of errors identified using grep.
4. Examples of searching application logs.
5. Examples of finding hardcoded IP addresses.
6. Examples of recursive codebase searching.
7. Production troubleshooting scenarios.

**Learning outcome:**

I have practised Linux `grep` commands using a hands-on lab covering basic text searches, regular expressions, Linux system logs, application troubleshooting, configuration inspection, and DevOps codebase searches.

The objective is to understand not only what each command does, but also how it can be applied in real-world DevOps, SRE, and Platform Engineering environments.

---

**Practice principle:** Execute every command, inspect the actual output, understand why it works, and identify one real-world DevOps use case.

Do not just memorize commands. Learn to troubleshoot with them.
