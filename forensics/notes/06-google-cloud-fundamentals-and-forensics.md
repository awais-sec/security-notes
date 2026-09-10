# Google Cloud Fundamentals and Forensics

*Module reference: CHFI (Computer Hacking Forensic Investigator) exam 312-49, Cloud Forensics — Google Cloud Fundamentals and Forensics (LOH07 / LOH08).*

## LOH07: Understand Google Cloud Fundamentals

### Introduction to Google Cloud

- Google Cloud: an initiative offering IaaS, PaaS, SaaS, and CaaS products
- Services: computing, storage, AI/ML, databases, analytics, networking, developer tools
- Physical resources: computers, VMs, hard drives across global data centers
- Data center structure:
  - Regions: geographic areas
  - Zones: isolated locations within regions (e.g. `asia-east1-x`)
- Resource access scope:
  - Global: networks
  - Regional: static external IPs
  - Zonal: VM instances, disks

### Google Cloud Architecture Framework — Six Pillars

1. System Design — foundation for meeting cloud requirements
2. Operational Excellence — adopt, organize, manage, monitor workloads
3. Security, Privacy, and Compliance — enhance workload/data security
4. Reliability — maintain workload resilience
5. Cost Optimization — increase business value for investments
6. Performance Optimization — maintain optimal performance

### Interacting with Google Cloud Services

1. **Google Cloud Console** — web-based GUI for resource/project management
2. **Command-Line Interface (CLI)** — `gcloud` commands (e.g. `gcloud compute instances create`), accessible via Cloud Shell in the console
3. **Client Libraries** — App APIs (service access) and Admin APIs (resource management); languages: C++, C#, Go, Java, Node.js, PHP, Python, Ruby

### Shared Responsibilities in Google Cloud

Based on workload type and service used. Customer considerations: regulatory compliance obligations, security standards and risk management, vendor/customer security requirements.

| Service Type | Customer Responsibility | Google Responsibility |
|---|---|---|
| IaaS (e.g. Compute Engine, Cloud Storage) | Most security (data, access, apps) | Physical security, fundamental infrastructure |
| PaaS (e.g. App Engine, GKE, BigQuery) | IAM, application-level controls, data/client security | Platform security |
| SaaS (e.g. Google Workspace, Chronicle) | Access controls, data for the application | Most security (app, platform, infrastructure) |
| FaaS/Serverless (e.g. Cloud Functions) | Similar to SaaS | Platform and infrastructure |

**Google Kubernetes Engine (GKE) shared responsibility:**

- **Google's responsibilities**: protect infrastructure (firmware, hardware, OS, kernel, storage, network), patch/harden node OSes, manage threat detection for containers, patch/harden Kubernetes components, provide integration support (Connect, KMS, IAM, Cloud Operations), log Google administrative accesses
- **Customer's responsibilities**: manage/monitor workloads (code, images, data, RBAC/IAM), ensure clusters are upgraded, examine applications/clusters using the security posture dashboard, provide environment details for troubleshooting

### Data Storage in Google Cloud

**Storage hierarchy:** Organization (company name) → Project (associated with an application; has APIs/resources) → Bucket (container for objects within a project) → Object (immutable file/data), with Managed Folders providing granular access within a bucket.

**Access methods:** Console (GUI), Google Cloud CLI, Client Libraries, REST APIs (JSON/XML), Terraform, Cloud Storage FUSE (mount a bucket to a local file system).

**Google Cloud storage classes:**

| Class | Use Case | Min. Storage | Availability | Location |
|---|---|---|---|---|
| Standard | Frequently accessed data (hot data) | — | 99.99% | Single region |
| Nearline | Data accessed < once/month | 30 days | 99.95% | Single region |
| Coldline | Data accessed < once/quarter | 90 days | 99.95% | Single region |
| Archive | Long-term preservation (< once/year) | 365 days | 99.95% | Multi-region |

**Storage options:**
- Object Storage — unstructured data (photos, videos); scalable, regional independence
- Network File Storage — hierarchical (files/folders); high reliability/availability
- Block Storage (Persistent Disks) — large volumes, attachable SSDs/HDDs, resizable, snapshots for forensics

**Google Cloud data storage services:**

| Category | Service | Use Case |
|---|---|---|
| Object Storage | Cloud Storage | Streaming videos, image libraries, data lakes |
| Block Storage | Persistent Disk | VM disks, database storage |
| Block Storage | Local SSD | Flash-optimized DBs, scratch disks |
| Block Storage | Hyperdisk | SAP HANA, Oracle, SQL Server, analytics |
| File Storage | Filestore | Data analytics, app migrations |
| File Storage | Parallelstore | AI/ML scratch space, modeling/simulation |
| File Storage | Cloud NetApp Volumes | Data sharing (Windows/Linux), ransomware recovery |
| Archival Storage | Cloud Storage | Backups, media archives |
| Data Transfer | Transfer Services | Migration from other clouds/private data centers |
| Data Transfer | Transfer Appliance | Secure physical data transfer |
| Backup and DR | Google Cloud Backup and DR | Protect VMs, VMware, databases, file systems |
| Mobile App Services | Cloud Storage for Firebase | User-generated content |
| Collaboration/File Storage | Google Workspace | Secure storage, collaboration, connectivity |
| Build Artifacts | Artifact Registry | Container images, OS packages, CI/CD integration |

