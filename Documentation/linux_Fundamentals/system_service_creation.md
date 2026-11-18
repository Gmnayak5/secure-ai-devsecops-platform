
# Linux systemd Services – Complete Teaching Guide

A practical guide to understanding, creating, configuring, managing, monitoring, troubleshooting and securing Linux services.

This guide is designed for Linux beginners, DevOps engineers, SRE engineers and students preparing for production support roles.

It includes:
- Why Linux services are required.
- Why DevOps engineers create services.
- Production best practices.
- Important systemd directives.
- 25 practical service-file examples.
- Hands-on Linux service creation.
- Production troubleshooting scenarios.
- Student assignments and GitHub project ideas.

---

# 1. What is a Linux Service?

A Linux service is a program or process that runs in the background to provide a function to the operating system or other applications.

Examples:

- SSH service – remote server administration.
- Nginx – web server.
- Docker – container management.
- Jenkins – CI/CD automation.
- Prometheus Node Exporter – infrastructure monitoring.
- Custom Python application – backend service.

Modern Linux distributions such as Ubuntu, RHEL and Rocky Linux commonly use **systemd** to manage services.

## Process vs Daemon vs Service

| Term | Explanation | Example |
|---|---|---|
| Process | Running instance of a program | `python app.py` |
| Daemon | Background process | `sshd` |
| Service | Managed unit controlled by systemd | `ssh.service` |
| systemd | Linux system and service manager | PID 1 |

## Why do we need services?

Consider this command:

```bash
python3 app.py
```

The application runs in the current terminal session.

What happens if:
1. You close the terminal?
2. The server reboots?
3. The application crashes?
4. Nobody is logged into the server?

You need a reliable mechanism to manage the application.

This is where systemd services become useful.

A systemd service can:

- Start an application during system boot.
- Restart an application when it fails.
- Run applications under dedicated Linux users.
- Capture application logs.
- Manage application lifecycle.
- Configure resource restrictions.
- Define startup dependencies.

# 2. Why DevOps Engineers Write Linux Services

Imagine you have developed a Python REST API running on port 8000.

Initially, you start it manually:

```bash
python3 app.py
```

Now imagine the production server reboots at 2 AM.

Nobody is logged in, and the application does not start.

Customers cannot access the application.

This is an operational reliability problem.

A systemd service helps solve this problem.

## Manual Execution vs systemd Service

| Capability | Manual Execution | systemd Service |
|---|---|---|
| Start manually | Yes | Yes |
| Start during boot | Not automatically | Yes |
| Restart on failure | Manual | Configurable |
| Centralised logs | Not automatically | journald |
| Dedicated service user | Possible | Supported |
| Dependency ordering | Manual | Supported |
| Lifecycle management | Manual | Supported |

**Real-world DevOps responsibility:**

A DevOps engineer should understand how applications are deployed, started, monitored, restarted and maintained on Linux servers.

Writing systemd service files is one part of this responsibility.

---

# 3. What is a systemd Service File?

A systemd service file tells systemd how to manage an application or background process.

Typical location:

```text
/etc/systemd/system/myapp.service
```

Example:

