# SEIR-I Lab Week A: GCP VM Deployment and Automated HTTP Validation

## Overview
This project demonstrates a foundational cloud infrastructure pattern: provisioning a virtual machine in Google Cloud Platform (GCP) to serve a web application, and rigorously validating its health and metadata endpoints using automated bash scripts.

## What It Does
A web server (Nginx) is deployed onto a GCP Virtual Machine (VM) to serve a custom homepage. Beyond simply serving static content, the application is configured to expose specific `/healthz` and `/metadata` endpoints to provide operational visibility. 

To ensure production readiness, the deployment is validated using an automated "Gate Script." Instead of relying purely on manual browser checks, the script automatically verifies that the service is publicly reachable, the health endpoint returns a valid status, and the metadata endpoint returns properly formatted JSON detailing the instance name and region.

## Core Cloud Services & Tools Used
* **Compute:** GCP Compute Engine (VMs)
* **Web Server:** Nginx
* **Operations & Validation:** `gcloud` CLI, bash scripting, `curl`, `jq`

## Walkthrough
1. Provision a GCP VM and configure Nginx to serve a custom homepage over HTTP.
2. Validate the web server locally by SSHing into the VM and running `curl localhost` and `systemctl status nginx`.
3. Execute the automated gate script (`gate_gcp_vm_http_ok.sh`) against the instance's external IP address to test the `/healthz` and `/metadata` endpoints.
4. (Optional) Configure the HTML `<head>` tag to automatically refresh the page every 10 seconds.


## Deliverables
* Active GCP Virtual Machine serving an Nginx web application.
* Reachable homepage accessible via a public IP address.
* Functional `/healthz` and `/metadata` endpoints returning valid operational data.
* Automated gate script execution yielding a "PASS" result.
* Generated verification artifacts, including a `badge.txt` and a `gate_result.json` file proving the infrastructure meets operational standards.