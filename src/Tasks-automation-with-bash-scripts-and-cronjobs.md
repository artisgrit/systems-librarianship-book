# Tasks Automation with Bash Scripts and Cron Jobs

## Learning Objectives

By the end of this lesson, you should be able to:

- Explain the purpose and benefits of task automation.
- Create and execute Bash scripts.
- Use variables, user input, loops, conditionals, and functions in Bash.
- Implement error handling and logging techniques.
- Automate common Linux administrative tasks.
- Configure and manage cron jobs.
- Troubleshoot automation workflows.
- Design reliable and secure automation solutions.
- Develop real-world automation projects using Bash and cron.

---

# What Is Task Automation?

Task automation is the process of using software to perform repetitive tasks automatically without requiring continuous human intervention.

In Linux environments, system administrators often perform recurring activities such as:

- Creating backups
- Monitoring system resources
- Cleaning temporary files
- Updating software packages
- Generating reports
- Monitoring websites and services

Performing these tasks manually can be inefficient and error-prone. Automation allows these operations to execute consistently and reliably.

### Example

Without automation:

```bash
tar -czf backup.tar.gz /home/user/Documents
```

must be executed manually every day.

With automation:

- A Bash script performs the backup.
- A cron job schedules it to run nightly.

### Benefits of Automation

- Reduces repetitive work
- Improves consistency
- Saves time
- Reduces human error
- Enhances system reliability
- Enables scalability

---

# Why Automate Tasks?

Modern computing environments contain hundreds or thousands of systems.

Manual administration does not scale effectively.

Consider a company with:

- 100 servers
- Daily backups
- Weekly software updates
- Continuous monitoring requirements

Without automation, administrators would spend most of their time performing routine maintenance.

Automation allows administrators to focus on:

- System design
- Security improvements
- Performance optimization
- Problem-solving

### Real-World Examples

| Task | Manual Method | Automated Method |
|--------|--------|--------|
| Backup files | Run backup commands daily | Schedule backup script |
| Monitor disk space | Check manually | Automated monitoring script |
| Update software | Run updates periodically | Scheduled update job |
| Website monitoring | Manual testing | Automated health checks |

---

# Understanding Bash

Bash (Bourne Again Shell) is one of the most widely used command-line shells on Linux systems.

Bash serves two primary purposes:

1. Interactive command execution
2. Scripting and automation

Examples of Bash commands:

```bash
pwd
ls
date
whoami
```

Bash scripting allows multiple commands to be stored inside a file and executed as a single program.

### Why Bash for Automation?

Bash is:

- Available on most Linux systems
- Lightweight
- Easy to learn
- Excellent for system administration tasks
- Highly integrated with Linux utilities

---

# What Is a Bash Script?

A Bash script is a plain-text file containing Bash commands.

Example:

```bash
#!/bin/bash

echo "System Report"
date
uptime
```

Save the file:

```text
system-report.sh
```

Make it executable:

```bash
chmod +x system-report.sh
```

Run it:

```bash
./system-report.sh
```

### Advantages of Scripts

- Reusable
- Easy to modify
- Easy to schedule
- Consistent execution

---

# Understanding the Shebang

Most Bash scripts begin with:

```bash
#!/bin/bash
```

This line is called the **shebang**.

It tells Linux which interpreter should execute the script.

Alternative example:

```bash
#!/usr/bin/env bash
```

This version searches the user's environment for Bash and is often considered more portable.

### Why the Shebang Matters

Without a shebang:

- Scripts may execute with the wrong shell.
- Commands may behave differently.
- Compatibility issues may occur.

---

# Comments and Documentation

Comments improve script readability and maintainability.

Example:

```bash
# Backup user documents
tar -czf backup.tar.gz ~/Documents
```

### Best Practices

- Explain script purpose.
- Document complex logic.
- Add author information.
- Include usage instructions.

Example:

```bash
#!/bin/bash

# Script Name: backup.sh
# Purpose: Create daily backups
# Author: Student Name
```

---

# Variables

Variables store information.

Example:

```bash
username="student"
```

Display variable contents:

```bash
echo $username
```

Output:

```text
student
```

### Common Variable Types

```bash
backup_dir="/backup"
today=$(date)
hostname=$(hostname)
```

### Benefits

Variables make scripts:

- Flexible
- Easier to maintain
- More readable

---

# User Input

Scripts can collect information from users.

Example:

```bash
echo "Enter your name:"
read username

echo "Hello $username"
```

### Password Input

Hide typed characters:

```bash
read -s password
```

### Practical Uses

- Selecting backup locations
- Providing usernames
- Choosing options

---

# Command Substitution

