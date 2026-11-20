
# Linux System Administration Fundamentals and Production Troubleshooting

**Training Duration:** 2 Hours  
**Operating System:** Ubuntu Server 22.04 / 24.04 LTS  
**Shell:** Bash  
**Audience:** Freshers, beginners and non-IT students  
**Training Type:** Theory + Live Demonstration + Hands-on Practice

---

# 1. Two-Hour Training Agenda

| Time | Topic | Practical Focus |
|---|---|---|
| 0–10 min | Linux System Administration overview | Understanding administration |
| 10–25 min | System information | Identifying server health |
| 25–40 min | User profiles and environment variables | `.bashrc`, `.profile`, PATH |
| 40–60 min | File ownership and permissions | `chown`, `chmod`, `stat` |
| 60–80 min | Network troubleshooting | `ss`, IP address, ports |
| 80–95 min | HTTP and API testing | `curl` |
| 95–105 min | Process and service management | `top`, `kill`, `systemctl` |
| 105–112 min | Log management | `journalctl`, `grep`, `tail` |
| 112–120 min | Combined troubleshooting lab | Application troubleshooting |

# 2. Linux System Administration – Understanding the Basics

## 2.1 What is Linux System Administration?

Imagine you manage a large office building.

You need to:
- Manage who can enter the building.
- Decide which rooms people can access.
- Ensure electricity and water are available.
- Monitor security.
- Fix problems when something stops working.

A Linux System Administrator performs similar responsibilities, but instead of managing a building, they manage Linux servers.

A Linux System Administrator is responsible for:
- Managing Linux users.
- Managing file permissions.
- Monitoring system resources.
- Managing running applications.
- Checking network connectivity.
- Managing services.
- Troubleshooting server problems.
- Monitoring system logs.

### Real-world example

Imagine an IT company has an application running on a Linux server.

An employee reports:

"Our application is not opening."

The Linux administrator investigates:

1. Is the Linux server running?
2. Is the application running?
3. Is the required network port listening?
4. Does the application have the correct permissions?
5. Are there any errors in application logs?
6. Is the application responding to HTTP requests?

These are everyday system administration activities.

## 2.2 How Linux System Administration Connects to DevOps

| Linux Administration | DevOps Application |
|---|---|
| User management | Managing deployment users |
| File permissions | Securing application directories |
| Environment variables | Configuring application environments |
| Network troubleshooting | Troubleshooting application connectivity |
| Process management | Monitoring application processes |
| Service management | Managing application services |
| Log management | Investigating deployment failures |

**Remember:** Linux administration is one of the foundations of DevOps engineering.

---

# 3. Understanding Linux System Information

Before troubleshooting a Linux server, we must understand its current condition.

For example, if an application is slow, we may need to investigate:

- How long has the server been running?
- How much memory is available?
- How much disk space is available?
- What is the hostname?
- Which user is logged in?
- What processes are consuming resources?

Let's explore the commands.

## 3.1 whoami – Identify the Current User

### What does it do?

Displays the username of the currently logged-in user.

### Command

```bash
whoami
```

### Expected output

```text
dheeraj
```

### Explanation

The output tells us which Linux user is currently executing commands.

### Why is it useful?

Suppose you are logged into a Linux server and trying to modify an application file.

Before making changes, you should understand which user you are using.

### Practice

```bash
whoami
```

Find your current username.

## 3.2 id – Display User and Group Information

### What does it do?

Displays the user ID (UID), primary group ID (GID), and supplementary group memberships.

### Command

```bash
id
```

### Illustrative output

```text
uid=1000(dheeraj) gid=1000(dheeraj) groups=1000(dheeraj),27(sudo)
```

### Understanding the output

| Field | Meaning |
|---|---|
| uid | User ID |
| gid | Primary group ID |
| groups | Groups to which the user belongs |
| sudo | Indicates membership in the sudo group |

### What are UID and GID?

- UID identifies a Linux user internally.
- GID identifies a Linux group internally.

Think of UID as an employee ID and GID as a department ID.

### Production example

A developer reports that they cannot execute an administrative command.

The administrator checks:

```bash
id developer
```

This helps determine whether the developer belongs to the required groups.

## 3.3 hostname – Identify the Server Name

Every Linux server can have a hostname.

A company may have hundreds of servers. Hostnames help administrators identify them.

### Command

```bash
hostname
```

Example output:

```text
ubuntu-server
```

### Production example

Imagine these servers:

```text
dev-server-01
test-server-01
prod-server-01
database-server-01
```

The hostname helps administrators understand which server they are connected to.

## 3.4 hostnamectl – Display Host Information

### Command

```bash
hostnamectl
```

Illustrative output:

```text
 Static hostname: ubuntu-server
       Icon name: computer-vm
         Chassis: vm
      Machine ID: xxxxxxxxx
         Boot ID: xxxxxxxxx
  Virtualization: oracle
Operating System: Ubuntu 24.04 LTS
          Kernel: Linux 6.x
    Architecture: x86-64
```

### Explanation

| Field | Meaning |
|---|---|
| Static hostname | Configured hostname |
| Operating System | Installed Linux distribution |
| Kernel | Linux kernel version |
| Architecture | CPU architecture |
| Virtualization | Virtual machine environment, when detected |

## 3.5 uptime – Check Server Running Time

### Command

```bash
uptime
```

Illustrative output:

```text
15:30:10 up 12 days, 4:20, 2 users, load average: 0.20, 0.35, 0.40
```

### Understanding the output

| Field | Meaning |
|---|---|
| 15:30:10 | Current system time |
| up 12 days | Server has been running for 12 days |
| 4:20 | Time since last boot |
| 2 users | Number of logged-in sessions |
| load average | Average system load over 1, 5 and 15 minutes |

### What is load average?

Load average indicates the average number of tasks running or waiting for CPU time.

A high load average may indicate that the system is under pressure, but it must be interpreted relative to CPU count and workload.

Check CPU count:

```bash
nproc
```

### Production scenario

A server is unexpectedly slow.

The administrator executes:

```bash
uptime
```

This provides an initial indication of system load.

## 3.6 date – Display Date and Time

```bash
date
```

Example:

```text
Fri Nov 20 15:30:10 IST 2026
```

Why is this important?

Incorrect server time can affect:

- Application logs.
- Scheduled jobs.
- Authentication.
- SSL certificate validation.
- Distributed application communication.

## 3.7 free -h – Check Memory Usage

### What is RAM?

RAM is temporary working memory used by applications and the operating system.

Imagine RAM as your office desk.

The larger your desk, the more documents you can work with simultaneously.

### Command

```bash
free -h
```

Illustrative output:

