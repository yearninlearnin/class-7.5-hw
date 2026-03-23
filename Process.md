# Staging the VM
- Make sure you are in a custom GCP Project and not the default
- Sometime’s I need the light background, but that is grounds for *getting supplexed 59 times* during a class call, so switch to the dark side (dark mode) for the sake of eye sight.
- In the search bar, type “vm” and VM Instances will pop up
- In Machine Configuration, for a basic VM we will stick to these:
  - Virtual CPU: E2 (e2-medium)
  - OS: Linux (Debian)
  - Data protection: No backups
  - Networking: Allow HTTP traffic is selected
  - Advanced: Import the Startup script provided from SEIR-1 repo
    - [SEIR-1/weekly_lessons/weeka/userscripts/supera.sh at main · BalericaAI/SEIR-1](https://github.com/BalericaAI/SEIR-1/blob/main/weekly_lessons/weeka/userscripts/supera.sh)
  - Create VM: Create button near bottom of the Create an instance page.
___
# Having some fun
- Return to the VM instances page, and we will see any vm we’ve created.
- The details of my vm Name and Zone were picked by me in the previous steps.
- We want to make sure that the bash script worked so we grabbed the External IP by copying it and opening a new tab in our browser.
- Type `http:.//<External IP>` in this case it is `http://34.101.98.17`.
- Webpage should display the SEIR-I-Ops Panel. Styles may very depending html styling.
- We can run some tests, by connecting to the vm instance via SSH.
- GCP has a built-in SSH plugin or you can also SSH via from your machine as long as your `gcloud auth login` is all set up. To SSH into a GCP vm instance:
```bash 
gcloud compute ssh VM_NAME --zone=ZONE
```
- Inside the vm we run these commands:
  - curl localhost
  - curl -s localhost | head
  - systemctl status nginx --no-pager
  - curl -s localhost/healthz
  - curl -s localhost/metadata | jq .
# Gate Script Checks
- We can deploy a script and run it through vscode for gate check validations.
- Using the terminal, we create a directory on our local machine to store our code and open up vscode. for example : ~/dev/theowaf/class7.5/gcp/homework/weeka-a
- Once inside that folder, use `code . ` to open vs code within that directory.
- Create the file `gate_gcp_vm_http_ok.sh` and grab the script `gate_gcp_vm_http_ok.sh` from the SEIR-1 repo.
- Paste the content of the script inside of that newly created file in vscode.
- We can use the terminal within vscode to run the script. First, let’s make note of the External IP of our vm instance. 
- To run the script, double check that we are inside of the same directory where the script is located with command `pwd`: 
- Enter: VM_IP=<External IP> ./gate_gcp_vm_http_ok.sh
- If you get an error output `Permission denied` then make the script executable via:
- `chmod +x gate_gcp_vm_http_ok.sh` command.
- Once the scripts runs with no issue, your output will indicate Lab 1 Gate Result: PASS.
- Your service is reachable, endpoints are healthy, metadata endpoint returns valid JSON.
- badge.txt and gate_result.json will generate inside the directory and vscode as your proof of pass.


# Troubleshooting
## Issue with macOS users
- For macOS users, there is a known runtime error that will occur once you run the script: `gate_gcp_vm_http_ok.sh: line 115: failures[@]: unbound variable`
- ```bash
  Line 115:failures_json=￼$(printf '%s\n' "$￼￼{failures[@]}" | jq -R . | jq -s .)` 
  ```
- To fix the error, add `[@]:-}`


At the beginning of the gate_gcp_vm_http_ok.sh script, the line set -euo pipefail is present. The 'u' component signifies that empty or unbound variables should be regarded as mistakes.
The failures array remains empty as long as all checks are successful. When set -u is enabled, bash on macOS interprets an empty array ${failures[@]} as an unbound variable. The script fails at line 115 due to its attempt to utilize it.

The modification ${failures[@]:-} introduces a default empty value. This instructs bash to utilize nothing in the event that the variable is empty or unset, rather than generating an error.