```ini
[Unit]
Description=My Application Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 /opt/myapp/app.py
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

## Three Important Sections

### 1. [Unit]

Defines service metadata and relationships.

Examples:
- Description
- Dependencies
- Startup ordering

### 2. [Service]

Defines how the application should run.

Examples:
- User
- Group
- WorkingDirectory
- ExecStart
- Restart policy
- Environment variables

### 3. [Install]

Defines how the service should be enabled.

Example:

```ini
WantedBy=multi-user.target
```

This is commonly used for services that should start during normal multi-user system startup.

---

# 4. Production Best Practices

| Best Practice | Why It Matters |
|---|---|
| Use a dedicated service account | Reduces security risks |
| Avoid running applications as root | Limits damage if compromised |
| Use absolute executable paths | Avoids PATH-related failures |
| Configure Restart=on-failure | Improves application recovery |
| Configure WorkingDirectory | Ensures correct application context |
| Use journald or centralised logging | Simplifies troubleshooting |
| Use EnvironmentFile | Separates configuration from code |
| Set resource limits | Prevents resource exhaustion |
| Configure timeouts | Prevents indefinite shutdowns |
| Apply systemd security restrictions | Reduces application privileges |
| Define dependencies correctly | Improves startup ordering |
| Implement health monitoring | Detects failures beyond process status |
| Avoid hardcoding secrets | Reduces credential exposure |
| Test before enabling at boot | Prevents startup failures |

## Important systemd Directives

| Directive | Purpose |
|---|---|
| Description | Human-readable description |
| After | Startup ordering |
| Wants | Weak dependency |
| Requires | Strong dependency |
| Type | Defines service startup behaviour |
| User | Linux user running the service |
| Group | Linux group |
| WorkingDirectory | Application working directory |
| ExecStart | Main startup command |
| ExecStartPre | Command executed before startup |
| ExecStartPost | Command executed after startup |
| ExecReload | Reload command |
| ExecStop | Stop command |
| Restart | Restart policy |
| RestartSec | Delay before restarting |
| Environment | Environment variables |
| EnvironmentFile | External environment configuration |
| StandardOutput | Standard output destination |
| StandardError | Standard error destination |
| WantedBy | Installation target |
| MemoryMax | Memory limit |
| CPUQuota | CPU usage limit |
| TimeoutStartSec | Startup timeout |
| TimeoutStopSec | Shutdown timeout |

---

# 5. Different systemd Service Types

| Type | When to Use | Example |
|---|---|---|
| simple | Long-running foreground process | Python API |
| exec | Long-running process; startup considered complete after successful execution | Custom application |
| forking | Traditional application that forks into background | Legacy daemon |
| oneshot | Task that executes and exits | Backup script |
| notify | Application notifies systemd when ready | Notification-aware application |
| dbus | Application acquires a D-Bus name | D-Bus service |
| idle | Delays execution until other jobs are dispatched | Special startup task |

For modern applications, prefer `Type=exec` or `Type=simple`, depending on requirements and compatibility.

Use `Type=notify` when the application supports systemd readiness notifications.

---

# 6. Twenty-Five Practical Linux systemd Service Examples

These examples are templates.

Adjust executable paths, users, directories and application names to your environment before using them.

# Category A – Application Services

## Example 1: Basic Python Application

File: `/etc/systemd/system/python-app.service`

```ini
[Unit]
Description=Python Application
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 /opt/python-app/app.py
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Runs a Python application continuously in the background.

**Production scenario:**

A Python API or monitoring application must start automatically after a server reboot.

## Example 2: Python FastAPI Application

File: `/etc/systemd/system/fastapi.service`

```ini
[Unit]
Description=FastAPI Application
Wants=network-online.target
After=network-online.target

[Service]
Type=exec
User=fastapi
Group=fastapi
WorkingDirectory=/opt/fastapi
ExecStart=/opt/fastapi/venv/bin/uvicorn main:app --host 0.0.0.0 --port 8000
Restart=on-failure
RestartSec=5
Environment=PYTHONUNBUFFERED=1

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Runs a Python API using Uvicorn.

**Production scenario:**

Hosting a microservice on a Linux VM.

## Example 3: Node.js Application

File: `/etc/systemd/system/node-app.service`

```ini
[Unit]
Description=Node.js Application
After=network.target

[Service]
Type=exec
User=nodeapp
Group=nodeapp
WorkingDirectory=/opt/node-app
ExecStart=/usr/bin/node /opt/node-app/server.js
Restart=on-failure
RestartSec=5
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Runs a Node.js application without requiring someone to log into the server.

**Production scenario:**

Running a Node.js backend API.

## Example 4: Java Spring Boot Application

File: `/etc/systemd/system/student-api.service`

```ini
[Unit]
Description=Student Management API
After=network.target

[Service]
Type=exec
User=studentapp
Group=studentapp
WorkingDirectory=/opt/student-api
ExecStart=/usr/bin/java -jar /opt/student-api/student-api.jar
Restart=on-failure
RestartSec=10
Environment=SPRING_PROFILES_ACTIVE=production

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Runs a Spring Boot application as a managed Linux service.

**Production scenario:**

Java microservices deployed on Linux VMs.

## Example 5: Go Application

File: `/etc/systemd/system/go-app.service`

```ini
[Unit]
Description=Go Backend Service
After=network.target