```text
               total        used        free      shared  buff/cache   available
Mem:            7.7Gi       2.1Gi       3.0Gi       100Mi       2.6Gi       5.2Gi
Swap:           2.0Gi          0B       2.0Gi
```

### Understanding the output

| Field | Meaning |
|---|---|
| total | Total RAM |
| used | Memory currently used |
| free | Completely unused memory |
| shared | Shared memory |
| buff/cache | Memory used for buffers and cache |
| available | Estimated memory available for new applications |

**Important:** Linux uses available RAM for caching. Low `free` memory does not automatically mean that the server is running out of memory.

### Production scenario

An application is experiencing memory-related problems.

The administrator executes:

```bash
free -h
```

This gives an overview of available memory.

## 3.8 df -h – Check Disk Space

### What is disk space?

Disk space is the storage capacity available to store files.

Think of it as the storage room in an office.

### Command

```bash
df -h
```

Illustrative output:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   35G   15G  70% /
tmpfs           3.8G     0  3.8G   0% /dev/shm
```

### Important fields

| Field | Meaning |
|---|---|
| Filesystem | Storage device or filesystem |
| Size | Total capacity |
| Used | Used storage |
| Avail | Available storage |
| Use% | Percentage used |
| Mounted on | Location where filesystem is accessible |

### Production scenario

An application fails to write log files.

One possible reason is that the filesystem is full.

Execute:

```bash
df -h
```

Check whether any filesystem is approaching 100% usage.

## 3.9 lsblk – Display Storage Devices

### Command

```bash
lsblk
```

Illustrative output:

```text
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda      8:0    0   50G  0 disk
└─sda1   8:1    0   50G  0 part /
sdb      8:16   0  100G  0 disk
```

### Explanation

- `sda` represents a disk.
- `sda1` represents a partition.
- `sdb` represents another disk.
- `MOUNTPOINTS` shows where storage is attached to the Linux filesystem.

## 3.10 top – Monitor Running Processes

### Command

```bash
top
```

This opens a live system-monitoring screen.

It displays:
- CPU utilization.
- Memory utilization.
- Running processes.
- Process IDs.
- Resource consumption.

Press:

```text
q
```

to exit.

### Production scenario

A server is running slowly.

The administrator executes:

```bash
top
```

They investigate whether a process is consuming excessive CPU or memory.

---

# 4. Linux User Profiles and Environment Variables

## 4.1 What is a Linux User Profile?

A user profile contains configuration settings associated with a Linux user.

When you log into Linux, the shell can load configuration files that define:

- Environment variables.
- Default command paths.
- Aliases.
- Shell preferences.
- Custom configurations.

### Real-life analogy

Imagine joining a company.

You receive:
- An employee ID.
- A desk.
- A computer.
- Access permissions.
- Your preferred workspace settings.

Similarly, Linux provides each user with a home directory and user-specific configuration.

## 4.2 What is a Home Directory?

Every normal Linux user generally has a home directory.

Example:

```text
/home/dheeraj
/home/student1
/home/student2
```

Check your home directory:

```bash
echo $HOME
```

Check your current directory:

```bash
pwd
```

### Difference between /home and /root

| Directory | Purpose |
|---|---|
| `/home/dheeraj` | Home directory of normal user |
| `/home/student1` | Home directory of student1 |
| `/root` | Home directory of root user |

The root user is the Linux superuser with extensive administrative privileges.

## 4.3 What are Environment Variables?

Environment variables are named values that applications and processes can use to obtain configuration information.

Think of them as configuration settings.

For example:

```text
JAVA_HOME=/usr/lib/jvm/java-21
```

This tells applications where Java is installed.

Other examples:

```text
HOME=/home/dheeraj
USER=dheeraj
SHELL=/bin/bash
```

## 4.4 Understanding Important Environment Variables

### HOME

```bash
echo $HOME
```

Displays the current user's home directory.

### USER

```bash
echo $USER
```

Displays the current username in a typical Bash login environment.

### SHELL

```bash
echo $SHELL
```

Displays the user's configured login shell.

### PATH

```bash
echo $PATH
```

Displays directories Linux searches when you execute a command without specifying its full path.

### Real-life analogy for PATH

Imagine a library with multiple sections.

If you want a particular book, you search the designated sections.

Similarly, Linux searches the directories listed in PATH to locate executable commands.

Example:

```text
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

## 4.5 env – Display Environment Variables

```bash
env
```

Displays the environment variables available to the current process.

## 4.6 printenv – Display Environment Variable Values

```bash
printenv
```

Displays environment variables.

To check a particular variable:

```bash
printenv HOME
printenv PATH
printenv USER
```

## 4.7 Creating a Custom Environment Variable

Let's create a variable called COMPANY.

```bash
COMPANY="ABC Technologies"
```

Display it:

```bash
echo $COMPANY
```

Expected output:

```text
ABC Technologies
```

### Important: Shell variable versus environment variable

A variable created using:

```bash
COMPANY="ABC Technologies"
```

is a shell variable.

To export it into the environment of child processes:

```bash
export COMPANY="ABC Technologies"
```

Check:

```bash
printenv COMPANY
```

Expected output:

```text
ABC Technologies
```

## 4.8 Temporary versus Persistent Variables

A temporary variable exists only in the current shell session unless exported or otherwise passed to another process.

Example:

```bash
export TRAINING="Linux"
echo $TRAINING
```

After closing that shell session, the variable normally disappears.

To make it persistent for future Bash sessions, add it to an appropriate shell configuration file.

## 4.9 Understanding Linux Profile Files

Linux uses different configuration files for different purposes.

| File | Purpose |
|---|---|
| `/etc/profile` | System-wide login-shell configuration |
| `~/.profile` | User-specific login configuration |
| `~/.bashrc` | User-specific interactive Bash configuration |
| `/etc/bash.bashrc` | System-wide interactive Bash configuration on Ubuntu |
| `/etc/environment` | System-wide environment variable assignments |

### A. ~/.bashrc

This file is commonly used to configure interactive Bash sessions.

Open it:

```bash
nano ~/.bashrc
```

Add:

```bash
export TRAINING="Linux Administration"
```

Save and exit.

Reload:

```bash
source ~/.bashrc
```

Verify:

```bash
echo $TRAINING
```

Expected output:

```text
Linux Administration
```

### B. ~/.profile

This file is commonly used for user-specific login configuration.

Open:

```bash
nano ~/.profile
```

You can add login-related environment settings here.

### C. /etc/profile

This is a system-wide login-shell configuration file.

Inspect it:

```bash
cat /etc/profile
```

Avoid modifying system-wide files without understanding the impact.

### D. /etc/environment

This file is commonly used for system-wide environment variable assignments.

Inspect:

```bash
cat /etc/environment
```

Example assignment:

