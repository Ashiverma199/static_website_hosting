# 🌐 Static Website Hosting on AWS

This project demonstrates how to host a **static website** on AWS using **S3, CloudFront, Route 53, ACM, and WAF**.  
The site is live with a scalable, secure, and production-ready cloud architecture.

---

## 📌 Architecture

**User → Route 53 (DNS) → CloudFront (CDN + HTTPS + WAF) → S3 Bucket (Static Website Files)**

---

## 🚀 Steps to Deploy

### Step 1: Prepare Website Content
- Create your static website using **HTML, CSS, JavaScript**.  
- Ensure you have an `index.html` file (main page).  
- Optional: Add `error.html` for 404 pages.  

---

### Step 2: Create an S3 Bucket
1. Go to **AWS Console → S3 → Create bucket**.  
2. Bucket name: `staticwebsiteashi`.  
3. Region: `us-east-1`.  
4. Keep **Block all public access ON** (we will use CloudFront for access).  
5. Create the bucket.  

---

### Step 3: Upload Website Files
1. Open the bucket → **Upload** → add all files (`index.html`, `style.css`, `script.js`).  
2. Go to **Properties → Static website hosting**.  
   - Index document: `index.html`  
   - Error document: `error.html`  

---

### Step 4: Set Up CloudFront
1. Go to **CloudFront → Create distribution**.  
2. **Origin domain**: Choose your S3 bucket (`staticwebsiteashi.s3.amazonaws.com`).  
3. **Default root object**: `index.html`.  
4. Create the distribution.  

---

### Step 5: Configure Origin Access Control (OAC)
1. Create a new OAC in CloudFront.  
2. Copy the generated bucket policy.  
3. Go to **S3 → Permissions → Bucket Policy** and paste the following:  

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::staticwebsiteashi/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::838220667806:distribution/E2PLQGCE2MA1XJ"
        }
      }
    }
  ]
}
Step 6: Register Domain in Route 53

Go to Route 53 → Register Domain.

Example: ashiverma.me.

If using external provider (GoDaddy, Namecheap), point DNS to Route 53 hosted zone.

Step 7: Provision SSL Certificate (ACM)

Open AWS Certificate Manager (ACM).

Request a certificate for:

yourdomain.com

*.yourdomain.com

Validate via DNS validation (CNAME record in Route 53).

Wait until status = Issued.

Step 8: Configure WAF Protection

Go to AWS WAF → Create Web ACL.

Associate it with your CloudFront distribution.

Add managed rules for:

SQL Injection

Cross-Site Scripting (XSS)

Bad Bot Protection

Step 9: Set Up DNS Records

Go to Route 53 → Hosted Zones → yourdomain.com.

Create a new record:

Type: A – Alias

Alias Target: CloudFront distribution domain (e.g., d123abc.cloudfront.net).

Save.

Now your website is live on https://yourdomain.com
 🎉

🛠 AWS Services Used

Amazon S3 → Static Website Hosting

Amazon CloudFront → CDN for performance & HTTPS

Amazon Route 53 → DNS & Domain Management

AWS Certificate Manager (ACM) → SSL/TLS Certificate

AWS WAF → Web Application Firewall

📖 References

Hosting a static website on Amazon S3

CloudFront + S3 setup

✅ Author

Ashi Verma