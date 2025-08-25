#  CloudLaunch Assignment

##  Project Overview
This project demonstrates:
- Hosting a static website on **Amazon S3** with optional CloudFront distribution.
- Creating IAM user and policies for fine-grained access control.
- Designing a secure VPC with public and private subnets, route tables, internet gateway, and security groups.

---

## Task 1: Static Website Hosting
- **S3 Buckets Created**:
    cloudlaunch-site-bucket-213 → Public static website hosting
    cloudlaunch-private-bucket-213 → Private bucket (no public access)
    cloudlaunch-visible-only-bucket-213 → Private bucket (no object access)

- **Static Website URL**:  
  [http://cloudlaunch-site-bucket-213.s3-website.eu-west-2.amazonaws.com/)  

- **CloudFront URL**:  
  [d3dccw6ya9846n.cloudfront.net)

---

## IAM User & Policies
Created user: **`cloudlaunch-user`**

- **Policy Permissions**:
  - `s3:ListBucket` on all three buckets
  - `s3:GetObject` on site bucket
  - `s3:GetObject` + `s3:PutObject` on private bucket
  - ❌ No access to visible-only bucket objects

Policy JSON: [policies/cloudlaunch-s3-policy.json](policies/cloudlaunch-s3-policy.json)  

---

## VPC Design
- **VPC Name**: `cloudlaunch-vpc`  
- **CIDR**: `10.0.0.0/16`  

### Subnets
- **Public Subnet** → `10.0.1.0/24`
- **App Subnet** → `10.0.2.0/24`
- **DB Subnet** → `10.0.3.0/28`

### Route Tables
- **Public RT** (cloudlaunch-public-rt) → 0.0.0.0/0 → IGW  
- **App RT** (cloudlaunch-app-rt) → Local only (private)  
- **DB RT** (cloudlaunch-db-rt) → Local only (private)  

### Security Groups
- `cloudlaunch-app-sg` → Allow HTTP (80) from 10.0.0.0/16  
- `cloudlaunch-db-sg` → Allow MySQL (3306) only from App Subnet  

### IAM Policy for Read-Only VPC Access
Policy JSON: [policies/cloudlaunch-vpc-readonly.json](policies/cloudlaunch-vpc-readonly.json)

---