### Logs in Google Cloud

Purpose: record administrative activities; answer "who did what, where, when?"

**Types of logs:**

1. **Google Cloud Audit Logs:**
   - Admin Activity Audit Logs — record API calls modifying config/metadata; cannot be disabled
   - Data Access Audit Logs — record API calls reading/modifying resource data; disabled by default
   - System Event Audit Logs — record Google Cloud actions modifying resource config; cannot be disabled
   - Policy Denied Audit Logs — record security policy violations; enabled by default
2. **VPC Flow Logs** — network flow data to/from VM instances
3. **Platform Logs** — generated by Google Cloud services for troubleshooting
4. **Kubernetes Logs** — container logs and metadata, available for a limited time

**Structure of an audit log entry:** project name; resource (type and instance); service (e.g. `cloudsql.googleapis.com`); payload (`protoPayload` containing an `AuditLog` object with `serviceName`, `authenticationInfo`, `metadata`); log name, formatted as:

```
projects/PROJECT_ID/logs/cloudaudit.googleapis.com%2Factivity
folders/FOLDER_ID/logs/cloudaudit.googleapis.com%2Fdata_access
organizations/ORGANIZATION_ID/logs/cloudaudit.googleapis.com%2Fsystem_event
```

**Log storage destinations:** Cloud Logging log buckets; Google Cloud projects (route logs between projects via Log Router); Pub/Sub topics (integrate with third-party apps like Splunk); Cloud Storage buckets (store logs in JSON format).

## LOH08: Perform Google Cloud Forensics

### Forensic Acquisition of Persistent Disk Volumes

**Methodology:** create instant snapshots for backup when a disk is compromised or failing.

**Steps:**
1. **Create instant snapshot** — Snapshots page → Create Snapshot; provide a name/description; type: Instant snapshot; source: Disk
2. **View instant snapshots** — Snapshots page → Instant snapshots tab → filter by source disk
3. **Copy snapshot to a different location** — Create Snapshot → snapshot source type: Instant snapshot → choose the source instant snapshot → type: Archive or Snapshot → location: Multi-regional (expensive) or Regional (less expensive)
4. **Delete the instant snapshot** after creating the long-term snapshot — Snapshots page → Instant snapshots tab → select snapshot → Delete

### Analyzing Google Workspace Logs

Integrate with Cloud Logging for security incident analysis. Filters for investigation:

- **Disabled Password Leak**: `protoPayload.resource.labels.service="login.googleapis.com"`, `logName="organizations/ORGANIZATION_ID/logs/cloudaudit.googleapis.com%2Fdata_access"`
- **Account Disabled Hijacked**: same filter as above
- **Two-step Verification Disabled**: same filter as above
- **Government-based Attack**: same filter as above
- **SSO Enablement Toggle**: `protoPayload.resource.labels.service="admin.googleapis.com"`, `protoPayload.metadata.event.parameter.value=DOMAIN_NAME`, `logName="organizations/ORGANIZATION_ID/logs/cloudaudit.googleapis.com%2Factivity"`
- **SSO Settings Changed**: same filter as above
- **Strong Authentication Disabled**: `protoPayload.resource.labels.service="admin.googleapis.com"`, `logName="organizations/ORGANIZATION_ID/logs/cloudaudit.googleapis.com%2Factivity"`

### Analyzing Log Data Using Google Cloud Log Analytics

- Run queries, integrate with BigQuery, create linked datasets
- Use cases: combine log data with threat intelligence (e.g. malicious URLs)

### Analyzing Google Cloud VPC Flow Logs

1. Navigation menu → Logging → Logs Explorer
2. Resource type → Subnetwork
3. Log name → `compute.googleapis.com/vpc_flows`
4. Query by `IP_ADDRESS` to filter logs
5. Expand a log entry → view `jsonPayload` for connection details

### Investigating Google Cloud Security Incidents

