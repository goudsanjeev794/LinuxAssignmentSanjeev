# LinuxAssignmentSanjeev

Please note my WSL does not work on my system, so I have used killercoda.com.

Question 1: Set Up Your DevOps Project Structure
Commands and output with screenshots:
root@ubuntu:~$ mkdir -p /home/ec2-user/webapp/scripts /home/ec2-user/webapp/logs /home/ec2-user/webapp/config
root@ubuntu:~$ cat > /home/ec2-user/webapp/config/app.conf
APP_NAME=WebApp
PORT=8080
root@ubuntu:~$ touch /home/ec2-user/webapp/logs/app.log
root@ubuntu:~$ chmod 755 /home/ec2-user/webapp/scripts
root@ubuntu:~$ chmod 644 /home/ec2-user/webapp/config/app.conf
root@ubuntu:~$ sudo chown -R root:root /home/ec2-user/webapp/
root@ubuntu:~$  ls -lR /home/ec2-user/webapp/
/home/ec2-user/webapp/:
total 12
drwxr-xr-x 2 root root 4096 May 21 12:04 config
drwxr-xr-x 2 root root 4096 May 21 12:04 logs
drwxr-xr-x 2 root root 4096 May 21 12:02 scripts

/home/ec2-user/webapp/config:
total 4
-rw-r--r-- 1 root root 26 May 21 12:04 app.conf

/home/ec2-user/webapp/logs:
total 0
-rw-r--r-- 1 root root 0 May 21 12:04 app.log

/home/ec2-user/webapp/scripts:
total 0
root@ubuntu:~$ cat ^C
root@ubuntu:~$ cat /home/ec2-user/webapp/config/app.conf
APP_NAME=WebApp
PORT=8080
root@ubuntu:~$

<img width="1036" height="453" alt="image" src="https://github.com/user-attachments/assets/325f2be1-fc60-4cc7-9949-33311f64eae5" />


 

Question 2: Write an Interactive Log Script

root@ubuntu:~$  cd /home/ec2-user/webapp/scripts/
root@ubuntu:/home/ec2-user/webapp/scripts$ vim log_user.sh 
root@ubuntu:/home/ec2-user/webapp/scripts$ cat log_user.sh
#!/bin/bash
read -p "Enter your name: " username
cat /home/ec2-user/webapp/config/app.conf
echo "Login: $username Date: $(date)" >> /home/ec2-user/webapp/logs/app.log
cat /home/ec2-user/webapp/logs/app.log

root@ubuntu:/home/ec2-user/webapp/scripts$ chmod +x log_user.sh
root@ubuntu:/home/ec2-user/webapp/scripts$ ls -ltr log_user.sh
-rwxr-xr-x 1 root root 207 May 21 12:28 log_user.sh
root@ubuntu:/home/ec2-user/webapp/scripts$ ./log_user.sh
Enter your name: Sanjeev
APP_NAME=WebApp
PORT=8080
Login: Sanjeev Date: Thu May 21 12:29:58 UTC 2026
root@ubuntu:/home/ec2-user/webapp/scripts$ ./log_user.sh
Enter your name: Kumar
APP_NAME=WebApp
PORT=8080
Login: Sanjeev Date: Thu May 21 12:29:58 UTC 2026
Login: Kumar Date: Thu May 21 12:30:07 UTC 2026
root@ubuntu:/home/ec2-user/webapp/scripts$ ./log_user.sh
Enter your name: Goud
APP_NAME=WebApp
PORT=8080
Login: Sanjeev Date: Thu May 21 12:29:58 UTC 2026
Login: Kumar Date: Thu May 21 12:30:07 UTC 2026
Login: Goud Date: Thu May 21 12:30:18 UTC 2026
root@ubuntu:/home/ec2-user/webapp/scripts$

 <img width="940" height="389" alt="image" src="https://github.com/user-attachments/assets/20de635c-877c-46a7-8ddf-4afc4078ac9b" />


Question 3: User Management and File Permission Control


root@ubuntu:/home/ec2-user/webapp/scripts$  sudo groupadd writers
root@ubuntu:/home/ec2-user/webapp/scripts$ sudo useradd -m devuser1
root@ubuntu:/home/ec2-user/webapp/scripts$ sudo useradd -m devuser2
root@ubuntu:/home/ec2-user/webapp/scripts$ sudo useradd -m devuser3
root@ubuntu:/home/ec2-user/webapp/scripts$ sudo useradd -m devuser4
root@ubuntu:/home/ec2-user/webapp/scripts$ sudo usermod -aG writers devuser1   &&   sudo usermod -aG writers devuser2
root@ubuntu	$ sudo chown root:writers /home/ec2-user/webapp/scripts/log_user.sh

<img width="940" height="154" alt="image" src="https://github.com/user-attachments/assets/ac5c3639-1cf4-4ebe-9963-75509774ead6" />

	
 

root@ubuntu:~$ sudo chmod 664 /home/ec2-user/webapp/scripts/log_user.sh
root@ubuntu:~$  ls -l /home/ec2-user/webapp/scripts/log_user.sh
-rw-rw-r-- 1 root writers 207 May 24 05:16 /home/ec2-user/webapp/scripts/log_user.sh
root@ubuntu:~$ su - devuser1  then  echo 'test' >> /home/ec2-user/webapp/scripts/log_user.sh
root@ubuntu:~$ su - devuser1
$ ^C
$ echo 'test' >> /home/ec2-user/webapp/scripts/log_user.sh
$ 
root@ubuntu:~$ cat ^[[200~/home/ec2-user/webapp/scripts/log_user.sh~
cat: ''$'\033''[200~/home/ec2-user/webapp/scripts/log_user.sh~': No such file or directory
root@ubuntu:~$ vi /home/ec2-user/webapp/scripts/log_user.sh
root@ubuntu:~$ cat /home/ec2-user/webapp/scripts/log_user.sh
#!/bin/bash
read -p "Enter your name: " username
cat /home/ec2-user/webapp/config/app.conf
echo "Login: $username Date: $(date)" >> /home/ec2-user/webapp/logs/app.log
cat /home/ec2-user/webapp/logs/app.log

test
root@ubuntu:~$ whoami
root
root@ubuntu:~$ su - devuser3
$ whoami
devuser3
$ cat /home/ec2-user/webapp/scripts/log_user.sh
#!/bin/bash
read -p "Enter your name: " username
cat /home/ec2-user/webapp/config/app.conf
echo "Login: $username Date: $(date)" >> /home/ec2-user/webapp/logs/app.log
cat /home/ec2-user/webapp/logs/app.log

test
$ 
root@ubuntu:~$

<img width="940" height="425" alt="image" src="https://github.com/user-attachments/assets/56a0910e-c77a-4afc-ae45-528560a34d60" />


 