[Service]
Type=exec
User=goapp
Group=goapp
WorkingDirectory=/opt/go-app
ExecStart=/opt/go-app/go-app
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Runs a compiled Go application as a background service.

**Production scenario:**

Lightweight infrastructure agents and backend services.

## Example 6: ASP.NET Core Application

File: `/etc/systemd/system/dotnet-api.service`

```ini
[Unit]
Description=ASP.NET Core API
After=network.target

[Service]
Type=exec
User=dotnetapp
Group=dotnetapp
WorkingDirectory=/opt/dotnet-api
ExecStart=/usr/bin/dotnet /opt/dotnet-api/MyApi.dll
Restart=on-failure
RestartSec=5
Environment=ASPNETCORE_ENVIRONMENT=Production

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Runs an ASP.NET Core application on Linux.

**Production scenario:**

Hosting .NET backend applications on Linux infrastructure.

## Example 7: Background Queue Worker

File: `/etc/systemd/system/queue-worker.service`

```ini
[Unit]
Description=Background Queue Worker
After=network.target

[Service]
Type=simple
User=worker
Group=worker
WorkingDirectory=/opt/worker
ExecStart=/usr/bin/python3 /opt/worker/worker.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Processes background jobs without requiring a user to execute the script manually.

**Production scenario:**

Email processing, asynchronous jobs and message consumers.

## Example 8: Gunicorn Application Server

File: `/etc/systemd/system/gunicorn.service`

```ini
[Unit]
Description=Gunicorn Application Server
After=network.target

[Service]
Type=exec
User=webapp
Group=webapp
WorkingDirectory=/opt/webapp
ExecStart=/opt/webapp/venv/bin/gunicorn --workers 3 --bind 127.0.0.1:8000 app:app
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Runs a production WSGI application server for Python applications.

**Production scenario:**

Gunicorn behind an Nginx reverse proxy.

## Example 9: Custom Monitoring Exporter

File: `/etc/systemd/system/custom-exporter.service`

```ini
[Unit]
Description=Custom Infrastructure Exporter
After=network.target

[Service]
Type=simple
User=monitoring
Group=monitoring
ExecStart=/opt/exporter/exporter --listen-address=:9105
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Runs a custom metrics exporter.

**Production scenario:**

Exposing application or infrastructure metrics to Prometheus.

## Example 10: Log Processing Service

File: `/etc/systemd/system/log-processor.service`

```ini
[Unit]
Description=Application Log Processor
After=network.target

[Service]
Type=simple
User=logprocessor
Group=logprocessor
WorkingDirectory=/opt/log-processor
ExecStart=/usr/bin/python3 /opt/log-processor/process_logs.py
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Continuously processes incoming application logs.

**Production scenario:**

Custom log forwarding, parsing and event processing.

---

# Category B – Special Service Types and Lifecycle Management

## Example 11: Socket-Activated Application

File: `/etc/systemd/system/socket-demo.service`

```ini
[Unit]
Description=Socket Activated Application
Requires=socket-demo.socket
After=socket-demo.socket

[Service]
Type=simple
ExecStart=/usr/local/bin/socket-server
StandardInput=socket
StandardOutput=journal
StandardError=journal
```

**Why use it?**

Allows systemd to start an application when a configured socket receives a connection.

**Production scenario:**

On-demand network services.

Note: Requires a matching `socket-demo.socket` unit. The socket unit owns the listening socket.

## Example 12: Application Using systemd Notification

File: `/etc/systemd/system/notify-app.service`

```ini
[Unit]
Description=Systemd Notification Application

[Service]
Type=notify
User=notifyapp
Group=notifyapp
ExecStart=/opt/notify-app/application
NotifyAccess=main
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**Why use it?**

The application explicitly tells systemd when it has completed startup and is ready.

**Production scenario:**

Applications that need accurate readiness reporting.

Important: The application must support the systemd notification protocol. A normal application cannot simply use `Type=notify`.

## Example 13: Legacy Forking Daemon

File: `/etc/systemd/system/legacy-daemon.service`

