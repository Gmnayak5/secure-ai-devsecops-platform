# AWK Command in Linux – 25 Practical Examples for DevOps

## 1. Objective

Learn and practice 25 essential AWK commands using a sample Linux file.

This hands-on exercise is designed for beginners and aspiring:

- Linux System Administrators
- DevOps Engineers
- Site Reliability Engineers (SRE)
- Platform Engineers

**Lab Environment:** Killercoda Linux Terminal

**Sample File:** `file.txt`

---

# 2. Create the Sample File

Execute the following command in your Killercoda terminal.

It creates a sample file containing Linux user information, application logs, failed login attempts, and configuration messages.

```bash
cat > file.txt <<'EOF'
# Linux User Management Sample File
# Format: type:username:UID:role:department:shell

user:admin:1001:admin:IT:/bin/bash
user:alice:1002:developer:Engineering:/bin/zsh
user:bob:1003:tester:QA:/bin/bash
user:charlie:1004:admin:Operations:/bin/sh
user:david:1005:developer:Engineering:/bin/bash
user:anna:1006:tester:QA:/bin/zsh
user:ben:1007:admin:Security:/bin/bash
user:john:999:guest:Support:/bin/sh

# Application Log Records
2026-11-20 10:00:01 login success user admin from 192.168.1.10
2026-11-20 10:05:12 login failed user alice from 192.168.1.15
2026-11-20 10:10:25 login failed user bob from 192.168.1.20
2026-11-20 10:15:30 service started successfully
2026-11-20 10:20:45 old configuration detected
2026-11-20 10:25:50 old backup file found
2026-11-20 10:30:00 deployment failed due to permission denied

EOF
```

## 3. Verify the Sample File

### Print the complete file

```bash
cat file.txt
```

### Check the number of lines

```bash
wc -l file.txt
```

### Display colon-separated fields

```bash
awk -F':' '/^user/ {print $1, $2, $3, $4, $5, $6}' file.txt
```

---

# 4. AWK Basics – Understanding Fields

Before executing the examples, understand these important AWK variables.

| AWK Variable | Meaning | Example |
|---|---|---|
| `$0` | Entire current line | Print complete line |
| `$1` | First field | First column |
| `$2` | Second field | Second column |
| `$3` | Third field | Third column |
| `$NF` | Last field | Last column |
| `NF` | Number of fields | Total columns |
| `NR` | Current record/line number | Line number |
| `FS` | Input field separator | Input delimiter |
| `OFS` | Output field separator | Output delimiter |

**Important:** AWK uses whitespace as its default field separator. You can change it using `-F`.

Example:

```bash
awk -F':' '{print $2}' file.txt
```

Here, the colon (`:`) is the delimiter.

---

# 5. Basic AWK Commands

## Example 1: Print the entire file

```bash
awk '{print}' file.txt
```

Explanation: Prints every line from the file.

## Example 2: Print the first column

```bash
awk '{print $1}' file.txt
```

Explanation: Prints the first space-separated field of every line.

## Example 3: Print the first and third columns

```bash
awk '{print $1, $3}' file.txt
```

Explanation: Prints only the first and third fields.

## Example 4: Print the last column

```bash
awk '{print $NF}' file.txt
```

Explanation: `$NF` represents the last field in the current record.

## Example 5: Print the second-last column

```bash
awk '{print $(NF-1)}' file.txt
```

Explanation: Prints the field immediately before the last field.

## Example 6: Print usernames and shells

```bash
awk -F':' '/^user/ {print $2, $6}' file.txt
```

Explanation:
- `-F':'` sets colon as delimiter.
- `/^user/` selects lines starting with user.
- `$2` prints username.
- `$6` prints login shell.

## Example 7: Print custom labels

```bash
awk -F':' '/^user/ {print "User:", $2, "| Role:", $4}' file.txt
```

Explanation: Adds custom labels to the output.

## Example 8: Change output field separator to comma

```bash
awk -F':' 'BEGIN{OFS=","} /^user/ {print $2,$3,$4}' file.txt
```

Explanation:
- `BEGIN` executes before reading the file.
- `OFS=","` changes the output separator.
- Prints username, UID and role separated by commas.

---

# 6. Filtering Rows and Pattern Matching

## Example 9: Find lines containing failed

```bash
awk '/failed/ {print}' file.txt
```

Explanation: Displays lines containing the word `failed`.

DevOps use case: Searching application logs for failed operations.

## Example 10: Exclude comment lines

```bash
awk '!/^#/ {print}' file.txt
```

Explanation: Prints lines that do not start with `#`.

