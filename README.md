# **Vercel-Clone — Deployment Platform (Full System Design)**
**Built by Sunilkumar T**  
*Backend Engineering • DevOps • Distributed Systems • PSG Tech*

---

---<img width="10772" height="10272" alt="Vercel-clone" src="https://github.com/user-attachments/assets/069d8498-e58d-4adc-9383-f45a2cbcb605" />
## **Overview**
A fully functional deployment platform inspired by Vercel.

This system can:
- Clone repositories  
- Build projects inside isolated Docker containers  
- Stream logs in real-time  
- Upload artifacts to S3  
- Serve deployments dynamically through Nginx  
- Route requests based on project/build/domain  

This README documents the **full architecture**, **flow**, and **engineering behind the platform**.

---

---

## **System Components**

### **1. Route Handler**
Handles all incoming requests:
- Maps domain/subdomain → project  
- Computes correct build path  
- Fetches files from S3  
- Proxies static assets  
- Returns HTML/CSS/JS

---

### **2. Deploy Service**
Responsible for:
- Accepting deployment requests  
- Cloning GitHub repos  
- Preparing build job metadata  
- Pushing build jobs to Redis  

Used by:
- Web dashboard  
- CLI (future)  
- API triggers

---

### **3. Build Worker**
A Docker-isolated worker that:
- Pulls jobs from Redis queue  
- Spawns a fresh Docker container  
- Installs dependencies  
- Executes build commands  
- Streams logs  
- Uploads final artifacts to S3  

Output stored at:

```
s3://bucket/main/<projectId>/<buildId>/
```

---

### **4. Upload Service**
