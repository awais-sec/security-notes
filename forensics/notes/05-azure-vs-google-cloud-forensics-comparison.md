# Azure vs. Google Cloud Forensics: A Comparison

A side-by-side comparison of how Azure and Google Cloud (GCP) structure their environments, storage, logging, and forensic acquisition, plus deeper investigative techniques and case studies.

## Series 1: Fundamentals, Resource Governance, and Access Methods

### Resource Governance Hierarchy

- **Microsoft Azure**: a logical construct of Subscriptions, which contain Resource Groups. Individual resources (VMs, disks, IPs) are deployed within these groups.
- **Google Cloud (GCP)**: hierarchy starting from the Organization, moving to Projects, which then contain Buckets and Objects (files). All objects and projects are considered resources.

### Management and Access Tools

Both platforms offer a web-based console/portal and a CLI.

- **Azure**: Azure Portal (unified console), Azure PowerShell (AzureRM/Az cmdlets), Azure CLI (programmatic automation)
- **GCP**: Google Cloud Console (GUI), Google Cloud CLI (`gcloud` commands, installed locally or via Cloud Shell), Client Libraries (APIs for granting access and managing resources programmatically)

### Architecture Frameworks

- **Azure**: governance depends on Resource Providers (like `Microsoft.Compute`) and Resource Managers that validate requests based on authentication, permissions, and subscription limits
- **GCP**: a 6-pillar Architecture Framework — Operational Excellence; Security, Privacy, and Compliance; Reliability; Cost Optimization; Performance Optimization

### Platform-Specific Governance Notes

- **Azure governance model**: a single service can create multiple resources — one Azure VM is composed of a disk, a public IP, and a network adapter, all considered individual entities
- **GCP resource scope**: resources are explicitly categorized by scope — Global (e.g. networks), Regional (e.g. static external IPs), or Zonal (e.g. VM instances and disks)
- **GCP access alternatives**: besides standard tools, GCP supports Terraform for infrastructure scaling and Cloud Storage FUSE, which mounts buckets to a local file system for read/write operations

## Series 2: Storage Architecture, Redundancy, and Specialized Services

### Core Storage Categories

**Object Storage:**
- Azure: Blobs (Binary Large Objects) for unstructured data — Block blobs (text/binary), Page blobs (VHD backups), Append blobs (VM logs)
- GCP: Cloud Storage for objects stored in buckets, highly scalable, accessible from anywhere

**File Storage:**
- Azure: Azure Files (SMB/NFS/REST), Azure NetApp Files for complex enterprise applications
- GCP: Filestore (fully managed), Parallelstore (high-performance parallel service), Google Cloud NetApp Volumes

**Block Storage (Disks):**
- Azure: Azure Managed Disks (OS and Data disks)
- GCP: Persistent Disks (SSD/HDD), Local SSD (ephemeral), Hyperdisk (next-gen high performance)

### Data Redundancy and Availability

**Azure replication types:**
- LRS (Locally Redundant) — 3 copies in one physical location
- ZRS (Zone-Redundant) — 3 copies across three availability zones in one region
- GRS (Geo-Redundant) — LRS in a primary region plus an asynchronous copy to a secondary region
- GZRS (Geo-Zone-Redundant) — ZRS in a primary region plus an asynchronous copy to a secondary region

**GCP storage classes (by availability and duration):**
- Standard — "hot" data, 99.99% availability in a single region
- Nearline / Coldline — infrequently accessed data (30-day and 90-day minimums respectively)
- Archive — long-term preservation (365-day minimum), stored across several regions worldwide

### Specialized Storage Solutions

- **Azure**: Azure Elastic SAN for large-scale iSCSI storage; Azure Container Storage for persistent volumes in Kubernetes
- **GCP**: Cloud Storage for Firebase for mobile user content; Artifact Registry for managing container images and OS packages

### Disk and Snapshot Mechanics