```text
JAVA_HOME=/usr/lib/jvm/java-21
```

Do not use shell commands such as `export` in this file.

### E. /etc/bash.bashrc

This file contains system-wide interactive Bash configuration on Ubuntu.

Inspect:

```bash
cat /etc/bash.bashrc
```

## 4.10 Login Shell versus Non-login Shell

A login shell is started as part of a login session.

A non-login shell is started without a new login session.

Examples:

- Logging into a server through SSH generally starts a login shell.
- Opening another Bash shell inside an existing terminal commonly starts a non-login shell.

The files loaded depend on how Bash is started.

## 4.11 Interactive versus Non-interactive Shell

An interactive shell allows the user to enter commands.

Example:

```bash
bash
```

A non-interactive shell executes commands or scripts without waiting for the user to interact.

Example:

```bash
bash -c 'echo Hello'
```

Expected output:

```text
Hello
```

### Why does this matter in DevOps?

Imagine a developer executes a script manually.

It works.

But the same script fails when executed through Jenkins.

One possible reason is that the Jenkins process does not receive the same environment variables or PATH configuration as the developer's interactive shell.

This is why environment configuration matters in automation.

---

# 5. Linux File Ownership and Permissions – CHOWN

## 5.1 What is File Ownership?

Every Linux file and directory has an owner and a group.

For example:

```text
-rw-r--r-- 1 dheeraj developers 1200 Nov 20 10:00 application.conf
```

Here:

- Owner: dheeraj
- Group: developers
- File: application.conf

### Real-life analogy

Imagine a company office.

- Owner = Employee responsible for a document.
- Group = Department that can access the document.
- Others = Everyone else.

Linux uses ownership and permissions to control access.

## 5.2 Understanding Linux Permissions

Consider:

```text
-rwxr-xr--
```

Break it into sections:

```text
- | rwx | r-x | r--
    Owner Group Others
```

| Symbol | Meaning |
|---|---|
| r | Read |
| w | Write |
| x | Execute |
| - | Permission not granted |

### Read permission

Allows reading file contents.

### Write permission

Allows modifying file contents.

### Execute permission

Allows executing a file when applicable.

For directories, permissions have different practical meanings:

- Read: List directory entries.
- Write: Create, delete or rename entries, subject to other permissions.
- Execute: Access/traverse the directory.

## 5.3 ls -l – Check Ownership and Permissions

Command:

```bash
ls -l
```

Illustrative output:

```text
-rw-r--r-- 1 dheeraj developers 1200 Nov 20 10:00 app.conf
```

| Section | Meaning |
|---|---|
| `-` | Regular file |
| `rw-` | Owner permissions |
| `r--` | Group permissions |
| `r--` | Others permissions |
| `dheeraj` | Owner |
| `developers` | Group |
| `1200` | File size in bytes |

## 5.4 groups – Check Group Membership

```bash
groups
```

Displays the groups associated with the current user.

To check another user:

```bash
groups student1
```

## 5.5 stat – Detailed File Information

```bash
stat application.conf
```

Illustrative output:

```text
File: application.conf
Size: 1200
Access: (0644/-rw-r--r--)
Uid: (1000/dheeraj)
Gid: (1001/developers)
```

This provides detailed information about the file.

## 5.6 What is CHOWN?

`chown` stands for Change Owner.

It is used to change the owner and optionally the group of a file or directory.

### Syntax

```bash
sudo chown username filename
```

Example:

```bash
sudo chown student1 application.conf
```

This changes the owner of the file to student1.

### Change owner and group together

```bash
sudo chown student1:developers application.conf
```

This changes:

- Owner to student1.
- Group to developers.

### Change ownership recursively

```bash
sudo chown -R student1:developers /opt/training/
```

The `-R` option means recursive.

It applies the ownership change to the directory and its contents.

**Warning:** Recursive ownership changes can cause serious problems if applied to system directories or application directories without understanding their requirements.

## 5.7 What is CHGRP?

`chgrp` means Change Group.

Example:

```bash
sudo chgrp developers application.conf
```

This changes the group ownership without changing the file owner.

## 5.8 What is CHMOD?

`chmod` means Change Mode.

It changes file or directory permissions.

Example:

```bash
chmod 755 script.sh
```

## 5.9 Understanding Numeric Permissions

Linux uses three numeric values for owner, group and others.

| Permission | Number |
|---|---|
| Read | 4 |
| Write | 2 |
| Execute | 1 |

Add these values to calculate permissions.

Example:

```text
7 = 4 + 2 + 1 = rwx
6 = 4 + 2 = rw-
5 = 4 + 1 = r-x
4 = 4 = r--
```

### Example 1: chmod 755

```bash
chmod 755 script.sh
```

Meaning:

```text
Owner  = 7 = rwx
Group  = 5 = r-x
Others = 5 = r-x
```

### Example 2: chmod 644

```bash
chmod 644 application.conf
```

Meaning:

```text
Owner  = 6 = rw-
Group  = 4 = r--
Others = 4 = r--
```

### Example 3: chmod 700

```bash
chmod 700 private.sh
```

Meaning:

```text
Owner  = 7 = rwx
Group  = 0 = ---
Others = 0 = ---
```

Only the owner receives permissions.

## 5.10 Practical CHOWN and CHMOD Lab

Create a directory:

```bash
mkdir linux-permission-lab
```

Create a file:

```bash
touch linux-permission-lab/app.conf
```

Check ownership:

```bash
ls -l linux-permission-lab/
```

Create a user:

```bash
sudo adduser student1
```

Change ownership:

```bash
sudo chown student1 linux-permission-lab/app.conf
```

Check:

```bash
ls -l linux-permission-lab/
```

Change permissions:

```bash
sudo chmod 640 linux-permission-lab/app.conf
```

Verify:

```bash
stat linux-permission-lab/app.conf
```

### Production example

An application runs using a dedicated Linux account called `appuser`.

The application files are stored under:

```text
/opt/myapplication/
```

If the application user does not have the required permissions, the application may fail to read configuration files or write logs.

An administrator investigates:

```bash
ls -ld /opt/myapplication
ls -l /opt/myapplication/
```

Then checks ownership and permissions before making any changes.

---

# 6. Linux Network Troubleshooting – SS Command

## 6.1 What is a Computer Network?

A computer network allows computers and servers to communicate with one another.

For example:

Your laptop connects to a Linux server.

The Linux server runs an application.

Your browser sends a request to that application.

The server responds with the requested information.

## 6.2 What is an IP Address?

An IP address identifies a network interface on an IP network.

Example:

```text
192.168.1.10
```

Think of an IP address as a building address.

It helps identify where network traffic should be delivered.

## 6.3 What is TCP?

TCP stands for Transmission Control Protocol.

