# Business Resilience and Backup

## Backup Schemes

Methods used to copy and store data so it remains available in case of data loss.

- **Full Backup**: Copies all main files and folders to backup media. Creates a unique, self-contained archive for restoration, but takes more time and media capacity.
- **Incremental Backup**: Copies only files/folders that changed or were added since the last backup. Faster and lighter than a full backup, but restoration requires every backup set in the chain.
- **Differential Backup**: Copies files/folders added or changed since the last *full* backup. Faster than a full backup, and restoration only needs the latest full backup plus the latest differential.
- **Method of rotation**: Different rotation schemes used to make sure data isn't lost and can be restored efficiently.

## RPO vs. RTO

| | RPO (Recovery Point Objective) | RTO (Recovery Time Objective) |
|---|---|---|
| Definition | Earliest point in time data can be recovered to | Amount of time allowed to recover a business function |
| Focus | Data loss tolerance | Time to restore operations |
| Measurement | Acceptable data loss in case of disruption | Time frame for recovery after a disaster |
| Example | Tape backups, log shipping, disk-based backups | Hot standby, cold standby, active-active clustering |

## Clustering

Grouping multiple servers (nodes) to work together for high availability. If one node fails, another takes over, minimizing downtime.

- **Active-Passive Clustering**: The application runs on one active node; other nodes stay passive, used only if the active node fails.
- **Active-Active Clustering**: The application runs on multiple active nodes simultaneously, sharing load and providing redundancy.

Both protect against single points of failure and keep service continuous.

## Disaster Objectives

A **Disaster Recovery Plan (DRP)** is a structured collection of processes and procedures designed to ensure business continuity and quick IT recovery after a disaster.

- **Recovery Point Objective (RPO)**: The maximum acceptable data loss, measured in time.
- **Recovery Time Objective (RTO)**: The maximum acceptable downtime for critical business functions.

### Physical access

Securing physical access to library contents so only authorized people can reach them. Includes measures like encrypting backup media (like in transit) and ensuring physical construction can withstand heat, fire & water.
### Environmental control

Maintaining proper environmental conditions to protect IT assets: temperature, humidity, static electricity, surge, and fire protection in the server facility, along with protecting backup media and keeping the environment clean.