```ini
[Unit]
Description=Legacy Background Daemon
After=network.target

[Service]
Type=forking
User=legacy
Group=legacy
PIDFile=/run/legacy-daemon.pid
ExecStart=/opt/legacy/bin/start.sh
ExecStop=/opt/legacy/bin/stop.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Manages applications that start a parent process and fork into the background.

**Production scenario:**

Legacy enterprise applications.

Important: The application must correctly create and remove its PID file. Prefer foreground execution and `Type=simple` or `Type=exec` for new applications.

## Example 14: One-Time Initialization Service

File: `/etc/systemd/system/app-init.service`

```ini
[Unit]
Description=Application Initialization

[Service]
Type=oneshot
ExecStart=/opt/app/scripts/initialize.sh
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Executes an initialization script and exits.

**Production scenario:**

Creating application directories or preparing local configuration before an application starts.

`RemainAfterExit=yes` keeps the unit in an active state after the script successfully exits.

## Example 15: Application Startup Validation

File: `/etc/systemd/system/app-validation.service`

```ini
[Unit]
Description=Application Startup Validation

[Service]
Type=oneshot
ExecStart=/opt/app/scripts/validate.sh
TimeoutStartSec=60

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Runs a validation script during startup.

**Production scenario:**

Checking configuration files, required directories and application prerequisites.

Note: If this validation must run before another application, add the appropriate dependency and ordering directives to that application's unit.

---

# Category C – Infrastructure Automation Services

## Example 16: Backup Service

File: `/etc/systemd/system/backup.service`

```ini
[Unit]
Description=Application Backup

[Service]
Type=oneshot
User=backup
Group=backup
ExecStart=/usr/local/sbin/backup.sh
Nice=10
TimeoutStartSec=3600
```

**Why use it?**

Executes a backup script as a managed systemd job.

**Production scenario:**

Backing up application data and configuration.

For recurring backups, create a corresponding `.timer` unit. A service file alone does not schedule periodic execution.

## Example 17: Temporary File Cleanup

File: `/etc/systemd/system/temp-cleanup.service`

```ini
[Unit]
Description=Application Temporary File Cleanup

[Service]
Type=oneshot
User=appcleanup
Group=appcleanup
ExecStart=/usr/local/sbin/cleanup-temp.sh
TimeoutStartSec=300
```

**Why use it?**

Runs cleanup automation.

**Production scenario:**

Removing expired temporary files and application-generated artifacts.

Use an application-specific directory and ensure the cleanup script cannot delete unrelated data.

## Example 18: Configuration Refresh

File: `/etc/systemd/system/config-refresh.service`

```ini
[Unit]
Description=Application Configuration Refresh
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
User=configagent
Group=configagent
ExecStart=/usr/local/bin/refresh-config.sh
TimeoutStartSec=120
```

**Why use it?**

Runs a script to retrieve or regenerate configuration.

**Production scenario:**

Configuration management automation.

## Example 19: Docker Compose Application Wrapper

File: `/etc/systemd/system/compose-app.service`

```ini
[Unit]
Description=Docker Compose Application
Requires=docker.service
After=docker.service

[Service]
Type=oneshot
RemainAfterExit=yes
WorkingDirectory=/opt/compose-app
ExecStart=/usr/bin/docker compose up -d
ExecStop=/usr/bin/docker compose down
TimeoutStartSec=120
TimeoutStopSec=120

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Connects a Docker Compose application's lifecycle to systemd.

**Production scenario:**

Operating a small Compose-based application stack on a Linux VM.

Caution: This is a simple wrapper example. `docker compose down` removes containers and networks, so ensure the deployment lifecycle is appropriate. Persistent data should use properly configured volumes. For larger production container platforms, prefer an appropriate orchestrator.

## Example 20: Rsync Daemon

File: `/etc/systemd/system/rsync-custom.service`

```ini
[Unit]
Description=Custom Rsync Daemon
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/rsync --daemon --no-detach --config=/etc/rsyncd.conf
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Runs an rsync daemon in the foreground so systemd can manage it.

**Production scenario:**

File synchronization between Linux servers.

Secure the rsync configuration, authentication and network access before exposing it.

---

# Category D – Advanced Service Configurations

## Example 21: User-Level Service

File: `~/.config/systemd/user/my-user-app.service`

```ini
[Unit]
Description=My User Application

