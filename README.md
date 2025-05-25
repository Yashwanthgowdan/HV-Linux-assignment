# HV-Linux-assignment
Testing, Linux and Servers assignment

##Task 1: System Monitoring Setup##
Install htop - sudo apt install -y htop
Run htop:
 ![image](https://github.com/user-attachments/assets/566e2b5a-0e84-476f-ad51-4f8f5f159594)

Install nmon - sudo apt install -y nmon
Run nmon:
Enter keys to get:
c — CPU
m — Memory
d — Disk I/O
n — Network
t — Top processes
![image](https://github.com/user-attachments/assets/723cde27-017b-4217-88be-9e141efb0636)

 
To Check Disk Usage and Tracking using df and du
Check disk space usage and store the output in the disk_usage.log file:
df -h >> /log/disk_tracking/disk_usage.log
Check directory sizes of home directory and store the output in the disk_usage.log file:
du -sh /home/* >> / /log/disk_tracking/disk_usage.log 
![image](https://github.com/user-attachments/assets/00125328-e9ed-48db-a898-882f71b101f9)

To monitor resource-intensive Processes
ps aux --sort=-%mem | head -n 10 >> ~/log/disk_tracking/top_mem_processes.log 
![image](https://github.com/user-attachments/assets/de10da1c-1e17-4901-b364-44b8a3c51c1c)

Bash script to run daily: 
![image](https://github.com/user-attachments/assets/c06433e4-a0e3-42fd-b4f5-8bbf565c4bfd)

Cron job to run every day at 7 AM:
![image](https://github.com/user-attachments/assets/1e22273e-70da-48f3-9db4-01c5f52a92e9)

##Task 2: User Management and Access Control##
>Creating user accounts for Sarah and Mike with secure passwords:
sudo adduser sarah

sudo useradd -m mike
sudo passwd mike
 
cut -d: -f1 /etc/passwd
![image](https://github.com/user-attachments/assets/13398416-49cb-4fc1-9529-6832cb1c61ec)

>Setting up isolated directories with appropriate permissions:
sudo mkdir -p /home/Sarah/workspace
sudo mkdir -p /home/Mike/workspace
![image](https://github.com/user-attachments/assets/968ed09e-a476-4a37-a9be-69bcb69fa217)

sudo chown sarah Sarah/workspace/
sudo chown mike Mike/workspace/
![image](https://github.com/user-attachments/assets/81816205-f3d7-445d-8c91-7ba062bc44dd)

>Enforcing a password policy with expiration and complexity:
 sudo chage -l sarah
![image](https://github.com/user-attachments/assets/48d9399d-1492-446b-b8af-f9b3b0852280)

Changed the maximum password age to 30 days from 99999 with following commands:
 sudo chage -M 30 sarah
 sudo chage -M 30 mike
After changes:
 sudo chage -l mike
 sudo chage -l sarah
 ![image](https://github.com/user-attachments/assets/f73a9fab-76a9-4c3c-b595-c4f21711a904)

##Task 3: Backup Configuration for Web Servers##
 sudo apt install -y nginx
 sudo apt install -y apache2
>Created backup scripts for Sarah and Mike
Sarah’s backup script: 
sudo nano -p /usr/local/bin/backup_apache.sh
Script:
#!/bin/bash
tar -czf /home/ubuntu/backups/apache_backup_$(date +%F).tar.gz /etc/apache2 /var/www/html

Mike’s backup script: 
sudo nano -p /usr/local/bin/backup_nginx.sh
Script:
#!/bin/bash
tar -czf /home/ubuntu/backups/nginx_backup_$(date +%F).tar.gz /etc/nginx /usr/share/nginx/html

>Set permissions to make scripts executable:
sudo chmod +x /usr/local/bin/backup_apache.sh
sudo chmod +x /usr/local/bin/backup_nginx.sh

>Ensured backup directories exist:
mkdir -p /home/ubuntu/backups

>Tested manual script execution with:
sudo /usr/local/bin/backup_apache.sh
![image](https://github.com/user-attachments/assets/dc608410-ef22-45b1-9cd3-b69a6bd0c1e3)

sudo /usr/local/bin/backup_nginx.sh
![image](https://github.com/user-attachments/assets/47275ea2-d49d-47b3-b7c3-038ee1dc67e7)


>Scheduled cron jobs to run backups every Tuesday 12 AM:
crontab -e
Sarah: 0 0 * * 2 /usr/local/bin/backup_apache.sh
Mike: 0 0 * * 2 /usr/local/bin/backup_nginx.sh
![image](https://github.com/user-attachments/assets/992c3c84-d940-47a8-a29a-c736f4828cbb)

