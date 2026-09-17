# Use Case: File Integrity Monitoring (FIM) Test

## What I did
Created and deleted test files in a monitored directory on the Windows agent (`wazuh_test` folder) to verify Wazuh's File Integrity Monitoring was actively tracking filesystem changes.

## What Wazuh detected
Wazuh's FIM module logged each change in near real-time:

| Event | File | Rule ID | Rule Level | Description |
|-------|------|---------|------------|--------------|
| Added | test_.txt | 554 | 5 | File added to the system |
| Deleted | new text document.txt | 553 | 7 | File deleted |
| Added | new text document.txt | 554 | 5 | File added to the system |

![FIM events](../screenshots/fim-events.png)

## What I learned
- Wazuh's `syscheck` module tracks file creation, modification, and deletion in configured directories
- Deletions are scored at a higher rule level (7) than additions (5), reflecting their relative risk
- FIM events include full metadata — file path, user, timestamp — useful for building a timeline during incident investigation