- **Azure disk roles**: investigators must distinguish between OS disks (contains the boot volume, usually drive C:), Data disks (user-defined letters), and Temporary disks (not part of the managed disk, used for short-term storage — drive D: on Windows or `/dev/sdb` on Linux)
- **GCP Persistent Disks**: during forensic acquisition, these allow teams to create snapshots without losing data, even while the disks are attached to active VMs
- **GCP storage hierarchy**: all objects, buckets, and projects are considered Resources within the storage environment
- **Azure triplicate rule**: data stored in Azure storage remains in triplicate within specific datacenters in a region to ensure durability

## Series 3: Logging Frameworks, Monitoring, and Forensic Evidence

### Core Logging Frameworks

- **Azure**: Azure Platform Logs, split into Activity Logs (management plane — subscription-layer operations like creation or deletion) and Resource Logs (data plane — operations within a specific resource; not collected by default, requires Diagnostic Settings)
- **GCP**: Cloud Audit Logs, categorized into four types — Admin Activity (metadata/config changes), Data Access (reading metadata or resource data), System Event (Google-initiated actions), and Policy Denied (security violations). Admin Activity and System Event logs are mandatory and cannot be disabled.

### Network Monitoring

- **Azure**: Network Security Group (NSG) Flow Logs via Azure Network Watcher; JSON format, retained up to one year
- **GCP**: VPC Flow Logs recording network flow data to/from VM instances; investigators can gather these in real time by subscribing to Pub/Sub

### Identity and Workspace Logging

- **Azure**: forensic evidence in Microsoft Entra ID reports — Security Reports (risky users/sign-ins) and Activity Reports (audit logs and sign-in status)
- **GCP**: Google Workspace Logs, critical for investigating compromised accounts, password leaks, or government-backed attacks

### Forensic Analytics Tools

- **Azure**: centralizes telemetry into Log Analytics Workspaces, uses Azure Sentinel (cloud-native SIEM) for real-time monitoring and anomaly detection; investigators use Kusto Query Language (KQL) to parse large datasets like Unified Audit Logs (UALs)
- **GCP**: Logs Explorer for manual log analysis; Log Analytics for BigQuery-linked queries against logs stored in buckets

### Platform-Specific Logging Features

- **Azure Storage Forensics**: Azure generates Storage Analytics Logs specifically for Blobs, Queues, and Tables, automatically stored in a specialized, non-removable container called `$logs`
- **Azure Provisioning Logs**: uniquely record access attempts to integrated third-party applications like ServiceNow and Adobe
- **GCP log structure**: every GCP log entry is stored as a `LogEntry` object containing a `protoPayload` field housing detailed metadata like `serviceName` and `authenticationInfo`
- **GCP Data Access limitations**: while Azure requires setting up diagnostics for resource logs, GCP's Data Access audit logs are disabled by default solely due to their large size and must be manually enabled by the project owner
- **Azure AI integration**: Azure emphasizes AI/ML through Azure ML Studio to automate detection of low-frequency anomalies that traditional rule-based systems might miss

## Series 4: Forensic Acquisition and Security Incident Investigation

### Forensic Acquisition of VM Disks

- **Azure**: create a snapshot of the OS disk (via Portal or CLI), then copy that snapshot to a storage account under a different resource group for analysis. Stop the VM before taking the snapshot to ensure consistency.
- **GCP**: Instant Snapshots, designed for effective backup and analysis even while the disks are attached to active VM instances. These can be viewed, copied to a different location (regional or multi-regional), or converted into long-term/archive snapshots.

### Investigating Phishing and Account Compromise

- **Azure**: relies on Azure Sentinel and KQL queries to detect indicators of compromise (IoCs) — unusual login locations and failed login attempts (Windows Security Event ID 4625)
- **GCP**: analyzes Google Workspace Logs using filters in the `protoPayload` field — e.g. "Account Disabled Hijacked," "Disabled Password Leak," "Government-backed Attack"

