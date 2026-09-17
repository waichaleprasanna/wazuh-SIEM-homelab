# Wazuh SIEM Homelab

A self-built SIEM lab using Wazuh for log analysis, detection engineering, and incident response practice, built as part of my transition into a SOC L1 analyst role.

## Architecture
- Wazuh manager, indexer, and dashboard: installed on a Kali Linux VM (VirtualBox, NAT networking), all-in-one via the official install script
- Monitored endpoint: Windows host, enrolled as an agent via the Wazuh dashboard's deploy wizard, with NAT port forwarding (1514/1515) and manual manager-address configuration on the agent side

## What's in this repo
- `docs/` — full setup walkthrough, including install commands, NAT configuration, and agent enrollment
- `screenshots/` — dashboard and alert screenshots
- `use-cases/` — detection scenarios tested and how Wazuh responded
- `detection-rules/` — custom detection rules (in progress)

## Key takeaways
- Hands-on experience deploying a full SIEM stack from scratch, including manager-agent network configuration across a VM boundary
- Practical troubleshooting of agent enrollment and manager-address configuration via ossec.conf
- Building toward custom detection engineering and documented use cases

## See also
- [Lab setup details](docs/setup.md)
