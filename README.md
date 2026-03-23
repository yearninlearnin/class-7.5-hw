# Class 7.5: Google Cloud Platform (GCP)

## Overview
This repository contains a comprehensive collection of coursework, homework assignments, and hands-on lab documentation for Class 7.5, focusing on Google Cloud Platform (GCP). 

## Purpose
The primary goal of this repository is to track my progression in mastering GCP core services, infrastructure provisioning, and cloud operations. It serves as a practical portfolio of applied cloud engineering concepts, moving from foundational deployments to automated, enterprise-ready architectures.

## Core Technologies Explored
*(Note: This will be updated as the class progresses)*
* **Compute:** GCP Compute Engine (VMs)
* **Networking:** Virtual Private Cloud (VPC), Load Balancing
* **Security & Identity:** Cloud IAM
* **Operations & Validation:** `gcloud` CLI, Bash Scripting, Cloud Monitoring

## Repository Structure
Each homework assignment and lab is organized into its own dedicated directory. Inside each folder, you will find the necessary deployment scripts, configuration files, and a detailed `README.md` outlining the architectural walkthrough, expected deliverables, and automated verification evidence.
---
### [Week A — GCP VM Deployment and Automated HTTP Validation](https://github.com/yearninlearnin/class-7.5-hw/tree/week-a-hw)
**Overview:**

- Deploy an Nginx web server on a GCP VM with custom `/healthz` and `/metadata` endpoints, validated by an automated gate script.

- **Tools:** GCP Compute Engine, Nginx, gcloud CLI, bash, curl, jq

**Deliverables:**
- Active GCP VM serving Nginx over public IP
- Functional `/healthz` and `/metadata` endpoints
- Automated gate script passing with `badge.txt` and `gate_result.json`
---