# Microsoft Azure Fundamentals and Forensics

## 1. Introduction to Microsoft Azure

- Microsoft Azure is a cloud computing platform offering a wide range of cloud services: computing, analytics, storage, networking, databases, AI, IoT, security, etc.
- Managed via the Azure Portal (web-based console), PowerShell, or the Azure CLI
- Services are categorized in the portal for easy access

## 2. Division of Responsibilities in Azure

Responsibilities vary based on the cloud service model (IaaS, PaaS, SaaS, on-premise). Shared responsibility model between customer and Microsoft:

**Customer responsible for:**
- Information and data
- Devices (mobile/PCs)
- Accounts and identities
- Applications
- Network controls (in some models)

**Microsoft responsible for:**
- Identity and directory infrastructure
- Operating system (in PaaS/SaaS)
- Physical hosts, network, and datacenter

## 3. Data Storage in Azure

- When deploying a service, customers choose a region for data storage
- Data can be stored in multiple availability zones within a region for high availability
- **Geo**: a pair of regions within the same geography (country or group of countries)
- **Availability Zones**: physically separate datacenters within a region, each with independent power, cooling, and networking
- **Non-regional services**: some services (e.g. Microsoft Entra ID) are global and don't allow region selection

**Azure data storage services:**

1. **Azure Blobs** — unstructured data (documents, media, VM disks); types: Block Blobs, Page Blobs (VHDs), Append Blobs (logs)
2. **Azure Files** — managed file shares (SMB/NFS)
3. **Azure Elastic SAN** (preview) — large-scale SAN storage
4. **Azure Queues** — message storage for asynchronous processing
5. **Azure Tables** — NoSQL structured data storage
6. **Azure Disks** — managed disks for VMs (OS Disk, Data Disk, Temporary Disk)
7. **Azure Container Storage** (preview) — Kubernetes-integrated storage
8. **Azure NetApp Files** — enterprise file storage (NFS/SMB)

**Data redundancy options:**

| Option | Description |
|---|---|
| LRS (Locally Redundant Storage) | 3 copies in one datacenter |
| ZRS (Zone-Redundant Storage) | 3 copies across availability zones |
| GRS (Geo-Redundant Storage) | LRS plus an asynchronous copy to a secondary region |
| GZRS (Geo-Zone-Redundant Storage) | ZRS plus an asynchronous copy to a secondary region |

## 4. Logs in Azure

Azure provides multiple logging mechanisms for security, auditing, and forensics.

1. **Azure Activity Logs** — records write operations (POST, UPDATE, PUT, DELETE) at the subscription level; shows who did what, when, and status
2. **Azure Resource Logs** — records operations inside a resource (data plane); not collected by default, must enable diagnostic settings; can be sent to Log Analytics, Storage, or Event Hubs
3. **Microsoft Entra ID Reports**:
   - Security reports: users flagged for risk, risky sign-ins
   - Activity reports: sign-in logs (user, time, status, IP, location)
4. **Network Security Group (NSG) Flow Logs** — captures inbound/outbound traffic through NSGs; stored in JSON format, retained up to 1 year
5. **VM Log Data** — collected via Azure Monitor and the Log Analytics VM extension; Windows Event Logs, Linux Syslog, performance data
6. **Azure Storage Analytics Logs** — logs all requests (success/fail) to storage services (Blobs, Queues, Tables); stored in the `$logs` container as block blobs

## 5. Performing Microsoft Azure Forensics

- Forensics in Azure involves identifying, preserving, analyzing, and presenting digital evidence from Azure resources
- Key evidence sources: logs, VM snapshots, storage analytics, network flow logs

### Forensic Acquisition of Azure VMs

**Scenario:**
- Two resource groups: `Production-group` and `Security-group`
- Suspect VM: `azure-ubuntu` in `Production-group`

**Steps:**
1. Stop the VM
2. Create a snapshot of the OS disk (via Azure Portal or Azure CLI; snapshot type: read-only)
3. Copy the snapshot to a secure storage account in a different resource group
4. Delete the snapshot from the source group after backup
5. Mount the snapshot on the forensic workstation

**CLI commands:**

```bash
az login
az vm show --name azure-ubuntu --query "storageProfile.osDisk.name"
az snapshot create --resource-group Production-group --name ubuntudisksnap --source <disk-id> --location eastus
```

## Summary of Key Points

- Azure is a multi-service cloud platform with a shared responsibility model
- Data storage is region/zone-based with multiple redundancy options
- Logging is extensive: Activity, Resource, Entra ID, NSG, VM, and Storage Analytics logs
- Forensics involves log analysis and VM disk snapshots
- Acquisition must follow a structured, forensically sound methodology

---
*My notes from the Microsoft Azure Fundamentals and Forensics module.*
