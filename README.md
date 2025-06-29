# 🌐 Portfolio Website on Google Cloud Platform (GCP) [link: http://34.36.146.142/]

This repository contains the deployment of my personal portfolio website using Google Cloud services. It showcases how to serve a static website with logging, analytics, and performance optimization through Cloud CDN.

---

## 🏗️ Architecture Overview

![Static Portfolio Website Architecture on Google Cloud - visual selection](https://github.com/user-attachments/assets/8dcdd191-2858-4652-a63d-07b7c528e18d)


**Services Used:**

1. **Cloud Storage** – Hosts static HTML, CSS, JS files.
   https://storage.googleapis.com/my-portfolio-01/code.html
2. **Cloud CDN** – Caches website content at edge locations for faster delivery.
   ![image](https://github.com/user-attachments/assets/c34e5174-18ec-47b7-b8c1-781813f77f0c)

3. **Load Balancer** – Routes traffic to the Cloud Storage bucket through a backend bucket.
   ![image](https://github.com/user-attachments/assets/e4d92b55-fefd-4701-8897-a8a333e39076)

4. **Google Analytics / Tag Manager** – Tracks visitor data and interactions.
   ![image](https://github.com/user-attachments/assets/e78dbc88-3dc2-496a-b6f5-010aa3be074f)


> Note: Cloud Functions were excluded since no contact form or server-side logic is required.

---

## 🚀 Deployment Steps

![Static Portfolio Website Architecture on Google Cloud - visual selection (1)](https://github.com/user-attachments/assets/f063f342-955e-41ce-a24d-3e20b3f2d1b9)


### 1. Upload Files to Cloud Storage
- Create a bucket (e.g., `my-portfolio-bucket`)
- Set it to **Static Website Hosting**
- Upload your portfolio files
- Set `index.html` as the main page

### 2. Set Up Load Balancer with Cloud CDN
- Go to **Network Services > Load balancing**
- Select **HTTP(S) Load Balancer**
- **Frontend**: Create a new IP + Port 80
- **Backend**: Use "Create a backend bucket" linked to your GCS bucket
- Enable **Cloud CDN** checkbox
- **Routing Rule**: Use simple host and path rule (default)

### 3. Enable Public Access to GCS Bucket
- Go to your bucket → Permissions
- Add `allUsers` with role `Storage Object Viewer`
- Objects in the bucket will now be accessible via HTTP

### 4. Connect Google Analytics / Tag Manager
- Go to [Google Tag Manager](https://tagmanager.google.com/) or [Google Analytics](https://analytics.google.com/)
- Create a Web Stream
- Add the **Global Site Tag** (`<script>...</script>`) into your `index.html` `<head>`
- Confirm analytics by visiting the live site and checking "Active Users"

### 5. Optional: Invalidate Cache After Updates
- Go to **Network Services > Cloud CDN**
- Choose your load balancer
- Enter `/*` in **Path** to purge all cached content

![image](https://github.com/user-attachments/assets/5c768a6e-f9fc-4880-b594-5ee520543617)


## 🧾 Logging Setup

Enable HTTP request logging for load balancer:
- Go to **Backend Bucket Settings**
- Check `Enable Logging`
- Logs are available at **Operations > Logging (Logs Explorer)**
- Use this filter:
```lql
resource.type="http_load_balancer"