[Service]
Type=simple
ExecStart=%h/bin/my-user-app
Restart=on-failure
RestartSec=5

[Install]
WantedBy=default.target
```

**Why use it?**

Runs an application under an individual user's systemd manager rather than as a system-wide service.

**Production scenario:**

User-specific background agents and developer tools.

Commands use `systemctl --user` rather than system-wide `systemctl`.

Starting it at boot without an interactive login may require enabling lingering with:

```bash
loginctl enable-linger USER
```

Subject to system policy.

## Example 22: Template Service for Multiple Instances

File: `/etc/systemd/system/worker@.service`

```ini
[Unit]
Description=Worker Instance %i
After=network.target

[Service]
Type=simple
User=worker
Group=worker
ExecStart=/opt/workers/worker --instance %i
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Allows multiple instances of the same service to run with different instance identifiers.

**Production scenario:**

Running multiple queue workers.

Start instances:

```bash
sudo systemctl start worker@1
sudo systemctl start worker@2
sudo systemctl start worker@3
```

`%i` is replaced with the instance identifier.

## Example 23: Service Using an External Environment File

File: `/etc/systemd/system/env-app.service`

```ini
[Unit]
Description=Environment Configured Application

[Service]
Type=exec
User=envapp
Group=envapp
WorkingDirectory=/opt/env-app
EnvironmentFile=/etc/env-app/app.env
ExecStart=/opt/env-app/app
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Example environment file:

```bash
APP_ENV=production
APP_PORT=8080
LOG_LEVEL=INFO
```

**Why use it?**

Separates application configuration from the service definition.

**Production scenario:**

Managing different configurations across development, staging and production.

Do not store plaintext passwords or secrets in ordinary environment files unless the security implications are understood. Prefer proper secrets management or systemd credentials where supported.

## Example 24: Security-Hardened Application Service

File: `/etc/systemd/system/secure-app.service`

```ini
[Unit]
Description=Security Hardened Application
After=network.target

[Service]
Type=exec
User=secureapp
Group=secureapp
WorkingDirectory=/opt/secure-app
ExecStart=/opt/secure-app/app

Restart=on-failure
NoNewPrivileges=yes
PrivateTmp=yes
ProtectSystem=strict
ProtectHome=yes
ProtectKernelTunables=yes
ProtectControlGroups=yes
ReadWritePaths=/var/lib/secure-app
RestrictSUIDSGID=yes
LockPersonality=yes

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Restricts what the application can access and modify.

**Production scenario:**

Running an internet-facing service with a reduced privilege boundary.

Important: These restrictions can break applications that need access to protected paths or kernel interfaces. Test them and grant only the minimum required access.

## Example 25: Resource-Limited Application Service

File: `/etc/systemd/system/resource-app.service`

```ini
[Unit]
Description=Resource Limited Application

[Service]
Type=exec
User=resourceapp
Group=resourceapp
ExecStart=/opt/resource-app/app
Restart=on-failure
RestartSec=5

MemoryMax=512M
CPUQuota=50%
TasksMax=100
TimeoutStartSec=60
TimeoutStopSec=30

[Install]
WantedBy=multi-user.target
```

**Why use it?**

Controls the resources available to an application through systemd resource-management capabilities.

**Production scenario:**

Preventing one service from consuming excessive CPU, memory or process capacity.

Note: Resource controls depend on the systemd version and available cgroup configuration. Validate limits on the target machine.

---

# 7. Hands-On Lab: Write Your First Linux Service

Do not start with all 25 examples.

Have students write one service themselves first.

We will create a small Python application and convert it into a systemd service.

## Step 1: Verify systemd

Run:

```bash
ps -p 1 -o comm=
```

Expected on a normal systemd boot:

```text
systemd
```

Check:

```bash
systemctl is-system-running
```

WSL2 does not always use systemd by default. If systemd is not running, configure it before continuing.

## Step 2: Create a Sample Application

```bash
sudo mkdir -p /opt/linux-service-demo
```

Create the application:

```bash
sudo nano /opt/linux-service-demo/app.py
```

Add:

```python
import time
from datetime import datetime

while True:
    print(f"Linux service running: {datetime.now()}", flush=True)
    time.sleep(10)
```

This application prints a message every 10 seconds.

## Step 3: Create a Dedicated Linux User

```bash
sudo useradd \
  --system \
  --no-create-home \
  --shell /usr/sbin/nologin \
  linuxdemo
```

Give the user ownership:

```bash
sudo chown -R linuxdemo:linuxdemo /opt/linux-service-demo
```

## Step 4: Write the Service File

```bash
sudo nano /etc/systemd/system/linux-demo.service
```

Add:

```ini
[Unit]
Description=Linux Learning Demo Service
After=network.target

[Service]
Type=simple
User=linuxdemo
Group=linuxdemo
WorkingDirectory=/opt/linux-service-demo
ExecStart=/usr/bin/python3 /opt/linux-service-demo/app.py
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Explain every line before moving forward.

## Step 5: Reload systemd

```bash
sudo systemctl daemon-reload
```

When a service file is created or modified, systemd needs to reload its unit definitions.

## Step 6: Start the Service

```bash
sudo systemctl start linux-demo.service
```

## Step 7: Verify the Service

```bash
sudo systemctl status linux-demo.service
```

Expected state:

```text
Active: active (running)
```

## Step 8: Read Service Logs

```bash
sudo journalctl -u linux-demo.service
```

Follow logs continuously:

```bash
sudo journalctl -u linux-demo.service -f
```

## Step 9: Enable Automatic Startup

```bash
sudo systemctl enable linux-demo.service
```

Explain the difference:

- `start`: starts the service now.
- `enable`: configures the service to start automatically through its installation target during boot.
- `enable --now`: enables and starts it.

## Step 10: Test Restart Behaviour

```bash
sudo systemctl restart linux-demo.service
```

Check:

```bash
sudo systemctl status linux-demo.service
```

## Step 11: Stop and Disable

```bash
sudo systemctl stop linux-demo.service
sudo systemctl disable linux-demo.service
```

The application will no longer be running, and automatic startup will be removed.

## Step 12: Verify Service Configuration

```bash
sudo systemd-analyze verify /etc/systemd/system/linux-demo.service
```

This checks the unit file for configuration issues. It does not prove the application itself works correctly.

---

# 8. Important systemctl Commands

| Command | Explanation |
|---|---|
| `systemctl list-units --type=service` | Lists loaded service units |
| `systemctl list-unit-files --type=service` | Lists installed service unit files |
| `systemctl status nginx` | Checks service status |
| `systemctl start nginx` | Starts a service |
| `systemctl stop nginx` | Stops a service |
| `systemctl restart nginx` | Restarts a service |
| `systemctl reload nginx` | Requests configuration reload if supported |
| `systemctl reload-or-restart nginx` | Reloads if supported; otherwise restarts |
| `systemctl enable nginx` | Enables boot startup |
| `systemctl disable nginx` | Disables boot startup |
| `systemctl enable --now nginx` | Enables and starts |
| `systemctl is-active nginx` | Checks active state |
| `systemctl is-enabled nginx` | Checks enablement |
| `systemctl daemon-reload` | Reloads systemd unit definitions |
| `systemctl cat nginx` | Displays unit and drop-ins |
| `systemctl show nginx` | Shows service properties |
| `systemctl reset-failed nginx` | Clears failed state |
| `systemd-analyze verify file.service` | Validates unit configuration |

## Troubleshooting Commands

```bash
sudo journalctl -u linux-demo.service -n 50
```

```bash
sudo journalctl -u linux-demo.service --since "1 hour ago"
```

```bash
sudo journalctl -xeu linux-demo.service
```

```bash
systemctl list-dependencies linux-demo.service
```

```bash
systemd-analyze critical-chain
```

---

# 9. Production Troubleshooting Scenarios

| Scenario | What Students Should Investigate |
|---|---|
| Service fails after reboot | enablement, boot dependencies and journal logs |
| Service exits immediately | Application behaviour and Type |
| Permission denied | Service user, group and filesystem permissions |
| Service keeps restarting | Application crash, exit code and restart policy |
| Port already in use | Listening processes and application configuration |
| Service cannot access files | WorkingDirectory, ownership and permissions |
| Works manually but not through systemd | Environment, PATH, working directory and user |
| Starts before dependency is ready | Ordering and dependency configuration |
| Takes too long to stop | Shutdown behaviour and TimeoutStopSec |
| Consumes too much memory | Resource limits and process investigation |

---

# 10. Important Production Engineering Lessons

1. Never run a production application as root unless there is a justified requirement.
2. Use dedicated Linux service accounts.
3. Keep custom service files in `/etc/systemd/system`.
4. Do not directly modify vendor unit files under `/usr/lib/systemd/system` or `/lib/systemd/system`. Use systemd drop-ins with `systemctl edit`.
5. Do not use `Restart=always` blindly. Understand application exit behaviour.
6. Use `Type=oneshot` for finite tasks, not continuous applications.
7. Use systemd timers for recurring jobs rather than keeping an infinite loop running solely to wait for a schedule.
8. Use `journalctl` to investigate service failures.
9. Validate service definitions before production deployment.
10. Test security restrictions and resource limits before rolling them out.
11. Use proper monitoring and health checks. A running process does not necessarily mean an application is healthy.
12. Use configuration management or infrastructure as code to maintain service files consistently across servers.

---

# 11. Student Assignment – Build a Linux Service

Build a small Linux application service and demonstrate:

- [ ] Creating a Linux service user.
- [ ] Creating the application directory.
- [ ] Writing a Python application.
- [ ] Creating a systemd service file.
- [ ] Starting the service.
- [ ] Enabling automatic startup.
- [ ] Verifying service status.
- [ ] Reading application logs.
- [ ] Simulating an application failure.
- [ ] Observing restart behaviour.
- [ ] Fixing the problem.
- [ ] Capturing terminal screenshots.

## GitHub Project Deliverable

Create a repository named:

`linux-systemd-service-lab`

Recommended structure:

```text
linux-systemd-service-lab/
│
├── README.md
├── app.py
├── linux-demo.service
├── troubleshooting.md
├── LEARNINGS.md
│
└── screenshots/
    ├── service-status.png
    ├── service-logs.png
    ├── service-restart.png
    └── service-enabled.png