Store command output inside variables.

Example:

```bash
current_date=$(date)
```

Equivalent older syntax:

```bash
current_date=`date`
```

Preferred syntax:

```bash
$(command)
```

### Examples

```bash
hostname=$(hostname)
uptime=$(uptime)
disk_usage=$(df -h)
```

---

# Arithmetic Operations

Perform calculations inside scripts.

Example:

```bash
x=5
y=10

result=$((x + y))
echo $result
```

Output:

```text
15
```

### Common Operations

```text
+
-
*
/
%
```

### Practical Uses

- Resource calculations
- Counters
- Statistics

---

# Conditional Statements

Conditional statements allow decision-making.

## if Statement

```bash
if [ condition ]
then
    command
fi
```

Example:

```bash
if [ -f file.txt ]
then
    echo "File exists"
fi
```

## if-else

```bash
if [ $disk_usage -gt 80 ]
then
    echo "Warning"
else
    echo "OK"
fi
```

## Comparison Operators

| Operator | Meaning |
|------------|------------|
| -eq | Equal |
| -ne | Not Equal |
| -gt | Greater Than |
| -lt | Less Than |
| -ge | Greater or Equal |
| -le | Less or Equal |

---

# Loops

Loops repeat actions.

## for Loop

```bash
for file in *.txt
do
    echo $file
done
```

## while Loop

```bash
count=1

while [ $count -le 5 ]
do
    echo $count
    count=$((count+1))
done
```

### Practical Uses

- Process files
- Iterate through users
- Monitor systems

---

# Functions

Functions organize reusable code.

Example:

```bash
backup() {
    echo "Creating backup..."
}
```

Call function:

```bash
backup
```

### Advantages

- Reusability
- Cleaner code
- Easier maintenance

---

# Exit Codes and Error Handling

Commands return exit codes.

```text
0 = Success
Non-zero = Failure
```

Check status:

```bash
echo $?
```

Example:

```bash
mkdir backup

if [ $? -eq 0 ]
then
    echo "Directory created"
fi
```

### Error Handling Best Practices

```bash
set -e
```

Stop script when an error occurs.

---

# Working with Files and Directories

Common commands:

```bash
cp
mv
rm
mkdir
touch
find
```

Example:

```bash
mkdir reports
cp report.txt reports/
```

### File Testing

```bash
-f file
-d directory
-r readable
-w writable
-x executable
```

---

# Logging and Monitoring

Logs provide visibility into script execution.

Example:

```bash
echo "$(date): Backup completed" >> backup.log
```

### Benefits

- Troubleshooting
- Auditing
- Monitoring

---

# Practical Automation Examples

## Automated Backups

Create compressed backups using:

```bash
tar -czf backup.tar.gz /home/user/Documents
```

### Learning Focus

- Compression
- Archiving
- Scheduling backups

## Log Cleanup

```bash
find /var/log -name "*.log" -mtime +30 -delete
```

### Learning Focus

- File management
- Storage maintenance

## System Health Reports

Collect:

```bash
uptime
free -h
df -h
```

Generate reports automatically.

## Website Monitoring

```bash
curl -I https://example.com
```

Check availability and response status.

## Database Backups

```bash
mysqldump database_name > backup.sql
```

Protect critical data.

## Software Updates

```bash
sudo apt update
sudo apt upgrade -y
```

Keep systems secure.

---

# Cron Jobs

## What Is Cron?

Cron is a Linux scheduling service.

It executes commands automatically according to predefined schedules.

Think of cron as an alarm clock for Linux tasks.

## The Cron Daemon

Cron runs continuously in the background.

Check status:

```bash
systemctl status cron
```

## Understanding Crontabs

View jobs:

```bash
crontab -l
```

Edit jobs:

```bash
crontab -e
```

Delete jobs:

```bash
crontab -r
```

## Cron Syntax Explained

```text
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of Week
│ │ │ └──── Month
│ │ └────── Day of Month
│ └──────── Hour
└────────── Minute
```

Example:

```text
0 2 * * * /home/user/backup.sh
```

Runs daily at 2:00 AM.

## Cron Special Strings

```text
@reboot
@hourly
@daily
@weekly
@monthly
```

## Scheduling Bash Scripts

```text
0 1 * * * /home/user/scripts/backup.sh
```

## Redirecting Output and Errors

```text
0 1 * * * script.sh >> backup.log 2>&1
```

## Environment Variables in Cron

Cron uses a limited environment.

Specify PATH:

```text
PATH=/usr/local/bin:/usr/bin:/bin
```

## Debugging Cron Jobs

Check logs:

```bash
grep CRON /var/log/syslog
```