### Malware and Exfiltration Investigations

- **Azure**:
  - Malware: NSG Flow Logs identify suspicious inbound/outbound traffic
  - Exfiltration: query successful `GetBlob` and `List` operations from external IPs, calculating bytes transferred to estimate the volume of data that left the environment
- **GCP**:
  - Malware: Security Command Center (SCC) findings such as "Malware: Bad IP"; scan VPC Flow Logs for connections to known C&C IPs, enrich via VirusTotal
  - Exfiltration: SCC findings like "Exfiltration: BigQuery Data Exfiltration," which provides attributes on source tables and destination targets

### Anomaly Detection and AI Integration

- **Azure**: Azure ML Studio, using algorithms like Isolation Forests to detect low-frequency anomalies that standard threshold-based alerts might miss
- **GCP**: automated findings for specialized threats — "Execution: Cryptocurrency Mining Hash Match" (memory signatures) and "Persistence: IAM Anomalous Grant" (suspicious domain additions as project owners)

### Platform-Specific Investigative Procedures

- **Azure chain of custody**: a technical and legal process for the "Forensics Vault" — a controlled subscription with restricted roles and automated capture of every person who accessed the evidence container
- **Azure immutable storage**: immutable blob storage with time-based retention or legal holds, locking evidence into a write-once, read-many (WORM) state until the hold is cleared
- **GCP container forensics (GKE)**: workflows for Kubernetes security incidents, such as analyzing "Malicious Script Executed" or "Reverse Shell" findings; investigators can connect directly to a running container environment via Cloud Shell using `kubectl exec`
- **GCP threat enrichment**: uses Chronicle to enrich security findings, pivoting from an SCC finding directly into a guided interface for deeper threat analysis

## Best Practices

### 1. Azure Storage Telemetry: the "Breadcrumbs" of Evidence

Storage telemetry is often the difference between a hunch and proof.

- **Specific schema fields** to look for when querying `StorageBlobLogs`:
  - Caller IP & authentication type — links credential theft to actual data movement
  - Operation name — specifically track `GetBlob`, `PutBlob`, `CopyBlob`, `SetBlobTier` to see if data was read, written, or wiped
  - Response codes & latencies — high latency or specific error codes can distinguish "automated bursts" (bots) from "manual probing" (human attackers)
- **Deprecation warning**: Classic Storage Analytics and metrics have been retired/superseded by Azure Monitor — relying on old pipelines can create forensic blind spots

### 2. Deep-Dive: Identity and Access Misuse

- **"Identity darkness" pitfall**: if an environment uses shared keys or broad Shared Access Signatures (SAS), attribution becomes messy
- **SAS token hygiene**: attackers favor exposed SAS links; user-delegation SAS (tied to Entra ID) is superior for forensics because it provides real user context in the logs
- **Disabling shared keys**: recommended to disable "Shared Key" access entirely where possible to improve identity attribution

### 3. Advanced Azure Analytics: KQL and AI/ML

- **Isolation Forests**: an unsupervised learning model used in Azure ML Studio that detects low-frequency anomalies (outliers) without needing previously labeled data
- **Standard deviation in KQL**: a specific forensic technique — compare a user's current login time to their 14-day average; if the login occurs more than 3 hours outside their typical range, flag it as an anomaly

### 4. GCP Forensic Nuances

- **Managed Folders**: beyond Buckets and Objects, GCP uses Managed Folders for granular access above what's provided to the complete bucket
- **Cloud Storage FUSE**: mounts a storage bucket to a local file system, enabling standard file-based forensic tools to perform read/write operations on cloud data
- **GCP shared responsibility (GKE)**: Google is responsible for patching the control panel and node OSes; the customer is responsible for RBAC/IAM policies and monitoring the actual workloads

### 5. Legal and Chain of Custody Rigor