TCP provides reliable, ordered communication between applications.

Examples:

- HTTPS communication.
- SSH connections.
- Many database connections.

## 6.4 What is UDP?

UDP stands for User Datagram Protocol.

UDP sends datagrams without providing TCP-style delivery guarantees.

Examples:

- DNS queries in common configurations.
- Certain real-time communication applications.
- Streaming and gaming protocols.

## 6.5 What is a Network Port?

A port identifies a communication endpoint used by an application.

Common examples:

| Port | Common Usage |
|---|---|
| 22 | SSH |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 5432 | PostgreSQL |
| 8080 | Common alternative web application port |

### Real-life analogy

Imagine a hotel.

- IP address = Hotel address.
- Port = Room number.
- Application = Service operating in that room.
- Network connection = Communication between two endpoints.

## 6.6 What is a Listening Port?

A listening port means an application is waiting for incoming network connections.

For example, a web application may listen on:

```text
0.0.0.0:8080
```

This means it is listening on port 8080 on all IPv4 interfaces.

Another example:

```text
127.0.0.1:8080
```

This means it is listening only on the local loopback interface.

External machines generally cannot directly connect to a service bound only to loopback.

## 6.7 What is an Established Connection?

An established connection indicates that a TCP connection has been established between two endpoints.

Example:

A browser connects to a web server.

Both endpoints maintain connection information while communicating.

## 6.8 What is a Socket?

A socket is a communication endpoint used by applications.

Linux uses sockets for network communication.

The `ss` command helps administrators inspect socket information.

## 6.9 What is SS?

`ss` stands for Socket Statistics.

It is used to display socket information, including listening ports and active connections.

It is commonly used for Linux network troubleshooting.

### Basic syntax

```bash
ss [options]
```

## 6.10 Understanding SS Commands

### Example 1: ss

```bash
ss
```

Displays socket information, commonly focusing on established TCP connections by default.

### Example 2: ss -t

```bash
ss -t
```

`-t` means TCP.

Displays TCP sockets.

### Example 3: ss -u

```bash
ss -u
```

`-u` means UDP.

Displays UDP sockets.

### Example 4: ss -l

```bash
ss -l
```

`-l` means listening.

Displays listening sockets.

### Example 5: ss -n

```bash
ss -n
```

`-n` means numeric.

Displays numeric addresses and port numbers instead of resolving names where applicable.

### Example 6: ss -tuln

```bash
ss -tuln
```

Breakdown:

| Option | Meaning |
|---|---|
| -t | TCP |
| -u | UDP |
| -l | Listening |
| -n | Numeric |

This is one of the most useful Linux administration commands.

It displays listening TCP and UDP sockets.

Illustrative output:

```text
Netid State  Recv-Q Send-Q Local Address:Port Peer Address:Port
tcp   LISTEN 0      128    0.0.0.0:22        0.0.0.0:*
tcp   LISTEN 0      128    0.0.0.0:8080      0.0.0.0:*
udp   UNCONN 0      0      127.0.0.53:53     0.0.0.0:*
```

### Understanding the fields

| Field | Explanation |
|---|---|
| Netid | Network protocol |
| State | Socket state |
| Recv-Q | Receive queue information |
| Send-Q | Send queue information |
| Local Address:Port | Local listening address and port |
| Peer Address:Port | Remote endpoint |

For listening sockets, Send-Q commonly represents the configured connection backlog limit.

### Example 7: ss -tulnp

```bash
sudo ss -tulnp
```

This displays listening TCP and UDP sockets with numeric addresses and process information where available.

`-p` displays process information.

Example:

```text
tcp LISTEN 0 128 0.0.0.0:8080 0.0.0.0:* users:(("python3",pid=1234,fd=3))
```

Here:

- Application: python3
- PID: 1234
- Port: 8080

### Example 8: ss -tan

```bash
ss -tan
```

Displays TCP sockets with numeric addresses, including listening and non-listening TCP sockets.

### Example 9: ss -s

```bash
ss -s
```

Displays socket summary statistics.

Useful for obtaining an overview of socket activity.

## 6.11 Troubleshooting with SS

### Scenario 1: Application is not accessible

An application is expected to listen on port 8080.

Check:

```bash
sudo ss -tulnp
```

Look for port 8080.

If it is missing, investigate whether the application is running and configured correctly.

### Scenario 2: Port 8080 is not listening

```bash
sudo ss -tulnp | grep 8080
```

If no matching output appears, there may be no listening socket on that port.

Check the application service and logs.

### Scenario 3: Application listens only on localhost

Example:

```text
127.0.0.1:8080
```

The application accepts local connections only.

If external access is required, review the application bind-address configuration.

### Scenario 4: Two applications use the same port

Two applications generally cannot bind the same IP address and TCP port combination in the ordinary case.

The second application may fail to start with an address-in-use error.

Check:

```bash
sudo ss -tulnp
```

Identify which process owns the port.

### Scenario 5: Service is running but inaccessible

Check:

```bash
sudo ss -tulnp
```

Then investigate:

- Whether the application is listening.
- Whether it is listening on the correct interface.
- Firewall rules.
- Network routing.
- Cloud security groups.
- Application health.

## 6.12 Related Networking Commands

### ip addr

```bash
ip addr
```

Displays network interfaces and IP addresses.

### ip route

```bash
ip route
```

Displays the routing table.

### hostname -I

```bash
hostname -I
```

Displays IP addresses assigned to the host.

### ping

```bash
ping -c 4 8.8.8.8
```

Sends four ICMP echo requests.

A failed ping does not automatically mean the server is down. ICMP traffic may be blocked by a firewall or network policy.

---

# 7. HTTP and API Testing – CURL Command

## 7.1 What is HTTP?

HTTP stands for Hypertext Transfer Protocol.

It is a communication protocol used by clients and servers.

Example:

You open a website in your browser.

Your browser sends an HTTP request.

The web server processes the request and sends an HTTP response.

## 7.2 What is HTTPS?

HTTPS is HTTP protected using TLS encryption.

It helps protect communication between the client and server.

## 7.3 What is a URL?

A URL is a web address.

Example:

```text
https://example.com/products
```

It identifies a resource that a client wants to access.

## 7.4 What is an API?

API stands for Application Programming Interface.

An API allows one application to communicate with another application.

Example:

A mobile application needs customer information.

It sends an API request to a backend server.

The backend returns customer information.

## 7.5 Understanding Request and Response

A client sends a request.

A server sends a response.

```text
Client -------------- Request -------------> Server

Client <------------- Response ------------- Server
```

A request may contain:

- URL.
- HTTP method.
- Headers.
- Request body.

A response may contain:

- Status code.
- Response headers.
- Response body.

## 7.6 What is CURL?