## System-Wide Cron Jobs

Located in:

```text
/etc/crontab
/etc/cron.daily
/etc/cron.weekly
```

## Security Considerations

- Avoid hardcoded passwords.
- Restrict permissions.
- Validate user input.
- Protect sensitive data.

---

# Real-World Automation Projects

## Daily Backup System

Automate backups every night.

### Objectives

- Create compressed archives.
- Store backups in a dedicated directory.
- Add timestamps to backup filenames.
- Schedule backups with cron.

## Disk Monitoring and Alerts

Notify administrators when disk usage exceeds thresholds.

### Objectives

- Check disk usage using `df`.
- Compare usage against thresholds.
- Log warnings.
- Send notifications.

## Web Server Monitoring

Verify web service availability and restart services if necessary.

### Objectives

- Check HTTP response codes.
- Detect downtime.
- Restart failed services.
- Record incidents in log files.

## User Account Auditing

Generate reports of inactive accounts.

### Objectives

- List user accounts.
- Check login activity.
- Identify dormant users.
- Generate audit reports.

## Automatic Report Generation

Create scheduled system reports.

### Include

- CPU usage
- Memory usage
- Disk usage
- Uptime
- Running services

---

# Best Practices

1. Use meaningful variable names.
2. Add comments.
3. Use absolute paths.
4. Implement logging.
5. Validate input.
6. Test before scheduling.
7. Handle errors gracefully.
8. Limit permissions.
9. Document scripts.
10. Keep scripts modular.

---

# Troubleshooting

## Script Won't Execute

Check permissions:

```bash
chmod +x script.sh
```

Verify the shebang exists:

```bash
#!/bin/bash
```

## Cron Job Doesn't Run

Verify scheduled jobs:

```bash
crontab -l
```

Check cron service:

```bash
systemctl status cron
```

Review logs:

```bash
grep CRON /var/log/syslog
```

## Commands Work Manually But Not in Cron

Specify full paths:

```bash
/usr/bin/python3
/usr/bin/tar
/usr/bin/curl
```

Cron often uses a limited environment.

## Script Produces Unexpected Results

Enable debugging:

```bash
bash -x script.sh
```

Or add:

```bash
set -x
```

inside the script.

---

# Conclusion

Task automation is one of the most valuable skills for Linux administrators and DevOps professionals.

Bash scripts allow repetitive tasks to be automated, while cron jobs provide a mechanism for scheduling those tasks.

Together they increase efficiency, improve reliability, reduce human error, and enable scalable system management.

As your skills grow, you will find that nearly every repetitive administrative task can be automated using Bash and cron. Mastering these tools is a significant step toward becoming an effective Linux professional.

---

# Appendix: Common Bash Commands

```text
pwd
ls
cd
cp
mv
rm
mkdir
touch
find
grep
awk
sed
tar
chmod
chown
```

## Command Categories

### File Management

```bash
cp
mv
rm
touch
mkdir
```

### Searching and Filtering

```bash
find
grep
awk
sed
```

### Permissions

```bash
chmod
chown
```

### Archiving

```bash
tar
gzip
gunzip
```

---

# Appendix: Common Cron Expressions

```text
* * * * *      Every minute
*/5 * * * *    Every 5 minutes
*/15 * * * *   Every 15 minutes
0 * * * *      Every hour
0 0 * * *      Daily at midnight
0 6 * * *      Daily at 6 AM
0 0 * * 0      Weekly
0 0 1 * *      Monthly
```

## Special Strings

```text
@reboot
@hourly
@daily
@weekly
@monthly
@yearly
```

---

# Appendix: Sample Automation Scripts

## Backup Script

```bash
#!/bin/bash

BACKUP_DIR="/backups"
DATE=$(date +%F)

mkdir -p "$BACKUP_DIR"

tar -czf "$BACKUP_DIR/backup-$DATE.tar.gz" ~/Documents

echo "$(date): Backup completed."
```

## Disk Monitoring Script

```bash
#!/bin/bash

usage=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')

if [ "$usage" -gt 80 ]
then
    echo "Disk usage warning: ${usage}%"
fi
```

## Daily Report Script

```bash
#!/bin/bash

echo "System Report"
echo "-------------"

date
echo

uptime
echo

free -h
echo

df -h
```

## Website Monitoring Script

```bash
#!/bin/bash

website="https://example.com"

if curl -s --head "$website" > /dev/null
then
    echo "Website is online."
else
    echo "Website is unreachable."
fi
```

## Log Cleanup Script

```bash
#!/bin/bash

find /var/log -name "*.log" -mtime +30 -delete

echo "Old log files removed."
```
