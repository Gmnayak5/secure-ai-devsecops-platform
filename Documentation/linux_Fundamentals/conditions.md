
# Linux Shell Scripting: Conditional Execution for DevOps Engineers

## If/Else, Loops and Case Statements — 75 Practical Examples

A practical learning guide covering Linux Bash conditional execution, automation, scripting, and real-world DevOps use cases.

---

# Table of Contents

1. Introduction to Conditional Execution
2. Bash Fundamentals
3. Part 1 — 25 If/Else Examples
4. Part 2 — 25 Loop Examples
5. Part 3 — 25 Case Statement Examples
6. Common DevOps Use Cases
7. Production Best Practices
8. Hands-on Assignments
9. Interview Questions
10. Suggested GitHub Repository Structure

---

# 1. Introduction to Conditional Execution

Conditional execution allows a script to make decisions and execute commands based on specific conditions.

For example:

- Check whether a file exists before reading it.
- Check whether a service is running before restarting it.
- Check whether a server has enough disk space.
- Check whether a Docker container is healthy.
- Check whether a Kubernetes deployment is available.
- Retry an application deployment if it fails.
- Execute different actions based on user input.

These concepts are important for:

- Linux Administration
- DevOps Engineering
- Site Reliability Engineering (SRE)
- CI/CD Automation
- Cloud Infrastructure Automation
- Kubernetes Administration
- Production Troubleshooting

## Three major conditional execution mechanisms

| Mechanism | Purpose | Typical use |
|---|---|---|
| if/else | Execute commands based on a condition | Check service status |
| Loops | Repeat commands | Monitor servers |
| case | Execute different command blocks based on a value | Deployment environment selection |

---

# 2. Bash Fundamentals

Before learning conditions, understand how Bash evaluates commands.

## 2.1 Exit status

Every Linux command returns an exit status.

- `0` = Success
- Non-zero = Failure or another status

Example:

```bash
ls /etc
echo $?
```

Possible output:

```text
0
```

Now execute:

```bash
ls /directory-that-does-not-exist
echo $?
```

Possible output:

```text
ls: cannot access '/directory-that-does-not-exist': No such file or directory
2
```

## 2.2 Important Bash operators

| Operator | Meaning |
|---|---|
| `-eq` | Equal to |
| `-ne` | Not equal to |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater than or equal |
| `-le` | Less than or equal |
| `=` | String equality |
| `!=` | String inequality |
| `-z` | String is empty |
| `-n` | String is not empty |
| `-f` | Regular file exists |
| `-d` | Directory exists |
| `-e` | Path exists |
| `-r` | File is readable |
| `-w` | File is writable |
| `-x` | File is executable |
| `&&` | Logical AND |
| `||` | Logical OR |
| `!` | Logical NOT |

## 2.3 Create a workspace

```bash
mkdir -p ~/linux-devops-scripting
cd ~/linux-devops-scripting
```

Create a script:

```bash
nano example.sh
```

Add:

```bash
#!/bin/bash

echo "Learning Linux Shell Scripting"
```

Make it executable:

```bash
chmod +x example.sh
```

Execute:

```bash
./example.sh
```

Expected output:

```text
Learning Linux Shell Scripting
```

---

# PART 1 — 25 IF/ELSE EXAMPLES

## Basic concepts

The `if` statement executes commands when a condition is true.

Syntax:

```bash
if [ condition ]; then
    commands
else
    commands
fi
```

For multiple conditions:

```bash
if [ condition1 ]; then
    commands
elif [ condition2 ]; then
    commands
else
    commands
fi
```

---

## Example 1 — Check whether a number is positive or negative

Level: Beginner

```bash
#!/bin/bash

number=10

if [ "$number" -gt 0 ]; then
    echo "Positive number"
else
    echo "Zero or negative number"
fi
```

Output:

```text
Positive number
```

DevOps application:

- Validate numeric configuration values.
- Check retry counts.
- Validate deployment parameters.

## Example 2 — Check whether a number is even or odd

Level: Beginner

```bash
#!/bin/bash

number=7

if [ $((number % 2)) -eq 0 ]; then
    echo "Even number"
else
    echo "Odd number"
fi
```

Output:

```text
Odd number
```

DevOps application:

- Basic Bash arithmetic.
- Selecting alternate execution paths.
- Testing automation logic.

## Example 3 — Check whether a file exists

Level: Beginner

```bash
#!/bin/bash

file="/etc/passwd"

if [ -f "$file" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

Output:

```text
File exists
```

DevOps application:

- Check application configuration.
- Validate deployment files.
- Check whether certificates or secrets files exist.

## Example 4 — Check whether a directory exists

Level: Beginner

```bash
#!/bin/bash

directory="/var/log"

if [ -d "$directory" ]; then
    echo "Directory exists"
else
    echo "Directory missing"
fi
```

DevOps application:

- Validate log directories.
- Check backup locations.
- Verify application deployment directories.

## Example 5 — Check whether a file is empty

Level: Beginner

```bash
#!/bin/bash

file="/tmp/application.log"

if [ -s "$file" ]; then
    echo "File contains data"
else
    echo "File is empty or missing"
fi
```

DevOps application:

- Validate application logs.
- Check generated reports.
- Verify backup output.

Note: `-s` returns true when a file exists and has a size greater than zero.

## Example 6 — Check whether a user exists

Level: Beginner

```bash
#!/bin/bash

username="ubuntu"

if id "$username" >/dev/null 2>&1; then
    echo "User exists"
else
    echo "User does not exist"
fi
```

DevOps application:

- Linux user provisioning.
- Ansible preparation scripts.
- CI/CD deployment account validation.

## Example 7 — Check whether a service is running

Level: Beginner

```bash
#!/bin/bash

service="ssh"

if systemctl is-active --quiet "$service"; then
    echo "$service is running"
else
    echo "$service is not running"
fi
```

DevOps application:

- Application service monitoring.
- Production health checks.
- Automated service recovery.

Note: The service name may differ between Linux distributions.

## Example 8 — Check whether a command is installed

Level: Beginner

```bash
#!/bin/bash

command_name="docker"

if command -v "$command_name" >/dev/null 2>&1; then
    echo "Docker is installed"
else
    echo "Docker is not installed"
fi
```

DevOps application:

- Validate prerequisites before deployments.
- Check whether kubectl, Helm, Terraform, or Docker is installed.
- Prepare build agents.

## Example 9 — Check whether a user has root privileges

Level: Beginner

```bash
#!/bin/bash

if [ "$EUID" -eq 0 ]; then
    echo "Running as root"
else
    echo "Not running as root"
fi
```

DevOps application:

- Validate administrative permissions.
- Prevent scripts from running with incorrect privileges.
- Automate system configuration.

## Example 10 — Compare two strings

Level: Beginner

```bash
#!/bin/bash