`curl` is a command-line tool used to transfer data using different network protocols.

In DevOps, it is commonly used to:

- Test application URLs.
- Test APIs.
- Verify application health.
- Download files.
- Investigate HTTP responses.
- Check connectivity.

## 7.7 Basic CURL Command

```bash
curl https://example.com
```

This sends an HTTP GET request by default.

It displays the response body.

## 7.8 CURL -I – Display Response Headers

```bash
curl -I https://example.com
```

`-I` requests headers only.

Illustrative output:

```text
HTTP/2 200
content-type: text/html
content-length: 1256
```

This is useful when checking whether a web server is responding.

## 7.9 CURL -v – Verbose Mode

```bash
curl -v https://example.com
```

`-v` means verbose.

It displays additional connection and HTTP communication details.

Useful when investigating:

- DNS resolution.
- TCP connection failures.
- TLS handshake issues.
- HTTP request and response headers.

Do not share verbose output publicly without checking for sensitive information.

## 7.10 CURL -o – Save Output to a File

```bash
curl -o output.html https://example.com
```

`-o` saves the response body using the specified filename.

Check:

```bash
ls -l output.html
```

## 7.11 CURL -O – Save Using the Remote Filename

```bash
curl -O https://example.com/index.html
```

`-O` uses the filename from the URL path.

## 7.12 CURL -L – Follow Redirects

```bash
curl -L https://example.com
```

`-L` follows HTTP redirects.

Example:

A website may redirect an HTTP request to an HTTPS URL.

## 7.13 CURL -s – Silent Mode

```bash
curl -s https://example.com
```

`-s` suppresses progress and error messages.

Useful in shell scripts.

## 7.14 Display Only the HTTP Status Code

```bash
curl -w "%{http_code}\n" -o /dev/null -s https://example.com
```

Expected output:

```text
200
```

Explanation:

| Option | Purpose |
|---|---|
| -w | Write additional output |
| `%{http_code}` | Display HTTP response status |
| -o | Save or discard response body |
| `/dev/null` | Discard response body on Linux |
| -s | Silent mode |

This is particularly useful in DevOps automation.

## 7.15 Understanding HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve information |
| POST | Submit data or request creation |
| PUT | Replace a resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |

### GET request

```bash
curl -X GET https://httpbin.org/get
```

### POST request

```bash
curl -X POST https://httpbin.org/post
```

`-X` specifies the HTTP method.

## 7.16 Sending JSON Data Using CURL

JSON is a common data format used by APIs.

Example:

```bash
curl -X POST https://httpbin.org/post \
-H "Content-Type: application/json" \
-d '{"name":"student","role":"devops"}'
```

### Explanation

| Option | Meaning |
|---|---|
| -X POST | Use POST method |
| -H | Add an HTTP header |
| Content-Type | Identify the request body format |
| -d | Send request data |

The endpoint returns information about the request.

You should see the submitted JSON in the response.

## 7.17 HTTP Status Codes

| Code | Meaning |
|---|---|
| 200 | Successful request |
| 201 | Resource created |
| 301 | Permanent redirect |
| 302 | Temporary redirect |
| 400 | Bad request |
| 401 | Authentication required or invalid |
| 403 | Forbidden |
| 404 | Resource not found |
| 500 | Internal server error |
| 502 | Bad gateway |
| 503 | Service unavailable |

### Real-world troubleshooting examples

**404 – Not Found**

The server could not find the requested resource.

Check the URL and application routing.

**500 – Internal Server Error**

The application encountered an internal error.

Check application logs.

**502 – Bad Gateway**

A gateway or reverse proxy received an invalid response from an upstream server.

Check the backend application and proxy configuration.

**503 – Service Unavailable**

The service is temporarily unable to handle the request.

Check application availability and server capacity.

## 7.18 Production DevOps Example

Imagine a Jenkins pipeline deploys an application.

After deployment, Jenkins needs to verify that the application is healthy.

A simple health check:

```bash
curl -f http://localhost:8080/health
```

The `-f` option makes curl return a failure status for HTTP responses with error status codes (400 and above).

A pipeline can use this result to decide whether deployment validation succeeded.

Example:

```bash
if curl -fsS http://localhost:8080/health; then
    echo "Application is healthy"
else
    echo "Application health check failed"
    exit 1
fi
```

This is a simple example of automated application health validation.

---

# 8. Process and Service Management

## 8.1 What is a Process?

A process is an instance of a running program.

For example:

When you start a browser, Linux creates a process.

When you execute a Python script, Linux creates a process.

Every process has a Process ID (PID).

Example:

```text
PID  COMMAND
1234 python3
```

The PID helps administrators identify and manage processes.

## 8.2 What is a Service?

A service is a program that provides functionality to other programs or users.

Many Linux services run in the background.

Examples:

- SSH service.
- Web server.
- Database service.
- Monitoring agent.

### Difference between a Process and a Service

| Process | Service |
|---|---|
| Running instance of a program | Background functionality managed as a service |
| Has a PID | Often managed by systemd |
| May be short-lived | Often intended to run continuously |
| Can run in foreground or background | Commonly runs in background |

A service is implemented by one or more processes, but not every process is a system service.

## 8.3 What is systemd?

systemd is a system and service manager used by Ubuntu.

It helps Linux:

- Start services.
- Stop services.
- Restart services.
- Monitor service states.
- Start services during boot.

The `systemctl` command communicates with systemd.

## 8.4 top – Monitor Processes

```bash
top
```

Displays live process and system-resource information.

Press `q` to exit.

## 8.5 htop – Interactive Process Monitoring

```bash
htop
```

Displays processes in an interactive interface.

If not installed:

```bash
sudo apt update
sudo apt install htop
```

## 8.6 pgrep – Find a Process

```bash
pgrep python
```

Displays process IDs matching the process name pattern.

For process names and IDs:

```bash
pgrep -a python
```

## 8.7 pidof – Find Process ID

```bash
pidof sshd
```

Displays the PID of the named running program, if found.

## 8.8 kill – Send a Signal to a Process

The `kill` command sends a signal to a process.

Example:

```bash
kill 1234
```

By default, this sends SIGTERM.

SIGTERM asks the process to terminate gracefully.

### kill -15

```bash
kill -15 1234
```

Sends SIGTERM explicitly.

This gives the process an opportunity to shut down cleanly.

### kill -9

```bash
kill -9 1234
```

Sends SIGKILL.

This forcibly terminates the process.

**Important:** Do not use `kill -9` as your first troubleshooting step. It does not allow the process to perform its normal cleanup.

Use SIGTERM first whenever appropriate.

## 8.9 systemctl status – Check Service Status

```bash
systemctl status ssh
```

Displays the status of the SSH service.

Illustrative output:

```text
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded
     Active: active (running)
```

Important states:

| State | Meaning |
|---|---|
| active (running) | Service is running |
| inactive (dead) | Service is not running |
| failed | Service failed |
| activating | Service is starting |
| deactivating | Service is stopping |

Ubuntu installations may use different service names depending on the installed software.

## 8.10 systemctl list-units

```bash
systemctl list-units --type=service
```

Lists loaded service units.

To include inactive loaded services:

```bash
systemctl list-units --type=service --all
```

## 8.11 systemctl start

```bash
sudo systemctl start ssh
```

Starts the SSH service.

## 8.12 systemctl stop

```bash
sudo systemctl stop ssh
```

Stops the SSH service.

**Warning:** Never stop SSH on a remote production server unless you have verified an alternative management path. You may lose remote access.

## 8.13 systemctl restart

```bash
sudo systemctl restart ssh
```

Stops and starts the service again.

Useful after certain configuration changes.

## 8.14 systemctl enable

```bash
sudo systemctl enable ssh
```

Configures the service to start automatically during boot.

## 8.15 systemctl disable

```bash
sudo systemctl disable ssh
```

Removes the automatic-start configuration.

It does not necessarily stop an already running service.

## 8.16 Production Scenario

Problem:

An application is not responding.

The administrator investigates:

```bash
systemctl status myapp
```

If the service has failed, inspect its logs:

```bash
sudo journalctl -u myapp -n 50 --no-pager
```

Check whether its process exists:

```bash
pgrep -a myapp
```

If appropriate, investigate the application configuration and restart the service.

---

# 9. Linux Log Management and Troubleshooting

## 9.1 What are Logs?

Logs are records of activities and events generated by an operating system or application.

Imagine a security guard maintaining a register.

Every time someone enters or exits, the guard records the event.

Similarly, applications and Linux services record important activities.

Logs can help identify:

- Application errors.
- Authentication failures.
- Service startup problems.
- Network problems.
- Configuration errors.
- System events.

## 9.2 What is /var/log?

Linux commonly stores log files under:

```text
/var/log
```

Explore the directory:

```bash
ls -lh /var/log
```

Some common log files on Ubuntu include:

| File | Purpose |
|---|---|
| `/var/log/syslog` | General system messages |
| `/var/log/auth.log` | Authentication-related messages |
| `/var/log/kern.log` | Kernel-related messages |

Availability depends on the installed logging configuration.

## 9.3 What is journald?

`journald` is the logging component of systemd.

It collects and stores system and service logs.

Administrators use `journalctl` to view these logs.

## 9.4 journalctl – View Logs

```bash
sudo journalctl
```

Displays journal entries accessible to the current user.

## 9.5 journalctl -xe

```bash
sudo journalctl -xe
```

Displays recent journal entries with additional explanatory context.

Useful when investigating system problems.

## 9.6 journalctl -u

```bash
sudo journalctl -u ssh
```

Displays logs associated with the SSH service.

For another service:

```bash
sudo journalctl -u nginx
```

## 9.7 journalctl -f

```bash
sudo journalctl -f
```

Displays new journal entries as they arrive.

Press `Ctrl+C` to exit.

## 9.8 tail -f

```bash
tail -f /var/log/syslog
```

Displays the end of the log file and continues showing new lines.

## 9.9 grep – Search Logs

```bash
grep -i "error" /var/log/syslog
```

`grep` searches for matching text.

`-i` makes the search case-insensitive.

This helps find messages containing error.

## 9.10 Real-World Troubleshooting Example

Problem:

An application is not starting.

Follow this investigation sequence.

### Step 1: Check service status

```bash
systemctl status myapp
```

Look for:

- Active state.
- Error messages.
- Exit status.

### Step 2: Check service logs

```bash
sudo journalctl -u myapp -n 50 --no-pager
```

Look for configuration errors, missing files or application exceptions.

### Step 3: Check process

```bash
pgrep -a myapp
```

Determine whether the process is running.

### Step 4: Check listening port

```bash
sudo ss -tulnp
```

Check whether the expected application port is listening.

### Step 5: Test application

```bash
curl -v http://localhost:8080
```

Check whether the application responds.

### Investigation flow

```text
Application not accessible
          |
          v
Check systemctl status
          |
          v
Check journalctl logs
          |
          v
Check process using pgrep
          |
          v
Check port using ss
          |
          v
Test using curl
          |
          v
Identify root cause
          |
          v
Apply corrective action
          |
          v
Validate application health
```

---

# 10. Combined Hands-on Linux Administration Lab

## Lab Objective

Students will create a small Linux application environment and practise:

- System information.
- Environment variables.
- User management.
- File ownership.
- File permissions.
- Network ports.
- HTTP testing.
- Process management.

## Prerequisites

- Ubuntu Server or Ubuntu Desktop.
- Bash terminal.
- sudo privileges.
- Python 3 installed.

Check Python:

```bash
python3 --version
```

## Step 1: Identify the Current User

```bash
whoami
id
```

Observe the username, UID and groups.

## Step 2: Identify the Operating System

```bash
cat /etc/os-release
hostnamectl
```

Observe the Linux distribution and version.

## Step 3: Check Memory and Disk

```bash
free -h
df -h
```

Understand the available system resources.

## Step 4: Display PATH

```bash
echo $PATH
```

Observe the directories searched by Bash.

## Step 5: Create a Custom Variable

```bash
export LAB_NAME="Linux Administration Lab"
```

Verify:

```bash
echo $LAB_NAME
printenv LAB_NAME
```

Expected output:

```text
Linux Administration Lab
```

## Step 6: Create a Working Directory

```bash
mkdir -p ~/linux-admin-lab
```

Navigate:

```bash
cd ~/linux-admin-lab
```

Verify:

```bash
pwd
```

## Step 7: Create an Application File

```bash
echo "Welcome to Linux Administration" > app.txt
```

Check:

```bash
cat app.txt
```

## Step 8: Create a Linux User

```bash
sudo adduser student1
```

Follow the prompts to create the user.

Verify:

```bash
id student1
```

## Step 9: Change Ownership

```bash
sudo chown student1:student1 app.txt
```

Verify:

```bash
ls -l app.txt
```

## Step 10: Change Permissions

```bash
sudo chmod 640 app.txt
```

Verify:

```bash
stat app.txt
```

The expected permission representation is:

```text
-rw-r-----
```

## Step 11: Start a Python Web Server

Create a separate folder for the web server:

```bash
mkdir -p ~/linux-admin-lab/web
```

Create an HTML file:

```bash
echo "<h1>Linux Admin Lab</h1>" > ~/linux-admin-lab/web/index.html
```

Navigate:

```bash
cd ~/linux-admin-lab/web
```