DevOps use case: Ignoring comments in configuration files.

## Example 11: Find users with UID greater than 1001

```bash
awk -F':' '$3 > 1001 {print $2, $3}' file.txt
```

Explanation: Checks whether the third colon-separated field is greater than 1001.

## Example 12: Find admin users

```bash
awk -F':' '$4 == "admin" {print $0}' file.txt
```

Explanation: Displays complete records where the fourth field equals `admin`.

## Example 13: Find usernames starting with a or b

```bash
awk -F':' '$2 ~ /^[ab]/ {print $2}' file.txt
```

Explanation:
- `~` means pattern matching.
- `^` means starts with.
- `[ab]` means either a or b.

Expected usernames:

```text
alice
bob
anna
ben
```

## Example 14: Print lines containing more than 5 fields

```bash
awk 'NF > 5 {print}' file.txt
```

Explanation: Prints lines having more than five whitespace-separated fields.

## Example 15: Print non-empty lines

```bash
awk 'NF > 0 {print}' file.txt
```

Explanation: Displays only lines containing fields and skips blank lines.

---

# 7. Line Numbers and Ranges

## Example 16: Print line numbers

```bash
awk '{print NR, $0}' file.txt
```

Explanation: `NR` represents the current record number.

## Example 17: Print only line number 4

```bash
awk 'NR == 4 {print}' file.txt
```

Explanation: Displays only the fourth physical line.

## Example 18: Print lines 2 through 5

```bash
awk 'NR >= 2 && NR <= 5 {print}' file.txt
```

Explanation:
- `NR >= 2` selects lines starting from line 2.
- `NR <= 5` selects lines up to line 5.
- `&&` means AND.

## Example 19: Count total lines

```bash
awk 'END {print "Total lines:", NR}' file.txt
```

Explanation:
- `END` executes after processing the file.
- `NR` contains the total number of records processed.

---

# 8. Calculations and Built-in Functions

## Example 20: Calculate the sum of all user IDs

```bash
awk -F':' '/^user/ {sum += $3} END {print "UID Sum:", sum}' file.txt
```

Expected output:

```text
UID Sum: 8027
```

Explanation: Adds the third field (UID) for every line starting with `user`.

## Example 21: Count the number of user records

```bash
awk '/^user/ {count++} END {print count}' file.txt
```

Expected output:

```text
8
```

Explanation: Increments the counter for every user record.

## Example 22: Convert usernames to uppercase

```bash
awk -F':' '/^user/ {print toupper($2)}' file.txt
```

Explanation: `toupper()` converts lowercase characters into uppercase.

## Example 23: Print character length of each line

```bash
awk '{print length($0), $0}' file.txt
```

Explanation: `length()` calculates the number of characters in the current line.

## Example 24: Replace old with NEW

```bash
awk '{gsub(/old/, "NEW"); print}' file.txt
```

Explanation:
- `gsub()` replaces all matching occurrences.
- `/old/` identifies the text to replace.
- `"NEW"` is the replacement text.

## Example 25: Add a header and footer

```bash
awk -F':' 'BEGIN{print "--- USERS ---"} /^user/{print $2} END{print "--- DONE ---"}' file.txt
```

Expected output:

```text
--- USERS ---
admin
alice
bob
charlie
david
anna
ben
john
--- DONE ---
```

Explanation:
- `BEGIN` prints before processing.
- Main AWK block processes user records.
- `END` prints after processing.

---

# 9. Real-World AWK Examples for Linux and DevOps

These examples use actual Linux system information rather than `file.txt`.

## Example 26: Calculate total memory used by running processes

```bash
ps aux | awk '{sum += $6} END {print sum/1024 " MB"}'
```

Explanation:
- `ps aux` displays running processes.
- `$6` represents RSS memory in KB.
- `sum += $6` accumulates memory usage.
- Dividing by 1024 converts KB to MB.

Note: This sums process RSS values, which can double-count shared memory.

## Example 27: List Linux user accounts with UID >= 1000

```bash
awk -F':' '$3 >= 1000 {print $1, $3}' /etc/passwd
```

Explanation:
- `/etc/passwd` stores local account information.
- `$1` is the username.
- `$3` is the UID.

Note: UID 1000 and above is a common human-user convention on many Linux distributions, but the actual UID_MIN setting may differ.

## Example 28: Find the top 5 IP addresses in Nginx access logs

```bash
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -5
```

Explanation:
- `awk '{print $1}'` extracts client IP addresses.
- `sort` sorts the addresses.
- `uniq -c` counts occurrences.
- `sort -nr` sorts by count in descending order.
- `head -5` displays the top five.

