### Staging the VM

First, make sure you are in a custom GCP Project and not the default one. 

Sometimes I need the light background, but that is grounds for getting suplexed 59 times during a class call. Do yourself a favor and switch to the dark side (dark mode) for the sake of your eyesight.

1. In the top search bar, type **"vm"** and select **VM Instances**.
2. Click the **Create Instance** button near the bottom of the page.
3. In the Machine Configuration section, we will stick to these settings for a basic VM:
   * **Virtual CPU:** E2 (`e2-medium`)
   * **OS:** Linux (Debian)
   * **Data protection:** No backups
   * **Networking:** Check "Allow HTTP traffic"
   * **Advanced:** Import the startup script provided from the SEIR-1 repo: [supera.sh](https://github.com/BalericaAI/SEIR-1/blob/main/weekly_lessons/weeka/userscripts/supera.sh)

### Having Some Fun

Return to the **VM instances** page, where you will now see the VM you just created (with the Name and Zone you picked in the previous steps).

Let's make sure the bash script actually worked:
1. Copy the **External IP** of your VM.
2. Open a new tab in your browser and go to `http://<External IP>` (for example: `http://34.101.98.17`).
3. The webpage should display the SEIR-I-Ops Panel. *(Note: Styles may vary depending on HTML styling).*

Now, let's run some tests by connecting to the VM instance via SSH. GCP has a built-in SSH plugin in the console, or you can SSH directly from your local terminal as long as you've run `gcloud auth login`. 

To SSH into your GCP VM instance from your terminal:
```bash
gcloud compute ssh VM_NAME --zone=ZONE
```

Once inside the VM, run these commands to verify everything is running smoothly:
```bash
curl localhost
curl -s localhost | head
systemctl status nginx --no-pager
curl -s localhost/healthz
curl -s localhost/metadata | jq .
```

### Gate Script Checks

Next, we are going to deploy a script and run it through VS Code for gate check validations.

1. Using your local terminal, create a directory to store your code and navigate into it. For example: 
   ```bash
   mkdir -p ~/dev/theowaf/class7.5/gcp/homework/weeka-a
   cd ~/dev/theowaf/class7.5/gcp/homework/weeka-a
   ```
2. Open VS Code in that directory by running: `code .`
3. Create a new file called `gate_gcp_vm_http_ok.sh` and paste in the script contents from the SEIR-1 repo.
4. Open the integrated terminal in VS Code and double-check you are in the correct directory using the `pwd` command. 

Before running the script, make it executable so you don't get a `Permission denied` error:
```bash
chmod +x gate_gcp_vm_http_ok.sh
```

Now, run the script, making sure to pass in your VM's External IP:
```bash
VM_IP=<Your_External_IP> ./gate_gcp_vm_http_ok.sh
```

If the script runs with no issues, your output will indicate: `Lab 1 Gate Result: PASS`. This proves your service is reachable, your endpoints are healthy, and your metadata endpoint returns valid JSON.

As proof of your pass, a `badge.txt` and `gate_result.json` file will automatically generate inside your directory.

---

### Troubleshooting

#### Known Issue for macOS Users

For macOS users, you might run into this runtime error when executing the script: 
`gate_gcp_vm_http_ok.sh: line 115: failures[@]: unbound variable`

**Why this happens:**
At the beginning of the script, there is a line that says `set -euo pipefail`. The `u` component tells bash to treat empty or unbound variables as errors. If all your checks pass, the `failures` array remains empty. On macOS, bash interprets that empty `${failures[@]}` array as an unbound variable, causing the script to fail at Line 115:
```bash
failures_json=$(printf '%s\n' "${failures[@]}" | jq -R . | jq -s .)
```

**The Fix:**
You need to add `:-` to the array call to introduce a default empty value. This instructs bash to use "nothing" if the variable is empty instead of throwing an error.

Change the variable in Line 115 to look like this:
```bash
"${failures[@]:-}"
```