Start the web server:

```bash
python3 -m http.server 8080 --bind 127.0.0.1
```

Expected output:

```text
Serving HTTP on 127.0.0.1 port 8080
```

Keep this terminal open.

The server is running in the foreground.

## Step 12: Check the Listening Port

Open another terminal.

Execute:

```bash
ss -tuln | grep 8080
```

Illustrative output:

```text
tcp LISTEN 0 5 127.0.0.1:8080 0.0.0.0:*
```

Explain:

- TCP: Protocol.
- LISTEN: Waiting for connections.
- 127.0.0.1: Localhost.
- 8080: Application port.

## Step 13: Test the Application Using CURL

```bash
curl http://127.0.0.1:8080
```

Expected output:

```html
<h1>Linux Admin Lab</h1>
```

Check headers:

```bash
curl -I http://127.0.0.1:8080
```

## Step 14: Identify the Process

```bash
pgrep -a python
```

Or:

```bash
sudo ss -tulnp | grep 8080
```

Identify the Python process and PID.

## Step 15: Stop the Web Server

Go to the terminal where Python is running.

Press:

```text
Ctrl+C
```

The foreground process should terminate.

## Step 16: Verify the Port

```bash
ss -tuln | grep 8080
```

There should be no matching listening socket.

Test again:

```bash
curl http://127.0.0.1:8080
```

The connection should fail because the test web server has stopped.

## Lab Completion Checklist

- [ ] Identified Linux user.
- [ ] Checked OS information.
- [ ] Checked memory and disk.
- [ ] Displayed PATH.
- [ ] Created environment variable.
- [ ] Created a directory and file.
- [ ] Created a Linux user.
- [ ] Changed file ownership.
- [ ] Changed permissions.
- [ ] Started Python web server.
- [ ] Checked port using ss.
- [ ] Tested application using curl.
- [ ] Identified the process.
- [ ] Stopped the process.
- [ ] Verified the port was closed.

---

# 11. Real-World Production Troubleshooting Scenarios

## Scenario 1: Application Is Running but Not Accessible

**Problem:** The application team reports that the application is not accessible through the browser.

### Investigation

Check application service:

```bash
systemctl status myapp
```

Check listening ports:

```bash
sudo ss -tulnp
```

Test locally:

```bash
curl -v http://localhost:8080
```

### Possible root causes

- Application is not listening.
- Application listens only on localhost.
- Firewall blocks traffic.
- Cloud security group blocks the port.
- Application has an incorrect port configuration.

### Corrective action

Identify the actual root cause before modifying the application or firewall configuration.

## Scenario 2: Port 8080 Is Not Listening

**Problem:** Application is expected to listen on port 8080.

### Investigation

```bash
sudo ss -tulnp | grep 8080
```

Check process:

```bash
pgrep -a java
```

Check service:

```bash
systemctl status myapp
```

Check logs:

```bash
sudo journalctl -u myapp -n 50
```

### Possible root causes

- Application failed to start.
- Incorrect port configuration.
- Port binding failure.
- Application crashed.

## Scenario 3: User Cannot Access Application Directory

**Problem:** Developer receives a permission denied error.

### Investigation

```bash
whoami
id
ls -ld /opt/myapplication
ls -l /opt/myapplication/
```

Check ownership and permissions.

### Possible root causes

- Incorrect owner.
- Incorrect group.
- Missing directory execute permission.
- Missing file read permission.

### Corrective action

Apply only the minimum required ownership and permissions.

Do not blindly use:

```bash
chmod -R 777 /opt/myapplication
```

This can create serious security problems.

## Scenario 4: Application Works Manually but Fails Through Jenkins

**Problem:** Developer starts an application manually and it works.

The Jenkins pipeline fails.

### Investigation

Check environment variables:

```bash
echo $PATH
echo $JAVA_HOME
```

Check executable location:

```bash
which java
```

Check Jenkins service environment and configuration.

### Possible root causes

- JAVA_HOME is missing.
- PATH is different.
- Jenkins runs under a different Linux user.
- Environment variables are configured only in an interactive shell.

### Corrective action

Configure the required environment explicitly in the Jenkins service or pipeline.

Do not depend on a developer's interactive `.bashrc` for production automation.

## Scenario 5: Application Service Fails to Start

**Problem:** Application deployment completed, but the application service is in failed state.

### Investigation

```bash
systemctl status myapp
```

Check recent logs:

```bash
sudo journalctl -u myapp -n 100 --no-pager
```

Check process:

```bash
pgrep -a java
```

Check port:

```bash
sudo ss -tulnp
```

### Possible root causes

- Missing configuration file.
- Incorrect file permissions.
- Port already in use.
- Incorrect environment variables.
- Application startup exception.

### Corrective action

Fix the actual configuration or application problem, then restart and validate.

```bash
sudo systemctl restart myapp
```

Verify:

```bash
systemctl status myapp
```

```bash
curl -f http://localhost:8080/health
```

---

# 12. Linux System Administration Interview Questions

## Beginner-Level Questions

### 1. What is Linux System Administration?

Managing Linux servers, users, files, services, resources and network operations.

### 2. What does whoami do?

It displays the current username.

### 3. What does id do?

It displays the user ID, group ID and group membership information.

### 4. What is a Linux home directory?

It is the personal directory associated with a Linux user.

### 5. What is an environment variable?

It is a named configuration value that can be made available to processes.

### 6. What is PATH?

PATH is an environment variable containing directories searched for executable commands.

### 7. What is the purpose of .bashrc?

It contains configuration commonly loaded by interactive Bash shells.

### 8. What does chown do?

It changes file or directory ownership.

### 9. What does chmod do?

It changes file or directory permissions.

### 10. What does chmod 755 mean?

Owner gets read, write and execute. Group and others get read and execute.

### 11. What does ss do?

It displays socket and network connection information.

### 12. What does ss -tuln do?

It displays listening TCP and UDP sockets using numeric addresses.

### 13. What does curl do?

It transfers data using URLs and supports HTTP/API testing.

### 14. What does systemctl do?

It manages services through systemd.

### 15. What does journalctl do?

It displays logs collected by systemd-journald.

## Intermediate-Level Questions

### 16. What is the difference between chown and chmod?

`chown` changes ownership.

`chmod` changes permissions.

### 17. What is the difference between .profile and .bashrc?

`.profile` is commonly used for login configuration, while `.bashrc` is commonly used for interactive Bash configuration.

### 18. What is the difference between a process and a service?

A process is a running program instance. A service is functionality generally managed in the background, often through systemd.

### 19. What is the difference between TCP and UDP?

TCP provides reliable, ordered communication. UDP does not provide TCP-style delivery guarantees.

### 20. What is the difference between listening and established?

