# Assignment #4 — Activating Log Creation on SQL Server

*Advanced Forensics, presented to Kaukab Jamal Zuberi. Submitted by Awais Ahmed, BS-DFCS, Sp-2023, Sec-A, Roll No. 008.*

## 1. SQL Server Error Log

**Cycle the error log** (creates a new log file):

```sql
EXEC sp_cycle_errorlog;
```

**Configure retention (SSMS):** right-click the server → Properties → Configure under the "Error log" section. Set the maximum number of error log files (default: 6 + current log).

## 2. Extended Events (Query/Event Logging)

**Create an Extended Events session (T-SQL):**

```sql
CREATE EVENT SESSION [QueryLog] ON SERVER
ADD EVENT sqlserver.sql_statement_completed
ADD TARGET package0.event_file(SET filename=N'C:\Logs\QueryLog.xel')
WITH (STARTUP_STATE=OFF);
```

**Start the session:**

```sql
ALTER EVENT SESSION [QueryLog] ON SERVER STATE = START;
```

**View logs:** in SSMS, Management → Extended Events → Session → View Target Data.

## 3. SQL Server Audit (Security/Compliance)

**Create an audit:**

```sql
USE master;
CREATE SERVER AUDIT [SecurityAudit]
TO FILE (FILEPATH = N'C:\Audits\')
WITH (ON_FAILURE = CONTINUE);
```

**Create an audit specification** (e.g., log failed logins):

```sql
CREATE SERVER AUDIT SPECIFICATION [FailedLogins]
FOR SERVER AUDIT [SecurityAudit]
ADD (FAILED_LOGIN_GROUP);
```

**Enable the audit:**

```sql
ALTER SERVER AUDIT [SecurityAudit] WITH (STATE = ON);
ALTER SERVER AUDIT SPECIFICATION [FailedLogins] WITH (STATE = ON);
```

## 4. SQL Server Profiler (Deprecated)

Launch Profiler from SSMS → Tools → SQL Server Profiler. Create a new trace, select events (e.g., `TSQL:SQL:BatchCompleted`), and save to a file or table.

## 5. Transaction Log Management

**Backup the transaction log** (affects recovery, not logging):

```sql
BACKUP LOG [YourDB] TO DISK = N'C:\Backups\YourDB_Log.bak';
```

## 6. View Existing Logs

**Error logs:**

- SSMS: Management → SQL Server Logs.
- T-SQL: `EXEC sp_readerrorlog 0` (0 = current log).

**Agent logs:**

- SSMS: SQL Server Agent → Error Logs.
