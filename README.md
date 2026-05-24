# LinuxAssignmentSanjeev

Please note my WSL does not work on my system, so I have used killercoda.com.

Question 1: Set Up Your DevOps Project Structure

## Commands Used

mkdir -p /home/ec2-user/webapp/scripts /home/ec2-user/webapp/logs /home/ec2-user/webapp/config
Created directories:
scripts
logs
config
-p creates parent directories if they don’t exist.

cat > /home/ec2-user/webapp/config/app.conf
Created the file app.conf

Allowed you to enter content manually:

APP_NAME=WebApp
PORT=8080
Pressing Ctrl + D saves and exits.
You pressed Ctrl + C, but the file had already been written successfully.

touch /home/ec2-user/webapp/logs/app.log
Created an empty log file named app.log.
chmod 755 /home/ec2-user/webapp/scripts

Permission breakdown for scripts:

Owner → read, write, execute
Group → read, execute
Others → read, execute

755=7(rwx)5(r−x)5(r−x)

chmod 644 /home/ec2-user/webapp/config/app.conf

Permission breakdown for app.conf:

Owner → read, write
Group → read only
Others → read only

644=6(rw−)4(r−−)4(r−−)

sudo chown -R root:root /home/ec2-user/webapp/
Changed ownership recursively (-R)
Owner = root
Group = root

Your final structure:

/home/ec2-user/webapp/
├── config/
│   └── app.conf
├── logs/
│   └── app.log
└── scripts/

Your verification command:

cat /home/ec2-user/webapp/config/app.conf

Correctly displayed:

APP_NAME=WebApp
PORT=8080



<img width="1036" height="453" alt="image" src="https://github.com/user-attachments/assets/325f2be1-fc60-4cc7-9949-33311f64eae5" />


 

Question 2: Write an Interactive Log Script


 User Login Logging Script

## Commands Used

```bash
# Navigate to scripts directory
cd /home/ec2-user/webapp/scripts/

# Create script file
vim log_user.sh

# View script content
cat log_user.sh

# Give execute permission
chmod +x log_user.sh

# Verify permissions
ls -ltr log_user.sh

# Run the script
./log_user.sh
```

## Script Content

```bash
#!/bin/bash

read -p "Enter your name: " username

cat /home/ec2-user/webapp/config/app.conf

echo "Login: $username Date: $(date)" >> /home/ec2-user/webapp/logs/app.log

cat /home/ec2-user/webapp/logs/app.log
```

## Sample Output

```text
Enter your name: Sanjeev
APP_NAME=WebApp
PORT=8080
Login: Sanjeev Date: Thu May 21 12:29:58 UTC 2026
```

```text
Enter your name: Kumar
APP_NAME=WebApp
PORT=8080
Login: Sanjeev Date: Thu May 21 12:29:58 UTC 2026
Login: Kumar Date: Thu May 21 12:30:07 UTC 2026
```

```text
Enter your name: Goud
APP_NAME=WebApp
PORT=8080
Login: Sanjeev Date: Thu May 21 12:29:58 UTC 2026
Login: Kumar Date: Thu May 21 12:30:07 UTC 2026
Login: Goud Date: Thu May 21 12:30:18 UTC 2026
```

 <img width="940" height="389" alt="image" src="https://github.com/user-attachments/assets/20de635c-877c-46a7-8ddf-4afc4078ac9b" />


Question 3: User Management and File Permission Control


# User and Group Management

## Commands Used

```bash
# Create a new group
sudo groupadd writers

# Create users with home directories
sudo useradd -m devuser1
sudo useradd -m devuser2
sudo useradd -m devuser3
sudo useradd -m devuser4

# Add users to the writers group
sudo usermod -aG writers devuser1
sudo usermod -aG writers devuser2

# Change group ownership of the script
sudo chown root:writers /home/ec2-user/webapp/scripts/log_user.sh
```

## Explanation

### Create Group
```bash
sudo groupadd writers
```
Creates a new group named `writers`.

---

### Create Users
```bash
sudo useradd -m devuser1
```

- `useradd` → creates a new user
- `-m` → creates a home directory automatically

Users created:
- `devuser1`
- `devuser2`
- `devuser3`
- `devuser4`

---

### Add Users to Group
```bash
sudo usermod -aG writers devuser1
```

- `usermod` → modifies user account
- `-aG` → append user to supplementary group

Added users:
- `devuser1`
- `devuser2`

to the `writers` group.

---

### Change File Group Ownership
```bash
sudo chown root:writers /home/ec2-user/webapp/scripts/log_user.sh
```

Changes:
- Owner → `root`
- Group → `writers`

for the script file `log_user.sh`.

<img width="940" height="154" alt="image" src="https://github.com/user-attachments/assets/ac5c3639-1cf4-4ebe-9963-75509774ead6" />

	
 

# File Permissions and Group Access

## Commands Used

```bash
# Change file permissions
sudo chmod 664 /home/ec2-user/webapp/scripts/log_user.sh

# Verify permissions
ls -l /home/ec2-user/webapp/scripts/log_user.sh

# Switch to user
su - devuser1

# Append text to the script file
echo 'test' >> /home/ec2-user/webapp/scripts/log_user.sh

# View file contents
cat /home/ec2-user/webapp/scripts/log_user.sh

# Check current user
whoami

# Switch to another user
su - devuser3

# View script file
cat /home/ec2-user/webapp/scripts/log_user.sh
```

## Permission Details

```bash
-rw-rw-r-- 1 root writers 207 May 24 05:16 log_user.sh
```

Permission breakdown:

- Owner (`root`) → read, write
- Group (`writers`) → read, write
- Others → read only

:contentReference[oaicite:0]{index=0}

---

## Explanation

### Change File Permissions

```bash
sudo chmod 664 /home/ec2-user/webapp/scripts/log_user.sh
```

Gives:
- write access to owner and group
- read access to everyone

---

### Group-Based Access

`devuser1` belongs to the `writers` group, so the user was able to append:

```bash
echo 'test' >> /home/ec2-user/webapp/scripts/log_user.sh
```

This added:

```text
test
```

to the end of the file.

---

### Verify Current User

```bash
whoami
```

Output:
```text
root
```

and later:

```text
devuser3
```

---

### Read Access for Other Users

`devuser3` was able to read the file because:
- Others have read permission (`r--`)

but `devuser3` cannot modify the file because:
- Others do not have write permission.

<img width="940" height="425" alt="image" src="https://github.com/user-attachments/assets/56a0910e-c77a-4afc-ae45-528560a34d60" />


 
