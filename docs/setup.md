# Lab Setup

## Architecture
- Wazuh manager, indexer, and dashboard: installed on a Kali Linux VM (VirtualBox, NAT networking)
- Monitored endpoint: Windows host (PRASANNA-PC replaced with generic name for public repo), enrolled as an agent via port-forwarded enrollment (ports 1514/1515)

## Install steps

### 1. Kali VM setup
- Created a Kali Linux VM in VirtualBox with NAT networking

### 2. Wazuh install (manager, indexer, dashboard)
Installed all-in-one using the official Wazuh install script:

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

This installs the Wazuh manager, indexer, and dashboard together on a single node.

### 3. NAT port forwarding
Configured two NAT port forwarding rules in VirtualBox (VM Settings → Network → Advanced → Port Forwarding):

| Rule Name           | Protocol | Host Port | Guest Port |
|----------------------|----------|-----------|------------|
| Wazuh-Agent-Enroll   | TCP      | 1515      | 1515       |
| Wazuh-Agent-Comm     | TCP      | 1514      | 1514       |
| Wazuh-Dashboard      | TCP      | 9443      | 443        |

Host IP and Guest IP left blank so the rules apply across interfaces. 
This let the Windows host reach the Kali VM's Wazuh manager on the standard agent enrollment (1515) and communication (1514) ports.

### 4. Accessing the dashboard
Retrieved the Kali VM's IP address:

```bash
ip -4 addr show
```
Accessed the Wazuh dashboard at `https://<kali-vm-ip>` from the browser.

### 5. Agent enrollment (Windows host)
Used the Wazuh dashboard's "Deploy new agent" wizard:
1. Selected Windows as the target OS
2. Entered the Kali VM's manager IP (`<kali-vm-ip>`)
3. The wizard generated a PowerShell one-liner to download and register the agent
4. Ran the generated command in PowerShell on the Windows host, which installed the agent and enrolled it against the manager

### 6. Manually configuring the agent's manager address
Opened the agent config file directly to set the manager IP:

```powershell
notepad "C:\Program Files (x86)\ossec-agent\ossec.conf"
```

Edited the `<address>` field to point to the Kali VM's IP (`<kali-vm-ip>`), then restarted the Wazuh agent service for the change to take effect.

### 7. Verifying the configuration
Confirmed the change was applied correctly:

```powershell
Get-Content "C:\Program Files (x86)\ossec-agent\ossec.conf" | Select-String "address"
```