```

Students should be able to explain:

1. Why they created the service.
2. How systemd manages it.
3. How the service starts after reboot.
4. How to troubleshoot failures.
5. What production best practices they applied.

---

# 12. Linux systemd Services vs Kubernetes

| Linux VM | Kubernetes |
|---|---|
| systemd starts a process | Kubelet manages containers |
| `.service` file | Pod specification |
| `Restart=on-failure` | Container restart policy |
| `journalctl` | `kubectl logs` |
| `systemctl status` | `kubectl get pods` and `kubectl describe pod` |
| systemd dependencies | Kubernetes readiness and startup mechanisms |

These are conceptual comparisons, not exact equivalents.

Kubernetes has its own controllers, probes, scheduling and desired-state management.

**Key lesson:**

Before students learn Kubernetes, they should understand how Linux starts, manages, monitors and restarts processes.

This foundation makes container and platform engineering easier to understand.

---

# 13. Suggested Two-Hour Teaching Sequence

| Duration | Topic |
|---|---|
| 15 minutes | Process vs daemon vs service |
| 15 minutes | systemd architecture and service files |
| 20 minutes | Unit, Service and Install sections |
| 30 minutes | Create and run the Python service |
| 15 minutes | Enable, disable, restart and inspect logs |
| 15 minutes | Production troubleshooting |
| 10 minutes | Student assignment and interview questions |

---

# Final Teaching Recommendation

Make Example 1 and the complete Python hands-on lab mandatory for every student.

Use Examples 2–10 to explain how different application stacks run on Linux.

Use Examples 11–25 to introduce advanced system administration and production engineering practices.

The objective is not simply to memorise service-file syntax.

Students should understand:

- Why Linux services are required.
- How systemd manages processes.
- How applications are operated in production.
- How to troubleshoot service failures.
- How to apply security and reliability best practices.
- How Linux service management connects to DevOps and Kubernetes.

**Learn by doing. Test locally. Document every command. Capture the output. Share your learning publicly.**