environment="production"

if [ "$environment" = "production" ]; then
    echo "Production environment selected"
else
    echo "Non-production environment selected"
fi
```

DevOps application:

- Environment selection.
- CI/CD pipeline configuration.
- Deployment decisions.

## Example 11 — Check whether a variable is empty

Level: Beginner

```bash
#!/bin/bash

username=""

if [ -z "$username" ]; then
    echo "Username is missing"
else
    echo "Username: $username"
fi
```

DevOps application:

- Validate environment variables.
- Prevent deployments with missing configuration.
- Validate pipeline parameters.

## Example 12 — Validate multiple required variables

Level: Intermediate

```bash
#!/bin/bash

APP_NAME="student-service"
ENVIRONMENT="production"

if [ -n "$APP_NAME" ] && [ -n "$ENVIRONMENT" ]; then
    echo "Configuration is valid"
else
    echo "Missing required configuration"
    exit 1
fi
```

DevOps application:

- Validate CI/CD environment variables.
- Validate application configuration before deployment.

## Example 13 — Check disk usage

Level: Intermediate

```bash
#!/bin/bash

usage=$(df -P / | awk 'NR==2 {gsub("%","",$5); print $5}')

if [ "$usage" -gt 80 ]; then
    echo "WARNING: Disk usage is above 80%"
else
    echo "Disk usage is normal: ${usage}%"
fi
```

DevOps application:

- Production disk monitoring.
- Server capacity checks.
- Alerting automation.

## Example 14 — Check available memory

Level: Intermediate

```bash
#!/bin/bash

available_memory=$(awk '/MemAvailable/ {print int($2/1024)}' /proc/meminfo)

if [ "$available_memory" -lt 500 ]; then
    echo "WARNING: Available memory is below 500 MB"
else
    echo "Available memory: ${available_memory} MB"
fi
```

DevOps application:

- Server health checks.
- Memory pressure detection.
- Infrastructure monitoring.

## Example 15 — Check whether a port is listening

Level: Intermediate

```bash
#!/bin/bash

port=8080

if ss -lnt | awk '{print $4}' | grep -Eq "[:.]${port}$"; then
    echo "Port $port is listening"
else
    echo "Port $port is not listening"
fi
```

DevOps application:

- Verify application startup.
- Check web server availability.
- Validate application deployment.

Note: This checks listening TCP sockets. It does not prove that the application responds correctly.

## Example 16 — Check HTTP response

Level: Intermediate

```bash
#!/bin/bash

url="http://localhost:8080/health"

if curl -fsS --max-time 5 "$url" >/dev/null; then
    echo "Application is healthy"
else
    echo "Application health check failed"
fi
```

DevOps application:

- Application health checks.
- Post-deployment validation.
- CI/CD smoke testing.

## Example 17 — Check Git working directory

Level: Intermediate

```bash
#!/bin/bash

if git rev-parse --is-inside-work-tree >/dev/null 2>&1; then
    echo "Inside a Git repository"
else
    echo "Not inside a Git repository"
fi
```

DevOps application:

- Git automation.
- Build scripts.
- Repository validation.

## Example 18 — Compare deployment environments using elif

Level: Intermediate

```bash
#!/bin/bash

environment="staging"

if [ "$environment" = "development" ]; then
    echo "Deploying to development"
elif [ "$environment" = "staging" ]; then
    echo "Deploying to staging"
elif [ "$environment" = "production" ]; then
    echo "Deploying to production"
else
    echo "Invalid environment"
    exit 1
fi
```

DevOps application:

- Multi-environment deployments.
- Jenkins pipeline scripts.
- Release automation.

## Example 19 — Check whether a backup is recent

Level: Intermediate

```bash
#!/bin/bash

backup="/backup/database.sql"

if [ -f "$backup" ] && find "$backup" -mmin -1440 -print -quit | grep -q .; then
    echo "Recent backup exists"
else
    echo "Backup missing or older than 24 hours"
fi
```

DevOps application:

- Backup verification.
- Disaster recovery readiness.
- Database operations.

## Example 20 — Validate a software version

Level: Intermediate

```bash
#!/bin/bash

required_version="1.0.0"
installed_version="1.0.0"

if [ "$installed_version" = "$required_version" ]; then
    echo "Version matches"
else
    echo "Version mismatch"
fi
```

DevOps application:

- Application version validation.
- Release verification.
- Deployment compatibility checks.

For semantic version ordering, use a version-aware comparison tool rather than ordinary string comparison.

## Example 21 — Check whether a Kubernetes deployment is available

Level: Advanced

```bash
#!/bin/bash

deployment="student-service"
namespace="default"

if kubectl rollout status deployment/"$deployment" \
    -n "$namespace" --timeout=30s; then
    echo "Deployment successful"
else
    echo "Deployment failed or timed out"
    exit 1
fi
```

DevOps application:

- Kubernetes deployment automation.
- CI/CD rollout verification.
- Release gates.

## Example 22 — Check whether a Docker container is running

Level: Advanced

```bash
#!/bin/bash

container="student-service"

if [ "$(docker inspect -f '{{.State.Running}}' "$container" 2>/dev/null)" = "true" ]; then
    echo "Container is running"
else
    echo "Container is not running"
fi
```

DevOps application:

- Container health monitoring.
- Deployment validation.
- Docker automation.

Note: Running state does not necessarily mean the container's application is healthy.

## Example 23 — Retry an application health check

Level: Advanced

```bash
#!/bin/bash

url="http://localhost:8080/health"
max_attempts=5

for attempt in $(seq 1 "$max_attempts"); do
    if curl -fsS --max-time 5 "$url" >/dev/null; then
        echo "Application is healthy"
        exit 0
    fi

    echo "Attempt $attempt failed"
    sleep 3
done

echo "Application did not become healthy"
exit 1
```

DevOps application:

- Post-deployment health checks.
- Startup validation.
- CI/CD retry logic.

## Example 24 — Check whether a package is installed before installing it

Level: Advanced

```bash
#!/bin/bash

package="nginx"

if dpkg-query -W -f='${Status}' "$package" 2>/dev/null | grep -q "install ok installed"; then
    echo "$package is already installed"
else
    echo "Installing $package"
    sudo apt-get update &&
    sudo apt-get install -y "$package"
fi
```

DevOps application:

- Server bootstrap automation.
- Configuration management.
- Repeatable infrastructure setup.

This example is for Debian/Ubuntu systems.

## Example 25 — Production deployment gate

Level: Advanced

```bash
#!/bin/bash

set -Eeuo pipefail

ENVIRONMENT="${ENVIRONMENT:-staging}"
IMAGE="${IMAGE:-}"

