# Zeek Connection Log Analysis

---

## 🎯 Objective

- Learn how to upload and search Zeek connection logs in Splunk.
- Find top clients, top servers, and most common services.
- Identify large traffic and long connections.

---

## ⚙️ Steps to Upload Conn Log into Splunk

1. Go to **Splunk Web → Settings > Add Data**.
2. Choose **Upload** and select `zeek_conn_logs.json`.
3. Set **Source type**: `json` or create a new one like `zeek:conn`.
4. Index: Use `main` or create a new one called `conn_lab`.
5. Complete the upload and check that logs are searchable.

---

### ✅Task 1: Find the Top 10 Client IPs
we make sure from where the connection established so that's why we are using this command

```spl
index=conn_lab sourcetype="json"
| stats count by id.orig_h
| sort -count
| head 10
```
<img width="1882" height="862" alt="Image" src="https://github.com/user-attachments/assets/33014e0a-dc1b-41d2-bcfe-eb054c4fad58" />

---

### ✅Task 2: List Most Common Services
In this we check the most common services used 

```spl
index=conn_lab sourcetype="json"
| stats count by service
| sort -count
```
<img width="1875" height="876" alt="Image" src="https://github.com/user-attachments/assets/4dad3255-680a-4143-b02d-f6d9e3462fd9" />

---

### ✅Task 3: Find Connections with Duration > 1 Second

```spl
index=conn_lab sourcetype="json" duration>1
| table ts id.orig_h id.resp_h service duration
| sort -duration
```
<img width="1871" height="906" alt="Image" src="https://github.com/user-attachments/assets/6d329852-9e8b-4e35-9053-4e325241ebfe" />
---

### ✅Task 4: Identify the Most Accessed Internal Servers
what are the all the servers have been accesed we check by using this command
```spl
index=conn_lab sourcetype="json"
| stats count by "id.resp_h"
| sort -count
| head 10
```
<img width="1869" height="980" alt="Image" src="https://github.com/user-attachments/assets/59b8d3fe-e924-4620-8eed-853bdf6ab686" />
