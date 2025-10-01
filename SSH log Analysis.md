# 🔐 SSH Log Analysis on Splunk

## 📌 Overview
This project analyzes **SSH authentication logs** using Splunk to detect anomalies, brute-force attempts, and suspicious connections.  
The analysis is divided into 5 key tasks:
1. Ingest and parse SSH logs  
2. Analyze failed login attempts  
3. Detect multiple authentication failures (brute-force)  
4. Track successful logins with prior failures  
5. Spot suspicious connections without authentication  


---

## 📝 Tasks

### **Task 1: Ingest and Parse SSH Logs**
1.Verify ingestion and parsing of SSH logs.  
2.Upload ssh_log.json into Splunk.
Ensure the following fields are extracted correctly:
event_type (Successful SSH Login, Failed SSH Login, Multiple Failed Authentication Attempts, Connection Without Authentication)
auth_success (true/false/null)
auth_attempts
id.orig_h (source IP)
id.resp_h (destination host)

```spl
index=ssh_log | stats count by event_type
```
<img width="1904" height="997" alt="Image" src="https://github.com/user-attachments/assets/d95944be-d800-41bc-a6db-ddf00f49cd52" />
to check what are the types of authentication status in that log file

---
### **Task 2: Analyze failed login attempts**
Identify all failed login attempts:
```
index=ssh_logs event_type="Failed SSH Login"
| stats count by id.orig_h
```
<img width="1857" height="946" alt="Image" src="https://github.com/user-attachments/assets/0225bb7d-be25-488f-adbc-0bfc51ac0219" />
Created a bar chart visualization for failed login attempts per source IP.
<img width="1859" height="881" alt="Image" src="https://github.com/user-attachments/assets/019d2145-7f7a-4e51-b99e-41ae0a2fea13" />

---
### **Task 3: Detect multiple authentication failures (brute-force)**
Search for multiple failed attempts in logs:
```
index=ssh_logs event_type="Multiple Failed Authentication Attempts"
| stats count by id.orig_h, id.resp_h
```
the differnce between falied login attempts and multiple authentication is a user may accidentally entered the wrong credentials(FAILED LOGIN ATTEMPTS),but for multiple authentication failure a suspicious ip_addr sent multiple request to gain access within a short period of time
<img width="1886" height="962" alt="Image" src="https://github.com/user-attachments/assets/f782fd4d-c60b-4ae2-88c3-199b61ef3e69" />

Also we have created a alert system so whenever it match those eventtype it will trigger the alert
<img width="1389" height="917" alt="Image" src="https://github.com/user-attachments/assets/8dd2cd68-d602-4154-9037-7e1dc059ff78" />

---
### **Task 4:track successful logins with prior failures**
Search for successful logins:
```
index=ssh_logs event_type="Connection Without Authentication"
| stats count by id.orig_h
```
why we are doing this where any previous suspicious ip_addr got any authentication or any access over the target_ip . as the same time verify the validate user.
<img width="1855" height="897" alt="Image" src="https://github.com/user-attachments/assets/6e99ba3c-567e-498a-8c94-4e5d0adc5e19" />

---
### **Task 5:Spot suspicious connections without authentication**
Search for unauthenticated SSH connections:
```
index=ssh_logs event_type="Connection Without Authentication"
| timechart count by id.orig_h
```
using timechart we know that at what time those events take place.
Identify repeated unauthenticated attempts — potential indicators of port scanning or SSH probing.
<img width="1875" height="871" alt="Image" src="https://github.com/user-attachments/assets/7520f4a4-9d79-42c2-86e4-87e98df0e441" />