if [ -z "$IMAGE" ]; then
    echo "ERROR: IMAGE variable is required"
    exit 1
fi

if [ "$ENVIRONMENT" = "production" ]; then
    if [ "${APPROVED:-false}" != "true" ]; then
        echo "ERROR: Production deployment requires approval"
        exit 1
    fi
fi

echo "Deployment validation passed"
echo "Environment: $ENVIRONMENT"
echo "Image: $IMAGE"
```

DevOps application:

- Production deployment approvals.
- CI/CD deployment gates.
- Release governance.

This script validates deployment inputs; it does not itself deploy the application.

---

# PART 2 — 25 LOOP EXAMPLES

## What is a loop?

A loop executes a group of commands repeatedly.

Common Bash loops:

| Loop | Purpose |
|---|---|
| for | Iterate over a list or range |
| while | Repeat while a condition is true |
| until | Repeat until a condition becomes true |
| select | Build a simple interactive menu |

---

## Example 1 — Basic for loop

Level: Beginner

```bash
#!/bin/bash

for name in Linux Docker Kubernetes
do
    echo "Learning $name"
done
```

Output:

```text
Learning Linux
Learning Docker
Learning Kubernetes
```

DevOps application:

- Process multiple tools.
- Execute repetitive administrative commands.
- Run multiple checks.

## Example 2 — Loop through numbers

Level: Beginner

```bash
#!/bin/bash

for number in {1..5}
do
    echo "Number: $number"
done
```

DevOps application:

- Retry counters.
- Numbered operations.
- Test execution.

## Example 3 — Loop through server names

Level: Beginner

```bash
#!/bin/bash

for server in web01 web02 db01
do
    echo "Checking server: $server"
done
```

DevOps application:

- Server inventory processing.
- Infrastructure automation.
- Fleet health checks.

## Example 4 — Loop through files

Level: Beginner

```bash
#!/bin/bash

