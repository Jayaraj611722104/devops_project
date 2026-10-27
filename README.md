# AWS Student Management System – DevOps Project

## Project Overview

This project demonstrates how to deploy a Student Management System web page to an AWS EC2 Linux instance using Git, GitHub, and Nginx. The current application is a static website served from `index.html`.

## Objectives

- Store and version-control the website source code with Git and GitHub.
- Deploy the website on an AWS EC2 instance.
- Serve the page using Nginx.
- Use AWS IAM to manage permissions.
- Use an Amazon VPC, subnet, route table, Internet Gateway, and security group to control network access.
- Extend the project with Amazon CloudWatch monitoring and Amazon SNS email alerts.
- Store website files and backups in Amazon S3 where required.

## Technology Stack

- **Source control:** Git and GitHub
- **Cloud platform:** Amazon Web Services (AWS)
- **Compute:** Amazon EC2
- **Web server:** Nginx
- **Object storage:** Amazon S3
- **Identity and access management:** AWS IAM
- **Networking:** Amazon VPC
- **Monitoring (planned):** Amazon CloudWatch
- **Notifications (planned):** Amazon SNS
- **Website:** HTML (`index.html`)

## Architecture

```text
Developer
   |
   | git push
   v
GitHub Repository
   |
   | git clone / deployment
   v
AWS VPC
   |
   v
EC2 Linux Instance
   |
   v
Nginx Web Server
   |
   v
Student Management System (index.html)

Supporting AWS services:
- IAM: permissions and roles
- S3: object storage and backups
- CloudWatch: metrics, logs and alarms (planned)
- SNS: email notifications from alarms (planned)
```

## Prerequisites

- An AWS account.
- An EC2 instance running Amazon Linux.
- A security group that allows HTTP traffic on port `80` for website testing.
- SSH access restricted to your own IP address.
- Git installed on your development machine.
- An Nginx web server on EC2.

## Deployment Steps

### 1. Push the source code to GitHub

Run these commands from the project folder on your development machine:

```bash
git add .
git commit -m "Add Student Management System website"
git branch -M main
git push -u origin main
```

If you have not configured the remote repository yet, add it:

```bash
git remote add origin https://github.com/Jayaraj611722104/devops_project.git
```

If `origin` already exists, do not add it again. Check it with:

```bash
git remote -v
```

### 2. Connect to the EC2 instance

Connect using SSH or EC2 Instance Connect.

### 3. Install Git and Nginx

For Amazon Linux 2023:

```bash
sudo dnf install git nginx -y
```

For Amazon Linux 2, package installation commands may differ; use the package manager supported by that operating-system version.

### 4. Clone the repository

```bash
git clone https://github.com/Jayaraj611722104/devops_project.git
cd devops_project
```

### 5. Deploy the HTML file to Nginx

```bash
sudo cp index.html /usr/share/nginx/html/index.html
```

### 6. Start Nginx and enable it at boot

```bash
sudo systemctl enable --now nginx
```

Check the service:

```bash
sudo systemctl status nginx
```

### 7. Test the website

Test from the EC2 instance:

```bash
curl -I http://localhost
```

A response such as `HTTP/1.1 200 OK` indicates that Nginx is serving a page successfully.

To access it externally, open:

```text
http://<EC2-PUBLIC-IP>
```

Replace `<EC2-PUBLIC-IP>` with the current public IPv4 address of your instance.

## AWS Configuration Notes

### Security Group

For a basic test deployment:

- **HTTP, TCP port 80:** allow from `0.0.0.0/0` if the website is intended to be public.
- **SSH, TCP port 22:** restrict the source to your own IP address.
- **HTTPS, TCP port 443:** add when HTTPS is configured.

### IAM and S3

Prefer an IAM role attached to EC2 rather than storing long-term AWS access keys on the server. Grant only the permissions needed for the project bucket. The S3 bucket can remain private when access is provided through IAM.

### VPC

The EC2 instance must be in the intended VPC and subnet. For direct public access, the instance needs a public IPv4 address, a route to an Internet Gateway, and suitable security-group rules.

## Current Status

### Verified

- The GitHub repository is used to store the website source.
- The `index.html` file has been deployed to the Nginx web directory.
- Nginx returned `HTTP/1.1 200 OK` when tested locally with `curl -I http://localhost`.

### Remaining / Optional Enhancements

- Configure HTTPS with a valid TLS certificate and a suitable domain name.
- Complete and test least-privilege IAM permissions for S3.
- Confirm the final VPC, subnet, route table, Internet Gateway, and security-group configuration.
- Configure CloudWatch alarms and dashboards.
- Configure an SNS email subscription and test notifications.
- Add a backend and database if the application needs persistent student records or create, read, update, and delete (CRUD) functionality.
- Automate deployment with GitHub Actions or another CI/CD tool.

## Important Security Notes

- Do not commit AWS access keys, private keys, passwords, or other secrets to GitHub.
- Do not store real student personal information in this static demo.
- The website uses the browser's Web Crypto API (`crypto.subtle`) for password hashing. Modern browsers require a secure context, such as HTTPS, for this feature on a public website. Configure HTTPS rather than removing or bypassing password hashing.
- Use least-privilege IAM permissions and keep SSH access restricted.

## Repository

GitHub: https://github.com/Jayaraj611722104/devops_project

## License

Add a license file if you intend to publish or redistribute this project. Until a license is added, do not assume that the repository grants others permission to reuse the code.
