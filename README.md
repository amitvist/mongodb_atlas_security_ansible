# MongoDB Atlas Security Provisioning (AAP - Fully Explained)

## Overview
This project automates MongoDB Atlas security provisioning directly inside Ansible Automation Platform (AAP).
It retrieves the Azure Entra group, assigns MongoDB roles, generates a CSV report, and emails the summary.

All credentials (MongoDB API keys, SMTP credentials, etc.) are securely fetched from AAP environment variables.
No manual variable file editing is needed.

---

## Folder Structure
```
create_mongodb_atlas_security.yml       # Main entry point playbook
inventory.yml                           # Localhost inventory for AAP
roles/
 └── provision_security/
     └── tasks/
         └── main.yml                   # Core logic for role assignment
```

---

## Required Environment Variables in AAP
| Variable | Description |
|-----------|-------------|
| `ATLAS_PUBLIC_KEY` | MongoDB Atlas API Public Key |
| `ATLAS_PRIVATE_KEY` | MongoDB Atlas API Private Key |
| `ORG_ID` | MongoDB Atlas Organization ID |
| `ENTRA_GROUP_NAME` | Azure Entra (AD) Group Name |
| `SMTP_HOST` | SMTP server hostname (e.g., smtp.office365.com) |
| `SMTP_PORT` | SMTP port (usually 587) |
| `SMTP_USER` | SMTP sender email address |
| `SMTP_PASS` | SMTP app password or key |
| `NOTIFICATION_EMAIL` | Recipient email for status updates |

---

## Steps to Use in Ansible Automation Platform (AAP)
1. Import this folder as a Project in AAP.
2. Add the included `inventory.yml`.
3. Create a Job Template using:
   - **Playbook:** `create_mongodb_atlas_security.yml`
   - **Inventory:** Localhost
   - **Execution Environment:** Default EE or custom
4. Launch the Job. It will:
   - Authenticate using stored environment variables
   - Assign MongoDB Atlas roles to Entra group
   - Generate `/tmp/role_mappings.csv`
   - Email the summary with attached CSV

---

## Security Notes
- No secrets are hardcoded — everything is pulled securely from environment variables.
- API and SMTP credentials are masked from logs (`no_log: true`).
- Base64 encoding ensures MongoDB Atlas API authentication works correctly.
