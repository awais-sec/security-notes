# Critical Azure Storage Logs: Forensic Investigation

## 1. Introduction and Importance of Azure Storage Logs in Forensics

- Breach response relies heavily on storage telemetry to reconstruct events in the first critical hour
- Storage logs provide a trail of access to files, containers, and snapshots
- The difference between suspicion and proof often hinges on a single log line showing caller IP, authentication type, and the precise operation on a blob
- Storage evidence is often overlooked but can determine investigation outcomes
- **Key message**: storage logs should be part of every incident response plan, not an afterthought

## 2. What Azure Storage Logs Capture

- Azure Monitor storage tables record detailed access logs
- `StorageBlobLogs` schema includes: account name, caller IP, authentication type, authorization details, operation name (e.g. `GetBlob`, `PutBlob`), target path (blob/container), response code, timing (latency, timestamps)
- **Logs vs. metrics**: metrics show volume/activity level; logs show who, what, where, when, and result — essential for timeline reconstruction

## 3. Collection and Storage of Logs

- Resource logs for `Microsoft.Storage` can be sent to: a Log Analytics workspace (for KQL queries and alerting), a storage account (for archiving), or Event Hubs (for streaming)
- Classic Storage Analytics and classic metrics are deprecated — migrate to Azure Monitor
- Diagnostic settings must be enabled for Blob, File, Queue, and Table services
- Centralize logs for security and compliance
- Retention planning is critical: monitor changes in diagnostic settings retention features; plan for Log Analytics (short-term) and archive storage (long-term)

## 4. Building an Incident Timeline from Blob Logs

**Process:**
1. Start with a time window and pull `StorageBlobLogs`
2. Group by operation name (`GetBlob`, `PutBlob`, `CopyBlob`, `Delete`, `SetBlobTier`)
3. Overlay response codes and latencies to distinguish automated vs. manual activity
4. Pivot from a suspicious IP to all containers/files accessed
5. Trace the sequence of events for a clear, reviewer-friendly timeline

Forensic value: logs show reads, writes, copies, deletions, and tier changes.

## 5. Detecting SAS Token Misuse and Shared Key Exposure

- Attackers exploit over-permissive Shared Access Signature (SAS) tokens and compromised account keys
- Logs reveal patterns in access methods and time windows
- **Mitigation**: prefer user-delegation SAS tied to Microsoft Entra ID; use direct Entra authorization where possible; disable Shared Key authentication; restrict SAS scope and lifespan
- Improved attribution: logs show real user/workload identity instead of anonymous keys

## 6. Proving Data Exfiltration

**Evidence includes:**
- Counts of successful `GetBlob` and `List` operations
- Source/destination IPs (especially external)
- Bytes transferred (response sizes, content length)

**Process:**
- Query logs for sensitive containers
- Calculate data transfer volumes
- Correlate with firewall logs, NSG flow logs, and proxy records

**Limitation**: logs alone may not fully prove exfiltration but narrow it down significantly.

## 7. Correlating with Other Logs

- **Azure Activity Log**: shows management operations (policy changes, key rotations, network rule changes); helps identify rushed config changes that enabled the breach
- **Microsoft Entra ID Logs**: sign-in logs, audit logs, conditional access results; correlate with `StorageBlobLogs` caller IPs
- Triangulation provides identity context for legal/audit purposes

## 8. KQL Examples for Fast Investigation

**High-value queries:**
- Which identities accessed container X from which IPs?
- Which blobs were read more than N times in an hour?
- Which requests used a shared key or account SAS?

**Large-scale/historical analysis:** use Azure Data Explorer for archived logs — enables fast querying over months of data without impacting the live workspace.

## 9. Evidence Preservation and Chain of Custody

- **Immutable storage**: use Azure Immutable Blob Storage with time-based retention and legal holds — a write-once, read-many state until the hold is cleared
- **Forensics vault design**: a dedicated SOC storage account with version-level immutability, storing snapshots, exports, and case files
- **Chain of custody process**: use a controlled subscription with restricted roles; automate access logging to capture who accessed evidence; document every transfer with timestamps and hashes; store collection scripts and hash manifests in an immutable container; treat chain of custody as both a technical and legal process

## 10. Common Pitfalls

1. **Logs not enabled everywhere** — new storage accounts may miss diagnostic settings; solution: continuous auditing and alerting on configuration drift
2. **Identity darkness** — overuse of shared keys or broad SAS tokens obscures attribution; solution: prefer Entra-based access and user-delegation SAS

## 11. Incident Response Playbook

- **Triage phase**: isolate the affected storage account, capture a log snapshot, freeze evidence in immutable storage
- **Investigation lenses**: identity (who accessed), network (from where), operation (what was done), object (which blobs/containers)
- **Closure**: write a sourced narrative with sample log lines; attach timestamps, IPs, result codes; map findings to actions taken and controls improved; feed lessons into baseline policies

## 12. Cost and Retention Strategy

- Short-term (30–90 days): keep logs searchable in Log Analytics
- Long-term: archive to a low-cost storage account with lifecycle policies
- High-volume tenants: summarize common queries into tables, export only necessary fields for long-term evidence
- Stay updated on Azure feature deprecations to avoid gaps

## 13. Readiness Checklist

1. Enable diagnostic settings for all storage accounts, feeding a central workspace
2. Block shared keys, prefer Entra auth, restrict SAS to short-lived user delegation
3. Test forensic queries for exfiltration and identity attribution
4. Stage an immutable evidence vault and practice using it
5. Run tabletop exercises simulating token leaks/breaches
6. Validate the ability to pull and analyze logs within minutes, not hours

## 14. Real-World Examples and Notes

- Common incident vector: long-lived SAS tokens exposed in public repositories (GitHub, etc.)
- A widely cited Microsoft case study involved an over-permissive token exposed in a public repo, which prompted broader guidance on token hygiene
- Over-scoped/long-lived tokens are consistently flagged as low-hanging fruit for attackers
- Forensic pivot: correlate log entries (e.g. reads from an unexpected ASN) with events like code commits

## 15. Key Takeaway

Treat storage accounts like crime scenes with built-in CCTV: enable logging, focus on critical areas, and ensure tamper-proof storage. With proper logging and preservation, forensic investigation becomes a reliable path to truth.

---
*Source: coursework notes summarizing third-party security research/vendor content on Azure storage forensics. The original also included a vendor "about us" and contact-details section for a security services firm, which has been dropped here as marketing content rather than forensics knowledge.*