Listening means waiting for incoming connections. Established means a TCP connection has been established.

### 21. What is the difference between curl -I and curl -v?

`-I` requests headers only. `-v` displays detailed connection and protocol information.

### 22. What does kill -15 do?

It sends SIGTERM to request graceful termination.

### 23. What does kill -9 do?

It sends SIGKILL to force termination.

### 24. What does systemctl enable do?

It configures a service to start automatically during boot.

### 25. How do you check the logs of a particular service?

```bash
sudo journalctl -u service-name
```

## Production Troubleshooting Questions

### 26. Application is not responding. What commands would you use?

```bash
systemctl status myapp
sudo ss -tulnp
curl -v http://localhost:8080
sudo journalctl -u myapp -n 50
```

### 27. How would you investigate a port conflict?

```bash
sudo ss -tulnp
```

Identify the process using the required port.

### 28. How would you troubleshoot permission denied errors?

```bash
whoami
id
ls -l
stat filename
```

Check ownership and permissions.

### 29. Why might a Jenkins pipeline not find Java even when Java works manually?

The Jenkins process may have a different PATH or JAVA_HOME configuration.

### 30. What would you do if a Linux service fails after deployment?

Check:

1. Service status.
2. Service logs.
3. Application process.
4. Listening port.
5. Application health endpoint.

Then identify and fix the root cause.

---

# 13. Quick Revision Cheat Sheet

## System Information

| Command | Purpose |
|---|---|
| `whoami` | Current username |
| `id` | User and group IDs |
| `hostname` | Server hostname |
| `hostnamectl` | Host information |
| `uptime` | Server running time and load |
| `date` | Current date and time |
| `free -h` | Memory information |
| `df -h` | Disk usage |
| `lsblk` | Storage devices |
| `top` | Live process monitoring |

## User Profiles

| Command | Purpose |
|---|---|
| `echo $HOME` | Display home directory |
| `echo $USER` | Display username |
| `echo $SHELL` | Display configured login shell |
| `echo $PATH` | Display command search path |
| `env` | Display environment |
| `printenv` | Display environment variables |
| `source ~/.bashrc` | Reload Bash configuration |

## Ownership and Permissions

| Command | Purpose |
|---|---|
| `ls -l` | Display permissions and ownership |
| `id` | Display user and group IDs |
| `groups` | Display group memberships |
| `chown` | Change ownership |
| `chgrp` | Change group |
| `chmod` | Change permissions |
| `stat` | Detailed file information |

## Networking

| Command | Purpose |
|---|---|
| `ss` | Display socket information |
| `ss -t` | TCP sockets |
| `ss -u` | UDP sockets |
| `ss -l` | Listening sockets |
| `ss -n` | Numeric addresses |
| `ss -tuln` | Listening TCP/UDP ports |
| `ss -tulnp` | Listening ports with process information |
| `ss -tan` | TCP socket information |
| `ss -s` | Socket statistics |
| `ip addr` | IP addresses |
| `ip route` | Routing information |
| `hostname -I` | Host IP addresses |

## CURL

| Command | Purpose |
|---|---|
| `curl URL` | Send HTTP request |
| `curl -I URL` | Request headers only |
| `curl -v URL` | Verbose connection information |
| `curl -o file URL` | Save response to filename |
| `curl -O URL` | Save using remote filename |
| `curl -L URL` | Follow redirects |
| `curl -s URL` | Silent mode |
| `curl -f URL` | Fail for HTTP error responses |
| `curl -X POST URL` | POST request |
| `curl -H` | Add HTTP header |
| `curl -d` | Send request data |

## Process and Service Management

| Command | Purpose |
|---|---|
| `top` | Monitor processes |
| `htop` | Interactive monitoring |
| `pgrep` | Find process IDs |
| `pidof` | Find process IDs by program name |
| `kill PID` | Send termination signal |
| `kill -15 PID` | Graceful termination request |
| `kill -9 PID` | Force termination |
| `systemctl status` | Check service |
| `systemctl start` | Start service |
| `systemctl stop` | Stop service |
| `systemctl restart` | Restart service |
| `systemctl enable` | Enable at boot |
| `systemctl disable` | Disable automatic startup |

## Log Management

| Command | Purpose |
|---|---|
| `ls -lh /var/log` | List log files |
| `journalctl` | View system journal |
| `journalctl -xe` | Inspect recent journal entries |
| `journalctl -u service` | View service logs |
| `journalctl -f` | Follow journal logs |
| `tail -f file` | Follow log file |
| `grep -i error file` | Search for errors |

---

# 14. Homework – Practical Linux Administration

Students should execute these tasks in their own Ubuntu environment.

## System Information

1. Display your current username.
2. Display your UID and GID.
3. Identify your Linux hostname.
4. Display your system uptime.
5. Check available memory and disk space.

## User Profiles

6. Display HOME, USER, SHELL and PATH.
7. Create an environment variable named STUDENT_NAME.
8. Make the variable persistent using an appropriate Bash configuration file.

## File Ownership

9. Create a directory named `student-admin-lab`.
10. Create a file named `application.conf`.
11. Change its ownership to a newly created Linux user.
12. Change its permissions to 640.
13. Verify the ownership and permissions using `stat`.

## Networking

14. Start a Python HTTP server on port 8000 and verify it using `ss`.
15. Test the application using `curl`, identify its PID, stop it and verify that the port is no longer listening.

---

# 15. GitHub Submission Checklist

Students should create a repository or use their existing Linux learning repository.

Suggested repository structure:

```text
linux-system-administration/
│
├── README.md
│
├── Linux-System-Administration-Session.md
│
├── screenshots/
│   ├── system-information.png
│   ├── environment-variables.png
│   ├── chown-chmod.png
│   ├── ss-command.png
│   ├── curl-command.png
│   ├── process-management.png
│   └── hands-on-lab.png
│
└── practical-exercises/
    └── homework.md
```

## What students should demonstrate

- Commands executed successfully.
- Screenshots of actual terminal output.
- Understanding of each command.
- Ability to explain the output.
- Troubleshooting observations.
- Hands-on lab completion.

## Final Learning Outcome

By completing this session, students should be able to:

1. Collect basic Linux system information.
2. Understand Linux user profiles and environment variables.
3. Manage file ownership and permissions.
4. Identify listening ports and network connections.
5. Test websites and APIs using curl.
6. Monitor processes.
7. Manage Linux services.
8. Investigate system and service logs.
9. Follow a basic production troubleshooting workflow.

**Remember:** A good Linux administrator does not simply execute commands. A good administrator understands what the command does, interprets its output and uses the evidence to identify the root cause of a problem.

**Learn by doing. Practise every command. Document your findings. Build your Linux skills one day at a time.**
