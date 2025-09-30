# DNS Log Analysis using Splunk

## Project Overview
This project demonstrates **DNS log analysis** using Splunk. The goal is to monitor and analyze DNS traffic to detect suspicious domains, unusual query patterns, and potential malicious activity.  

**Key Objectives:**  
- Parse and index DNS logs in Splunk  
- Extract relevant fields from raw DNS events  
- Identify top queried domains and abnormal requests  
- Detect suspicious systems querying malicious domains  

---

## Data Ingestion
Go to Splunk Web → **Settings → Add Data → Upload**

1. Select your DNS log file and set **Source Type** (e.g., `dns_sample`)  
2. Assign it to the appropriate index (`dns_logs` or `default`)  
3. Click **Next** → **Submit** to upload the data  

## Splunk Analysis Workflow

### 1. Search for Relevant DNS Events
Some raw events may not contain structured fields. Initially, filter relevant events using regex:

```spl
index=* sourcetype=dns_sample 
| regex _raw="(?i)\b(dns|domain|query|response|port 53)\b"
but the thing is it only show the revelant field in the above command so that's why we gonna create a new field which would be easier to analyse



### 2. Identify Top Queried Domains
Analyze domain frequency to detect suspicious activity:
index=* OR index=_* sourcetype=dns_sample
| stats count by domain


### 3. Investigate Suspicious Systems
To find systems querying a specific malicious domain:
index=* sourcetype=dns_sample fqdn="maliciousdomain.com"
| top limit=10 src_ip
why we are using top comm is beacuse it list out how many system have been made request to this server or else we can use table comm