for file in /var/log/*.log
do
    [ -f "$file" ] || continue
    echo "Processing: $file"
done
```

DevOps application:

- Log processing.
- Log archival.
- Application troubleshooting.

## Example 5 — Loop through directories

Level: Beginner

```bash
#!/bin/bash

for directory in /etc /var/log /home
do
    if [ -d "$directory" ]; then
        echo "$directory exists"
    fi
done
```

DevOps application:

- Filesystem validation.
- Server configuration checks.

## Example 6 — Loop through installed packages

Level: Beginner

```bash
#!/bin/bash

for package in nginx docker curl git
do
    if command -v "$package" >/dev/null 2>&1; then
        echo "$package is available"
    else
        echo "$package is missing"
    fi
done
```

DevOps application:

- Build agent validation.
- Dependency verification.
- Server readiness checks.

## Example 7 — C-style for loop

Level: Beginner

```bash
#!/bin/bash

for ((i=1; i<=5; i++))
do
    echo "Iteration: $i"
done
```

DevOps application:

- Controlled retries.
- Repeated test execution.
- Numeric iteration.

## Example 8 — Loop through command-line arguments

Level: Beginner

```bash
#!/bin/bash

for argument in "$@"
do
    echo "Argument: $argument"
done
```

Execute:

```bash
./script.sh deploy staging student-service
```

DevOps application:

- Reusable deployment scripts.
- Command-line automation.
- Parameter-driven scripts.

## Example 9 — Read a file line by line

Level: Intermediate

```bash
#!/bin/bash

while IFS= read -r line
do
    echo "Server: $line"
done < servers.txt
```

Example `servers.txt`:

```text
web01
web02
db01
```

DevOps application:

- Read server inventories.
- Process configuration files.
- Automate bulk operations.

## Example 10 — While loop with a counter

Level: Beginner

```bash
#!/bin/bash

count=1

while [ "$count" -le 5 ]
do
    echo "Count: $count"
    count=$((count + 1))
done
```

DevOps application:

- Repeated checks.
- Polling.
- Retry logic.

## Example 11 — Monitor a service continuously

Level: Intermediate

```bash
#!/bin/bash

service="ssh"

while true
do
    if systemctl is-active --quiet "$service"; then
        echo "$service is running"
    else
        echo "$service is down"
    fi

    sleep 10
done
```

DevOps application:

- Service monitoring.
- Basic operational monitoring.
- Troubleshooting exercises.

Stop using `Ctrl+C`.

Production note: For persistent monitoring, use Prometheus, an observability platform, or a systemd-managed monitoring process rather than relying on an unmanaged infinite loop.

## Example 12 — Retry an HTTP health check

Level: Intermediate

```bash
#!/bin/bash

url="http://localhost:8080/health"
attempt=1
max_attempts=5

while [ "$attempt" -le "$max_attempts" ]
do
    if curl -fsS --max-time 5 "$url" >/dev/null; then
        echo "Application is healthy"
        exit 0
    fi

    echo "Retry $attempt"
    attempt=$((attempt + 1))
    sleep 3
done

echo "Application health check failed"
exit 1
```

DevOps application:

- Deployment verification.
- Application startup checks.
- CI/CD automation.

## Example 13 — Wait until a service becomes active

Level: Intermediate

```bash
#!/bin/bash

service="nginx"
max_attempts=10
attempt=1

while [ "$attempt" -le "$max_attempts" ]; do
    if systemctl is-active --quiet "$service"; then
        echo "$service is active"
        exit 0
    fi

    echo "Waiting for $service..."
    sleep 2
    attempt=$((attempt + 1))
done

echo "Service failed to become active"
exit 1
```

DevOps application:

- Service startup orchestration.
- Deployment validation.
- Dependency checks.

## Example 14 — Use until to wait for a condition

Level: Intermediate

```bash
#!/bin/bash

count=1

until [ "$count" -gt 5 ]
do
    echo "Count: $count"
    count=$((count + 1))
done
```

Output:

```text
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
```

DevOps application:

- Polling for readiness.
- Waiting for infrastructure.
- Retry automation.

## Example 15 — Loop with break

Level: Intermediate

```bash
#!/bin/bash

for number in {1..10}
do
    if [ "$number" -eq 5 ]; then
        echo "Found 5. Stopping."
        break
    fi

    echo "$number"
done
```

DevOps application:

- Stop processing after a critical condition.
- Terminate a search when a result is found.
- Control retry operations.

## Example 16 — Loop with continue

Level: Intermediate

```bash
#!/bin/bash

for number in {1..5}
do
    if [ "$number" -eq 3 ]; then
        echo "Skipping 3"
        continue
    fi

    echo "Number: $number"
done
```

DevOps application:

- Skip unavailable servers.
- Ignore irrelevant log records.
- Skip optional deployment components.

## Example 17 — Loop through Docker containers

Level: Intermediate

```bash
#!/bin/bash

for container in $(docker ps --format '{{.Names}}')
do
    echo "Running container: $container"
done
```

DevOps application:

- Container inventory.
- Container inspection.
- Docker troubleshooting.

For scripts that must handle arbitrary container names safely, prefer reading the command output line by line rather than using command substitution and word splitting.

## Example 18 — Check multiple Kubernetes deployments

Level: Intermediate

```bash
#!/bin/bash

namespace="default"

for deployment in student-service course-service
do
    echo "Checking $deployment"

    if kubectl rollout status deployment/"$deployment" \
        -n "$namespace" --timeout=30s; then
        echo "$deployment is available"
    else
        echo "$deployment rollout failed"
    fi
done
```

DevOps application:

- Kubernetes release validation.
- Multi-application deployment checks.
- GitOps troubleshooting.

## Example 19 — Loop through Kubernetes namespaces

Level: Intermediate

```bash
#!/bin/bash

while IFS= read -r namespace
do
    [ -n "$namespace" ] || continue

    echo "Namespace: $namespace"
    kubectl get pods -n "$namespace"
done < <(kubectl get namespaces -o name | sed 's#namespace/##')
```

DevOps application:

- Cluster inventory.
- Namespace-level troubleshooting.
- Resource auditing.

## Example 20 — Process log files

Level: Advanced

```bash
#!/bin/bash

log_directory="/var/log"

while IFS= read -r -d '' logfile
do
    echo "Checking $logfile"

    if grep -qi "error" "$logfile"; then
        echo "ERROR found in $logfile"
    fi
done < <(find "$log_directory" -type f -name "*.log" -print0)
```

DevOps application:

- Log analysis.
- Error detection.
- Production troubleshooting.

## Example 21 — Backup multiple directories

Level: Advanced

```bash
#!/bin/bash

backup_root="/backup"
timestamp=$(date +%Y%m%d_%H%M%S)

mkdir -p "$backup_root"

for directory in /etc /home
do
    if [ -d "$directory" ]; then
        name=$(basename "$directory")

        tar -czf "$backup_root/${name}_${timestamp}.tar.gz" \
            -C "$(dirname "$directory")" "$name"

        echo "Backup completed: $directory"
    fi
done
```

DevOps application:

- Linux configuration backups.
- Operational recovery.
- Scheduled maintenance.

Production note: This example does not guarantee application-consistent backups. Databases and stateful applications need appropriate backup procedures.

## Example 22 — Rolling deployment to multiple hosts

Level: Advanced

```bash
#!/bin/bash

set -Eeuo pipefail

servers=("web01" "web02" "web03")

for server in "${servers[@]}"
do
    echo "Deploying to $server"

    if ssh "$server" "sudo systemctl restart nginx"; then
        echo "Restart succeeded on $server"
    else
        echo "Deployment failed on $server"
        exit 1
    fi
done
```

DevOps application:

- Multi-server deployment.
- Remote administration.
- Controlled service restarts.

Production note: This is a simplified sequential example. Real rolling deployments need health checks, drain operations, rollback, and concurrency controls.

## Example 23 — Retry a failed command with exponential backoff

Level: Advanced

```bash
#!/bin/bash

max_attempts=5
attempt=1
delay=2

until some_deployment_command; do
    if [ "$attempt" -ge "$max_attempts" ]; then
        echo "Command failed after $attempt attempts"
        exit 1
    fi

    echo "Attempt $attempt failed. Retrying in $delay seconds"

    sleep "$delay"
    attempt=$((attempt + 1))
    delay=$((delay * 2))
done

echo "Command succeeded"
```

DevOps application:

- Transient network failures.
- Deployment retries.
- API availability checks.

Production note: Replace `some_deployment_command` with a real command. Consider maximum delays, jitter, and whether retrying the operation is safe.

## Example 24 — Process a CSV file

Level: Advanced

```bash
#!/bin/bash

while IFS=, read -r server environment owner
do
    [ "$server" = "server" ] && continue

    echo "Server: $server"
    echo "Environment: $environment"
    echo "Owner: $owner"
done < servers.csv
```

Example CSV:

```text
server,environment,owner
web01,production,team-a
web02,staging,team-b
db01,production,team-c
```

DevOps application:

- Infrastructure inventory.
- Server management.
- Deployment target selection.

Note: This simple approach assumes CSV fields do not contain quoted commas or embedded newlines. Use a CSV-aware parser for general CSV files.

## Example 25 — Production-style deployment polling

Level: Advanced

```bash
#!/bin/bash

set -Eeuo pipefail

deployment="student-service"
namespace="default"
max_attempts=12
attempt=1

while [ "$attempt" -le "$max_attempts" ]; do

    if kubectl get deployment "$deployment" \
        -n "$namespace" >/dev/null 2>&1; then

        if kubectl rollout status deployment/"$deployment" \
            -n "$namespace" --timeout=5s; then

            echo "Deployment is ready"
            exit 0
        fi
    fi

    echo "Waiting for deployment readiness: attempt $attempt"
    attempt=$((attempt + 1))
    sleep 5
done

echo "Deployment did not become ready"
kubectl describe deployment "$deployment" -n "$namespace" || true
exit 1
```

DevOps application:

- Kubernetes release automation.
- Deployment readiness validation.
- CI/CD release gates.

---

# PART 3 — 25 CASE STATEMENT EXAMPLES

## What is a case statement?

The `case` statement executes different blocks depending on the value of a variable or expression.

Syntax:

```bash
case "$variable" in
    value1)
        commands
        ;;
    value2)
        commands
        ;;
    *)
        default_commands
        ;;
esac
```

Important:

- `case` compares a value against patterns.
- `)` begins a matching block.
- `;;` ends a matching block.
- `*` matches any value not matched earlier.
- `esac` ends the case statement.

---

## Example 1 — Basic case statement

Level: Beginner

```bash
#!/bin/bash

day="Monday"

case "$day" in
    Monday)
        echo "Start of work week"
        ;;
    Friday)
        echo "End of work week"
        ;;
    *)
        echo "Regular working day"
        ;;
esac
```

DevOps application:

- Execute different tasks based on a selected value.
- Build simple command-line automation.

## Example 2 — Select an environment

Level: Beginner

```bash
#!/bin/bash

environment="production"

case "$environment" in
    development)
        echo "Development environment"
        ;;
    staging)
        echo "Staging environment"
        ;;
    production)
        echo "Production environment"
        ;;
    *)
        echo "Invalid environment"
        exit 1
        ;;
esac
```

DevOps application:

- Environment-specific deployment.
- CI/CD configuration.
- Release management.

## Example 3 — Handle user input

Level: Beginner

```bash
#!/bin/bash

read -r -p "Continue? (yes/no): " answer

case "$answer" in
    yes|YES|y|Y)
        echo "Continuing"
        ;;
    no|NO|n|N)
        echo "Stopping"
        ;;
    *)
        echo "Invalid input"
        ;;
esac
```

DevOps application:

- Interactive maintenance scripts.
- Deployment confirmations.
- Administrative operations.

## Example 4 — Start, stop or restart a service

Level: Beginner

```bash
#!/bin/bash

action="$1"
service="nginx"

case "$action" in
    start)
        sudo systemctl start "$service"
        ;;
    stop)
        sudo systemctl stop "$service"
        ;;
    restart)
        sudo systemctl restart "$service"
        ;;
    status)
        systemctl status "$service"
        ;;
    *)
        echo "Usage: $0 {start|stop|restart|status}"
        exit 1
        ;;
esac
```

DevOps application:

- Linux service administration.
- Operational automation.
- Standardized maintenance scripts.

## Example 5 — Select a Linux package manager

Level: Beginner

```bash
#!/bin/bash

if command -v apt-get >/dev/null 2>&1; then
    manager="apt"
elif command -v dnf >/dev/null 2>&1; then
    manager="dnf"
elif command -v yum >/dev/null 2>&1; then
    manager="yum"
else
    manager="unknown"
fi

case "$manager" in
    apt)
        echo "Debian or Ubuntu package manager"
        ;;
    dnf)
        echo "Fedora or modern RHEL package manager"
        ;;
    yum)
        echo "YUM package manager"
        ;;
    *)
        echo "Unsupported package manager"
        exit 1
        ;;
esac
```

DevOps application:

- Cross-distribution automation.
- Linux provisioning.
- Server bootstrap scripts.

## Example 6 — File extension handling

Level: Beginner

```bash
#!/bin/bash

filename="application.log"

case "$filename" in
    *.log)
        echo "Log file"
        ;;
    *.sh)
        echo "Shell script"
        ;;
    *.conf)
        echo "Configuration file"
        ;;
    *)
        echo "Unknown file type"
        ;;
esac
```

DevOps application:

- Log processing.
- File classification.
- Deployment artifact handling.

## Example 7 — Identify HTTP status codes

Level: Beginner

```bash
#!/bin/bash

status_code="404"

case "$status_code" in
    200)
        echo "Success"
        ;;
    301|302)
        echo "Redirect"
        ;;
    400|401|403|404)
        echo "Client error"
        ;;
    500|502|503|504)
        echo "Server error"
        ;;
    *)
        echo "Other HTTP status"
        ;;
esac
```

DevOps application:

- API troubleshooting.
- Web application monitoring.
- HTTP response analysis.

## Example 8 — Choose a Docker operation

Level: Intermediate

```bash
#!/bin/bash

action="$1"
container="student-service"

case "$action" in
    start)
        docker start "$container"
        ;;
    stop)
        docker stop "$container"
        ;;
    restart)
        docker restart "$container"
        ;;
    logs)
        docker logs "$container"
        ;;
    inspect)
        docker inspect "$container"
        ;;
    *)
        echo "Usage: $0 {start|stop|restart|logs|inspect}"
        exit 1
        ;;
esac
```

DevOps application:

- Docker administration.
- Container troubleshooting.
- Operational scripts.

## Example 9 — Kubernetes operations

Level: Intermediate

```bash
#!/bin/bash

action="$1"
deployment="student-service"
namespace="default"

case "$action" in
    get)
        kubectl get deployment "$deployment" -n "$namespace"
        ;;
    describe)
        kubectl describe deployment "$deployment" -n "$namespace"
        ;;
    rollout)
        kubectl rollout status deployment/"$deployment" -n "$namespace"
        ;;
    restart)
        kubectl rollout restart deployment/"$deployment" -n "$namespace"
        ;;
    *)
        echo "Usage: $0 {get|describe|rollout|restart}"
        exit 1
        ;;
esac
```

DevOps application:

- Kubernetes administration.
- Deployment troubleshooting.
- Release automation.

## Example 10 — Select a cloud provider

Level: Intermediate

```bash
#!/bin/bash

provider="aws"

case "$provider" in
    aws)
        echo "AWS infrastructure workflow"
        ;;
    azure)
        echo "Azure infrastructure workflow"
        ;;
    gcp)
        echo "Google Cloud infrastructure workflow"
        ;;
    *)
        echo "Unsupported cloud provider"
        exit 1
        ;;
esac
```

DevOps application:

- Multi-cloud automation.
- Terraform workflows.
- Cloud deployment selection.

## Example 11 — Identify a Git operation

Level: Intermediate

```bash
#!/bin/bash

action="$1"

case "$action" in
    clone)
        git clone https://github.com/example/project.git
        ;;
    pull)
        git pull --ff-only
        ;;
    status)
        git status
        ;;
    log)
        git log --oneline -5
        ;;
    *)
        echo "Usage: $0 {clone|pull|status|log}"
        exit 1
        ;;
esac
```

DevOps application:

- Git automation.
- Repository management.
- Developer productivity scripts.

## Example 12 — Handle deployment actions

Level: Intermediate

```bash
#!/bin/bash

action="$1"

case "$action" in
    build)
        echo "Building application"
        ;;
    test)
        echo "Running tests"
        ;;
    deploy)
        echo "Deploying application"
        ;;
    rollback)
        echo "Starting rollback"
        ;;
    *)
        echo "Unknown deployment action"
        exit 1
        ;;
esac
```

DevOps application:

- CI/CD workflow scripts.
- Release orchestration.
- Deployment lifecycle automation.

## Example 13 — Match multiple file patterns

Level: Intermediate

```bash
#!/bin/bash

filename="deployment.yaml"

case "$filename" in
    *.yaml|*.yml)
        echo "YAML configuration"
        ;;
    *.json)
        echo "JSON configuration"
        ;;
    *.xml)
        echo "XML configuration"
        ;;
    *)
        echo "Unsupported format"
        ;;
esac
```

DevOps application:

- Configuration file validation.
- Kubernetes manifest handling.
- Automation pipelines.

## Example 14 — Match patterns with wildcards

Level: Intermediate

```bash
#!/bin/bash

branch="feature/login"

case "$branch" in
    main)
        echo "Main branch"
        ;;
    release/*)
        echo "Release branch"
        ;;
    feature/*)
        echo "Feature branch"
        ;;
    hotfix/*)
        echo "Hotfix branch"
        ;;
    *)
        echo "Other branch"
        ;;
esac
```

DevOps application:

- Branch-based CI/CD execution.
- Release pipelines.
- GitOps workflows.

## Example 15 — Check application environments

Level: Intermediate

```bash
#!/bin/bash

environment="${1:-development}"

case "$environment" in
    development)
        CONFIG_FILE="dev.conf"
        ;;
    staging)
        CONFIG_FILE="staging.conf"
        ;;
    production)
        CONFIG_FILE="prod.conf"
        ;;
    *)
        echo "Invalid environment"
        exit 1
        ;;
esac

echo "Using configuration: $CONFIG_FILE"
```

DevOps application:

- Environment configuration.
- Application deployments.
- Configuration management.

## Example 16 — Process a backup operation

Level: Intermediate

```bash
#!/bin/bash

action="$1"

case "$action" in
    create)
        echo "Creating backup"
        ;;
    verify)
        echo "Verifying backup"
        ;;
    restore)
        echo "Restoring backup"
        ;;
    list)
        echo "Listing backups"
        ;;
    *)
        echo "Usage: $0 {create|verify|restore|list}"
        exit 1
        ;;
esac
```

DevOps application:

- Backup automation.
- Disaster recovery.
- Database administration.

## Example 17 — Handle monitoring alerts

Level: Intermediate

```bash
#!/bin/bash

severity="$1"

case "$severity" in
    critical)
        echo "Trigger critical alert"
        ;;
    warning)
        echo "Send warning notification"
        ;;
    info)
        echo "Record informational event"
        ;;
    *)
        echo "Unknown alert severity"
        ;;
esac
```

DevOps application:

- Alert routing.
- Monitoring automation.
- Incident response workflows.

## Example 18 — Handle different application languages

Level: Intermediate

```bash
#!/bin/bash

language="$1"

case "$language" in
    java)
        echo "Using Gradle or Maven build"
        ;;
    python)
        echo "Using Python build and test tools"
        ;;
    dotnet)
        echo "Using dotnet build and test"
        ;;
    node)
        echo "Using npm build and test"
        ;;
    *)
        echo "Unsupported application language"
        exit 1
        ;;
esac
```

DevOps application:

- Polyglot CI/CD pipelines.
- Build automation.
- Language-specific deployment workflows.

## Example 19 — Select a maintenance task

Level: Intermediate

```bash
#!/bin/bash

read -r -p "Choose maintenance task: " task

case "$task" in
    disk)
        df -h
        ;;
    memory)
        free -h
        ;;
    processes)
        ps aux --sort=-%cpu | head
        ;;
    uptime)
        uptime
        ;;
    *)
        echo "Unknown maintenance task"
        ;;
esac
```

DevOps application:

- Linux maintenance tools.
- Operations support.
- Production diagnostics.

## Example 20 — Interactive operational menu

Level: Advanced

```bash
#!/bin/bash

echo "1. Check disk"
echo "2. Check memory"
echo "3. Check services"
echo "4. Check Kubernetes pods"
echo "5. Exit"

read -r -p "Select option: " choice

case "$choice" in
    1)
        df -h
        ;;
    2)
        free -h
        ;;
    3)
        systemctl --failed
        ;;
    4)
        kubectl get pods -A
        ;;
    5)
        echo "Exiting"
        exit 0
        ;;
    *)
        echo "Invalid selection"
        exit 1
        ;;
esac
```

DevOps application:

- Operations support menu.
- Troubleshooting utilities.
- Junior DevOps engineer training.

## Example 21 — Validate command-line options

Level: Advanced

```bash
#!/bin/bash

while [ "$#" -gt 0 ]; do
    case "$1" in
        --environment)
            [ "$#" -ge 2 ] || {
                echo "Missing environment value"
                exit 1
            }
            environment="$2"
            shift 2
            ;;
        --version)
            [ "$#" -ge 2 ] || {
                echo "Missing version value"
                exit 1
            }
            version="$2"
            shift 2
            ;;
        --help)
            echo "Usage: $0 --environment staging --version 1.0.0"
            exit 0
            ;;
        *)
            echo "Unknown argument: $1"
            exit 1
            ;;
    esac
done

echo "Environment: ${environment:-not-set}"
echo "Version: ${version:-not-set}"
```

DevOps application:

- Reusable deployment CLI tools.
- Build automation.
- Standardized script interfaces.

## Example 22 — Handle operating system types

Level: Advanced

```bash
#!/bin/bash

if [ -r /etc/os-release ]; then
    . /etc/os-release
else
    echo "Cannot identify operating system"
    exit 1
fi

case "$ID" in
    ubuntu|debian)
        echo "Debian-based Linux"
        ;;
    rhel|rocky|almalinux|fedora)
        echo "Red Hat-based Linux"
        ;;
    alpine)
        echo "Alpine Linux"
        ;;
    *)
        echo "Unsupported distribution: $ID"
        ;;
esac
```

DevOps application:

- Cross-platform Linux automation.
- Server provisioning.
- Package installation workflows.

## Example 23 — Handle deployment result

Level: Advanced

```bash
#!/bin/bash

deployment_status="${1:-unknown}"

case "$deployment_status" in
    success)
        echo "Deployment completed successfully"
        ;;
    failed)
        echo "Deployment failed. Investigate logs."
        ;;
    rolled-back)
        echo "Deployment was rolled back"
        ;;
    in-progress)
        echo "Deployment is still running"
        ;;
    *)
        echo "Unknown deployment status"
        exit 1
        ;;
esac
```

DevOps application:

- Release status processing.
- Deployment notification scripts.
- CI/CD pipeline reporting.

## Example 24 — Use case with pattern matching and fall-through

Level: Advanced

```bash
#!/bin/bash

environment="staging"

case "$environment" in
    development)
        echo "Development configuration"
        ;&
    staging)
        echo "Staging validation"
        ;&
    production)
        echo "Common deployment checks"
        ;;
    *)
        echo "Unsupported environment"
        ;;
esac
```

Important Bash behavior:

- `;;` stops after the matching block.
- `;&` executes the next block unconditionally.
- `;;&` continues testing subsequent patterns.

DevOps application:

- Advanced deployment workflow control.
- Shared validation logic.

Use fall-through carefully. For most deployment scripts, explicit shared functions are easier to maintain.

## Example 25 — Production-style deployment command router

Level: Advanced

```bash
#!/bin/bash

set -Eeuo pipefail

action="${1:-}"
environment="${2:-staging}"

case "$action" in
    validate)
        echo "Validating deployment configuration"
        ;;
    build)
        echo "Building application image"
        ;;
    test)
        echo "Running application tests"
        ;;
    deploy)
        case "$environment" in
            development|staging)
                echo "Deploying to $environment"
                ;;
            production)
                if [ "${APPROVED:-false}" != "true" ]; then
                    echo "Production approval is required"
                    exit 1
                fi
                echo "Deploying to production"
                ;;
            *)
                echo "Invalid environment"
                exit 1
                ;;
        esac
        ;;
    rollback)
        echo "Executing rollback workflow"
        ;;
    *)
        echo "Usage: $0 {validate|build|test|deploy|rollback} [environment]"
        exit 1
        ;;
esac
```

DevOps application:

- CI/CD command routing.
- Release automation.
- Environment-specific deployment logic.
- Production approval validation.

This is an illustrative command router, not a complete production deployment system.

---

# 4. How These Concepts Are Used Together in DevOps

Real-world DevOps scripts commonly combine all three concepts.

## Example 1 — Application health monitoring

This script:

1. Uses a loop to retry.
2. Uses if/else to check the health endpoint.
3. Uses a condition to decide whether to exit.

```bash
#!/bin/bash

url="http://localhost:8080/health"
max_attempts=5

for ((i=1; i<=max_attempts; i++))
do
    if curl -fsS --max-time 5 "$url" >/dev/null; then
        echo "Application is healthy"
        exit 0
    else
        echo "Health check failed: attempt $i"
        sleep 3
    fi
done

echo "Application is unhealthy"
exit 1
```

## Example 2 — Environment-based deployment

This script:

1. Uses case to select the environment.
2. Uses if/else to validate configuration.
3. Executes the relevant operation.

```bash
#!/bin/bash

environment="${1:-}"

case "$environment" in
    development)
        namespace="development"
        ;;
    staging)
        namespace="staging"
        ;;
    production)
        namespace="production"
        ;;
    *)
        echo "Invalid environment"
        exit 1
        ;;
esac

if kubectl get namespace "$namespace" >/dev/null 2>&1; then
    echo "Namespace exists: $namespace"
else
    echo "Namespace does not exist"
    exit 1
fi

kubectl get deployments -n "$namespace"
```

## Example 3 — Check multiple servers

This script:

1. Uses a loop to process servers.
2. Uses if/else to check SSH availability.
3. Skips unavailable servers.

```bash
#!/bin/bash

servers=("web01" "web02" "db01")

for server in "${servers[@]}"
do
    echo "Checking $server"

    if ssh -o BatchMode=yes -o ConnectTimeout=5 "$server" "hostname"; then
        echo "$server is reachable"
    else
        echo "$server is unavailable"
    fi
done
```

## Example 4 — Docker container management

This script uses case for operations and if/else for validation.

```bash
#!/bin/bash

action="${1:-}"
container="student-service"

if ! docker inspect "$container" >/dev/null 2>&1; then
    echo "Container does not exist"
    exit 1
fi

case "$action" in
    start)
        docker start "$container"
        ;;
    stop)
        docker stop "$container"
        ;;
    restart)
        docker restart "$container"
        ;;
    logs)
        docker logs --tail 100 "$container"
        ;;
    *)
        echo "Usage: $0 {start|stop|restart|logs}"
        exit 1
        ;;
esac
```

## Example 5 — Kubernetes deployment verification

```bash
#!/bin/bash

set -Eeuo pipefail

environment="${1:-staging}"

case "$environment" in
    development|staging|production)
        namespace="$environment"
        ;;
    *)
        echo "Invalid environment"
        exit 1
        ;;
esac

deployments=("student-service" "course-service")

for deployment in "${deployments[@]}"
do
    echo "Checking $deployment"

    if kubectl rollout status deployment/"$deployment" \
        -n "$namespace" --timeout=60s; then
        echo "$deployment deployment is ready"
    else
        echo "$deployment deployment failed"
        exit 1
    fi
done

echo "All deployments are ready"
```

---

# 5. Production Best Practices

## 5.1 Always quote variables

Avoid:

```bash
if [ $filename = "app.log" ]; then
    echo "Found"
fi
```

Prefer:

```bash
if [ "$filename" = "app.log" ]; then
    echo "Found"
fi
```

Quoting protects against spaces and empty variables.

## 5.2 Use meaningful exit codes

```bash
#!/bin/bash

if [ ! -f "/etc/app.conf" ]; then
    echo "Configuration missing"
    exit 1
fi

echo "Configuration found"
exit 0
```

## 5.3 Use strict mode carefully

```bash
set -Eeuo pipefail
```

Meaning:

- `-e`: Exit on certain unhandled command failures.
- `-E`: Inherit ERR traps in functions and subshell contexts.
- `-u`: Treat unset variables as errors.
- `-o pipefail`: Make a pipeline fail if a command in it fails.

Important: `set -e` has exceptions and context-dependent behavior. It is not a replacement for explicitly checking critical command results.

## 5.4 Use functions for reusable logic

```bash
#!/bin/bash

check_service() {
    local service="$1"

    if systemctl is-active --quiet "$service"; then
        echo "$service is running"
    else
        echo "$service is not running"
        return 1
    fi
}

check_service nginx
check_service ssh
```

## 5.5 Use logging

```bash
#!/bin/bash

LOG_FILE="/tmp/deployment.log"

log() {
    echo "$(date '+%F %T') - $*" | tee -a "$LOG_FILE"
}

log "Starting deployment"
log "Running validation"
log "Deployment completed"
```

For production systems, select an appropriate log destination, permissions, retention policy, and log rotation mechanism.

## 5.6 Avoid hardcoding secrets

Never write passwords, tokens, or private keys directly into shell scripts.

Prefer:

- Approved secret managers.
- CI/CD protected variables.
- Restricted environment files where appropriate.
- Short-lived credentials.
- Managed workload identities.

---

# 6. Hands-on DevOps Assignments

Complete these exercises without copying the solutions first.

## Beginner assignments

- [ ] Create a script to check whether a file exists.
- [ ] Create a script to check whether a user exists.
- [ ] Check whether a Linux service is running.
- [ ] Check whether a number is positive or negative.
- [ ] Print numbers from 1 to 100 using a loop.
- [ ] Print all `.log` files in a directory.
- [ ] Create a case-based menu for Linux commands.

## Intermediate assignments

- [ ] Check disk usage and print a warning above 80%.
- [ ] Check memory availability and print a warning below a threshold.
- [ ] Read server names from a text file.
- [ ] Check SSH connectivity to multiple servers.
- [ ] Check whether Docker is installed.
- [ ] Check the health endpoint of an application.
- [ ] Build a service management script using case.
- [ ] Create a script to check Kubernetes pods in multiple namespaces.
- [ ] Create a script that retries a failed health check five times.

## Advanced assignments

- [ ] Create an automated backup script.
- [ ] Create a multi-environment deployment script.
- [ ] Create a Kubernetes rollout validation script.
- [ ] Create a Docker container monitoring script.
- [ ] Create a script to validate production deployment approval.
- [ ] Create a rolling restart script for multiple Linux servers.
- [ ] Create an operational menu for SRE engineers.
- [ ] Create a log monitoring script that identifies application errors.
- [ ] Create a reusable deployment CLI using case and command-line arguments.
- [ ] Build a combined health-check tool using functions, if/else, loops, and case.

---

# 7. Troubleshooting Common Shell Scripting Errors

## Error 1 — Missing spaces in conditions

Incorrect:

```bash
if[$number -eq 10]; then
    echo "Ten"
fi
```

Correct:

```bash
if [ "$number" -eq 10 ]; then
    echo "Ten"
fi
```

## Error 2 — Missing fi

Incorrect:

```bash
if [ -f app.conf ]; then
    echo "File exists"
```

Correct:

```bash
if [ -f app.conf ]; then
    echo "File exists"
fi
```

## Error 3 — Missing done

Incorrect:

```bash
for name in Linux Docker Kubernetes
do
    echo "$name"
```

Correct:

```bash
for name in Linux Docker Kubernetes
do
    echo "$name"
done
```

## Error 4 — Missing double semicolon in case

Incorrect:

```bash
case "$action" in
    start)
        echo "Starting"
    stop)
        echo "Stopping"
esac
```

Correct:

```bash
case "$action" in
    start)
        echo "Starting"
        ;;
    stop)
        echo "Stopping"
        ;;
esac
```

## Error 5 — Incorrect string comparison

Incorrect:

```bash
if [ "$environment" -eq "production" ]; then
    echo "Production"
fi
```

Correct:

```bash
if [ "$environment" = "production" ]; then
    echo "Production"
fi
```

Use `-eq` for numeric comparison and `=` for string comparison.

---

# 8. Useful Commands for Debugging Bash Scripts

| Command | Purpose |
|---|---|
| `bash script.sh` | Execute a script |
| `bash -x script.sh` | Execute with debugging trace |
| `bash -n script.sh` | Check syntax |
| `shellcheck script.sh` | Static shell script analysis |
| `chmod +x script.sh` | Make script executable |
| `echo $?` | Display last command's exit status |
| `set -x` | Enable command tracing |
| `set +x` | Disable command tracing |

Example:

```bash
bash -n deployment.sh
```

Example with debugging:

```bash
bash -x deployment.sh
```

Install ShellCheck on Ubuntu:

```bash
sudo apt-get update
sudo apt-get install -y shellcheck
```

Run:

```bash
shellcheck deployment.sh
```

---

# 9. Interview Questions

## Beginner

1. What is conditional execution in Bash?
2. What is the difference between `if` and `else`?
3. What is the purpose of `elif`?
4. What does `-f` check?
5. What does `-d` check?
6. What is the difference between `-eq` and `=`?
7. What is an exit status?
8. What is the purpose of `fi`?
9. What is a loop?
10. What is the purpose of `done`?

## Intermediate

11. What is the difference between `while` and `until`?
12. What is the difference between `break` and `continue`?
13. How do you iterate through a file line by line?
14. How do you check whether a Linux service is running?
15. How do you validate an environment variable?
16. What is the purpose of a case statement?
17. What is the difference between `;;` and `;&`?
18. How do you check whether a port is listening?
19. How do you retry a command?
20. How do you process multiple servers using Bash?

## Advanced

21. How would you build a deployment script supporting development, staging, and production?
22. How would you prevent a deployment if disk usage exceeds a threshold?
23. How would you verify Kubernetes rollout status?
24. How would you implement retry logic with exponential backoff?
25. How would you design a production-safe Bash automation script?

---

# 10. Suggested GitHub Repository Structure

Create a repository:

`linux-devops-shell-scripting`

Suggested structure:

```text
linux-devops-shell-scripting/
│
├── README.md
│
├── 01-if-else/
│   ├── 01-positive-negative.sh
│   ├── 02-even-odd.sh
│   ├── 03-file-check.sh
│   ├── 04-directory-check.sh
│   ├── 05-file-size-check.sh
│   └── ...
│
├── 02-loops/
│   ├── 01-basic-for-loop.sh
│   ├── 02-number-loop.sh
│   ├── 03-server-loop.sh
│   ├── 04-file-loop.sh
│   └── ...
│
├── 03-case/
│   ├── 01-basic-case.sh
│   ├── 02-environment-selection.sh
│   ├── 03-service-management.sh
│   ├── 04-docker-operations.sh
│   └── ...
│
├── 04-devops-real-world/
│   ├── application-health-check.sh
│   ├── disk-monitoring.sh
│   ├── docker-monitoring.sh
│   ├── kubernetes-rollout-check.sh
│   ├── backup-automation.sh
│   └── deployment-validation.sh
│
├── screenshots/
│   ├── if-else/
│   ├── loops/
│   └── case/
│
└── docs/
    ├── concepts.md
    ├── troubleshooting.md
    └── interview-questions.md
```

---

# 11. How to Showcase This Learning on GitHub

For every example:

1. Create an individual `.sh` file.
2. Add comments explaining the purpose.
3. Execute the script on Ubuntu.
4. Capture the terminal output.
5. Run `bash -n` to validate syntax.
6. Run ShellCheck where available.
7. Record the expected output.
8. Document one real DevOps use case.

Example:

```bash
bash -n 01-positive-negative.sh
bash 01-positive-negative.sh
```

Commit your work:

```bash
git add .
git commit -m "Add Linux Bash conditional execution examples"
git push
```

## Suggested README learning tracker

| Topic | Examples | Status |
|---|---:|---|
| If/Else | 25 | Not started |
| Loops | 25 | Not started |
| Case | 25 | Not started |
| DevOps combined scripts | 5 | Not started |
| Troubleshooting | Practical exercises | Not started |
| Screenshots | Evidence | Not started |

---

# 12. Recommended Learning Sequence

Do not try to memorize all 75 examples in one sitting.

| Session | Learning focus | Practical goal |
|---|---|---|
| 1 | If/else basics | File, directory, user checks |
| 2 | Numeric and string conditions | Validate configuration |
| 3 | Service and infrastructure conditions | Service, disk, memory checks |
| 4 | Basic for loops | Process files and servers |
| 5 | While and until loops | Retry and polling |
| 6 | Advanced loops | Kubernetes and Docker checks |
| 7 | Basic case statements | Service management |
| 8 | Advanced case statements | Deployment command router |
| 9 | Combined scripting | Build health-check automation |
| 10 | GitHub documentation | Publish code and screenshots |

---

# Final Learning Objective

After completing this guide, you should be able to:

- Write Bash scripts using if/else conditions.
- Use numeric, string, file, and command conditions.
- Automate repetitive work using loops.
- Build interactive and reusable scripts using case.
- Validate Linux services and application health.
- Automate Docker and Kubernetes operational checks.
- Implement retry mechanisms and deployment validations.
- Troubleshoot Bash scripts using debugging and static analysis.
- Build a GitHub portfolio demonstrating practical Linux automation.

**The objective is not just to learn Bash syntax. It is to understand how Linux conditional execution helps DevOps and SRE engineers automate real production operations.**