Note: Requires an existing Nginx access log with the client IP as its first field.

## Example 29: Display disk usage and mount points

```bash
df -h | awk '{print $5, $6}'
```

Explanation:
- `df -h` displays filesystem disk usage.
- `$5` represents usage percentage.
- `$6` represents mount point.

---

# 10. Additional AWK Practice Commands

Try these additional commands to improve your understanding.

## Practice 1: Display number of fields in every line

```bash
awk '{print NF}' file.txt
```

## Practice 2: Display line number and number of fields

```bash
awk '{print "Line:", NR, "Fields:", NF}' file.txt
```

## Practice 3: Print only records having exactly 6 fields

```bash
awk 'NF == 6 {print}' file.txt
```

## Practice 4: Find usernames with UID greater than 1003

```bash
awk -F':' '/^user/ && $3 > 1003 {print $2, $3}' file.txt
```

## Practice 5: Count admin users

```bash
awk -F':' '$1 == "user" && $4 == "admin" {count++} END {print "Admin count:", count}' file.txt
```

---

# 11. Important AWK Concepts to Remember

| Concept | Meaning | Example |
|---|---|---|
| `$0` | Complete line | `print $0` |
| `$1` | First field | `print $1` |
| `$2` | Second field | `print $2` |
| `$NF` | Last field | `print $NF` |
| `NF` | Number of fields | `print NF` |
| `NR` | Record number | `print NR` |
| `-F` | Set input delimiter | `-F':'` |
| `FS` | Input field separator | `BEGIN{FS=":"}` |
| `OFS` | Output field separator | `BEGIN{OFS=","}` |
| `BEGIN` | Runs before input processing | `BEGIN{print "Start"}` |
| `END` | Runs after input processing | `END{print NR}` |
| `gsub()` | Replace all matching occurrences | `gsub(/old/,"NEW")` |
| `toupper()` | Convert to uppercase | `toupper($2)` |
| `length()` | Calculate string length | `length($0)` |

---

# 12. Student Hands-on Assignment

Complete these exercises without looking at the solutions.

- [ ] Print only usernames.
- [ ] Print only usernames and their departments.
- [ ] Find all users having the developer role.
- [ ] Find users whose UID is greater than 1004.
- [ ] Count how many users have the admin role.
- [ ] Display all failed login records.
- [ ] Count the number of failed login attempts.
- [ ] Print only application log records containing deployment.
- [ ] Replace every occurrence of `failed` with `ERROR`.
- [ ] Print the last field from every user record.
- [ ] Display usernames in uppercase.
- [ ] Print all records with more than 7 whitespace-separated fields.
- [ ] Display the total number of non-empty lines.
- [ ] Display usernames with their login shells in CSV format.
- [ ] Find all users with UID below 1000.

---

# 13. DevOps Production Use Cases of AWK

AWK is frequently useful in shell scripts and Linux troubleshooting.

| DevOps Scenario | AWK Application |
|---|---|
| Application troubleshooting | Extract error and failed messages |
| Nginx troubleshooting | Identify frequently accessing IP addresses |
| Linux administration | Read `/etc/passwd` and extract users |
| Server monitoring | Extract memory and CPU-related information |
| Disk monitoring | Extract disk usage percentages |
| CI/CD pipelines | Parse command output and build logs |
| Security monitoring | Identify failed login attempts |
| Automation | Extract required columns from command output |
| Log analysis | Filter records using patterns and conditions |

---

# 14. Final Lab Verification

Execute the following commands after completing the practice.

```bash
# Verify file exists
ls -l file.txt

# Verify file contents
cat file.txt

# Count physical lines
wc -l file.txt

# Count user records
grep -c '^user:' file.txt

# Count failed login records
grep -c 'failed' file.txt

# Print all usernames
awk -F':' '/^user/ {print $2}' file.txt

# Print all admin usernames
awk -F':' '$1 == "user" && $4 == "admin" {print $2}' file.txt

# Print total UID sum
awk -F':' '/^user/ {sum += $3} END {print sum}' file.txt
```

## Learning Outcome

After completing this lab, students should understand:

1. How AWK reads and processes files.
2. How to extract individual columns.
3. How to filter records using conditions.
4. How to work with delimiters.
5. How to process Linux user and application log files.
6. How to perform calculations using AWK.
7. How AWK supports Linux administration and DevOps automation.

**Practice philosophy:** Predict the output → Execute the command → Verify the output → Explain the command in your own words.

**Lab Environment:** Killercoda  
**Practice File:** `file.txt`  
**Topic:** Linux AWK Commands for DevOps and System Administration