The "Forensics Vault" is a combined technical and legal process:
- Use a controlled subscription with restricted roles and automated capture of everyone who accesses the evidence container
- Document every transfer with timestamps and hashes
- Use version-level immutable storage in a dedicated SOC storage account to stage snapshots and case files, preventing tampering before legal review

### 6. GCP-Specific Incident Signatures (Quick Reference)

Specific findings to look for in Security Command Center:
- **SSH Brute Force**: look for "Persistence: SSH Brute Force," examine the `attempts` field for the count of login attempts
- **IAM Anomalous Grant**: flagged when a gmail.com user is added as a project owner, or a service account is added from outside the organization's perimeter
- **Cryptocurrency Mining**: detectable via memory signature matches or YARA rules matching known constants of mining software

## Step-by-Step Methodologies and Case Studies

### General Forensic Process (High-Level Framework)

A fundamental four-phase process for any cloud forensic investigation:

1. **Identification** — recognizing and localizing potential sources of evidence, such as hard drives, network logs, or emails
2. **Preservation** — securing and isolating evidence to prevent alteration, often involving hashing and generating backups
3. **Analysis** — using forensic tools to reveal patterns, decrypt data, or recover deleted files to reconstruct events
4. **Presentation** — organizing findings into succinct reports, timelines, or activity diagrams for legal admissibility

### Stepwise Forensics: Azure vs. GCP

**A. Azure VM acquisition methodology**
1. Stop the VM — ensures data consistency during the snapshot
2. Create an OS disk snapshot (via Azure Portal or CLI)
3. Transfer the snapshot to a storage account in a different resource group (e.g. a "Security-group") to prevent tampering
4. Create a backup copy and delete the snapshot from the source production group
5. Mount the snapshot on a forensic workstation for deep analysis

**B. GCP Persistent Disk acquisition methodology** (focused on Instant Snapshots)
1. Create an Instant Snapshot — can be done while the disk is still attached to an active VM
2. View and verify the snapshot in the Google Cloud Console
3. Convert/copy it to a different regional or multi-regional location
4. Delete the instant snapshot only after a permanent, long-term snapshot has been successfully created

**C. Incident timeline creation (Azure Storage)** — when investigating data theft:
1. Define the window ("start with a clock")
2. Pull `StorageBlobLogs` for that window
3. Group operations by operation name (e.g. `GetBlob`, `Delete`)
4. Overlay response codes and latencies to distinguish bot activity from human probing
5. Pivot: trace all actions taken by a specific suspicious IP across all containers

### Case Studies

**The Azure ransomware case study**

A medium-sized enterprise scenario where a phishing email led to a ransomware attack:
- **The breach**: an attacker used stolen credentials from a phishing link to gain access to a high-value account (`finance_admin@company.com`)
- **The investigation**: analysts used KQL to find anomalous logins from "HighRiskRegions" and detected privilege escalation where the attacker added accounts to the "Global Admin" role
- **The outcome**: by analyzing the `$logs` container and Sign-in Logs, the team isolated the malicious IP and identified exactly which files were accessed before they could be fully encrypted

**GCP incident examples** (Security Command Center finding categories used as mini case studies):
- **BigQuery Exfiltration**: investigating when a resource is saved outside the network perimeter or when VPC Service Controls are terminated
- **SSH Brute Force**: using syslog and VPC Flow Logs to identify external IPs making repeated, failed connection attempts to a VM
- **Cryptocurrency Mining**: using memory signature matches (e.g. `ethminer`) to find hidden mining scripts on compromised VMs

**Analogy for stepwise forensics**: think of cloud forensics as investigating a bank robbery in a digital building — the methodology is the police procedure (first cordoning off the area by isolating the VM, then taking photographs of the scene via snapshots, then checking the visitor logs via activity logs), while the case study is the detective's final report explaining exactly how the thief got in (phishing), which vault they opened (privilege escalation), and what they took before the alarms went off.

---
*My comparison notes from the Cloud Forensics module.*