**1. Access Attempts from Anonymous Proxy**
- Security Command Center → Findings → "Evasion: Access from Anonymizing Proxy"
- Additional Information → Finding Details → Source Properties → retrieve `principalEmail` (compromised account), IP (attacker's proxy IP)
- Mitigation: check MITRE ATT&CK "Proxy: Multi-hop Proxy"; contact account owner

**2. BigQuery Data Exfiltration**
- Open "Exfiltration: BigQuery Data Exfiltration" finding
- Attributes tab → retrieve exfiltration (`sources`, `targets`)
- Source Properties → retrieve `projectId`, `jobLink`, `query`, `userEmail`, `contextUris`
- Investigate in Chronicle: Security Command Center → Event Threat Detection → Exfiltration
- Check logs: filters `protoPayload.methodName="Jobservice.insert"` or `"google.cloud.bigquery.v2.JobService.InsertJob"`
- Mitigation: MITRE ATT&CK "Exfiltration Over Web Service: Exfiltration to Cloud Storage"

**3. SSH Brute Force Attempts**
- Open "Brute Force: SSH" finding
- Source Properties → retrieve `sourceLogId` (`projectId`), Attempts (`srcIP`, `username`, `vmName`, `authResult`), `contextUris`
- Check logs: filters `logName="projects/projectId/logs/syslog"`, `labels."compute.googleapis.com/resource_name"="vmName"`
- Mitigation: MITRE ATT&CK "Valid Accounts: Local Accounts"

**4. Malware Incident**
- Open "Malware: Bad IP" finding
- Attributes tab → retrieve connections (`srcIP`, `dstIP`, ports, protocol)
- Source Properties → retrieve `sourceLogId` (`projectId`), `instanceDetails`, `contextUris`
- Check logs: filter `logName="projects/projectId/logs/compute.googleapis.com%2Fvpc_flows" AND (jsonPayload.connection.src_ip="srcIP" OR jsonPayload.connection.dest_ip="destIP")`
- Mitigation: MITRE ATT&CK "Dynamic Resolution and Command and Control"; check VirusTotal

**5. Persistent Anomalous IAM Grants**
- Open "Persistence: IAM Anomalous Grant" finding
- Source Properties → retrieve `projectId`, `principalEmail`, `bindingDeltas` (`action`, `role`, `member`), `contextUris`
- Check logs: filters `protoPayload.methodName="SetIamPolicy"`, `"google.iam.admin.v1.UpdateRole"`, `"google.iam.admin.v1.CreateRole"`
- Mitigation: MITRE ATT&CK "Valid Accounts: Cloud Accounts"

### Investigating Google Cloud Container Security Incidents

**1. Malicious Script Executed**
- Security Command Center → Findings → "Malicious Script Detected"
- Attributes tab → retrieve `resource.name`, `resource.project_display_name`, Processes (`binary`, `args`, `script.contents`, `script.sha256`, `script.path`)
- Source Properties → retrieve `Pod_Namespace`, `Pod_Name`, `Container_Name`, `Container_Image_Uri`, `VM_Instance_Name`
- Check logs:
  - Pod logs: `resource.type="k8s_container"`, filter by project, location, cluster, namespace, pod
  - Cluster audit logs: `logName="projects/.../cloudaudit.googleapis.com%2Factivity"`, `resource.type="k8s_cluster"`
  - GKE node console logs: `resource.type="gce_instance"`, `resource.labels.instance_id="instance_id"`
- Investigate running container: Kubernetes Clusters → Connect → Run in Cloud Shell → `kubectl exec --namespace=Pod_Namespace -ti Pod_Name -c Container_Name -- /bin/sh`
- Mitigation: MITRE ATT&CK "Command and Scripting Interpreter, Ingress Tool Transfer"

**2. Reverse Shell**
- Open "Reverse Shell" finding
- Attributes tab → retrieve `resource.name`, `resource.project_display_name`, Processes (`binary.path`, `args`)
- Source Properties → retrieve `Pod_Namespace`, `Pod_Name`, `Container_Name`, `Container_Image_Uri`, Reverse Shell stdin redirection fields (`Dst_Ip`, `Dst_Port`, `Src_Ip`, `Src_Port`), `VM_Instance_Name`
- Investigate running container: activate Cloud Shell → get GKE credentials (`gcloud container clusters get-credentials`) → `kubectl exec --namespace=Pod_Namespace -ti Pod_Name -c Container_Name -- /bin/sh` → `ps axjf` to view processes
- Mitigation: MITRE ATT&CK "Command and Scripting Interpreter, Ingress Tool Transfer"

### Investigating Google Cloud VM-based Security Incidents

**1. Cryptocurrency Mining Hash Match**
- Security Command Center → Findings → "Execution: Cryptocurrency Mining Hash Match"
- Attributes tab → retrieve `resourceName`, Processes (`binary.path`, `name`, `args`), indicator (`signatures`, `memory_hash_signature`, `binary_family`, `detections`)
- Check logs: Logs Explorer → analyze intrusion signs
- Mitigation: MITRE ATT&CK "Execution" techniques

**2. Cryptocurrency Mining YARA Rule**
- Security Command Center → Findings → "Execution: Cryptocurrency Mining YARA Rule"
- Attributes tab → retrieve `resourceName`, Processes, indicator (`signatures`, `yara_rule_signature`, `yara_rule_name`)
- Mitigation: MITRE ATT&CK "Execution" techniques

## Key Takeaways

- **Google Cloud Fundamentals**: understand architecture, shared responsibility, storage classes, and logging
- **Forensic Acquisition**: use snapshots for persistent disk evidence
- **Incident Investigation**: leverage Security Command Center, Cloud Logging, Chronicle, and the MITRE ATT&CK framework
- **Container Forensics**: analyze malicious scripts and reverse shells in GKE
- **VM Forensics**: detect cryptocurrency mining via hash/YARA rule matching
- **Legal/Procedural**: adhere to proper authorization and chain of custody in cloud investigations

---
*My notes from the Google Cloud Fundamentals and Forensics module.*
