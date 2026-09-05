# Active Directory Forensics

Exam-prep notes on Active Directory (AD) forensics, structured as short and medium-length Q&A plus a case-based long-form question.

## Section A: Short Questions

**1. Define Active Directory and explain its purpose in enterprise environments.**

Active Directory (AD) is a hierarchical directory service developed by Microsoft, introduced with Windows 2000 Server. It stores user accounts, computers, and network resources, and provides centralized authentication and authorization. It is used to manage permissions and access to networked resources across the enterprise.

**2. What is the forensic significance of the NTDS.dit file?**

The NTDS.dit file is a critical component in AD forensics. It contains the Active Directory database, storing user credentials (e.g., password hashes), group memberships, and other metadata. Investigators use this file to extract evidence, such as login times and deleted accounts, during incident response and auditing.

**3. Differentiate between EDB.log, EDB.chk, and EDB0000X.log.**

- `EDB.log`: Stores Active Directory transaction logs before they are committed to NTDS.dit.
- `EDB.chk`: Checkpoint file used to track uncommitted transactions.
- `EDB0000X.log`: Overflow log files created when EDB.log is full.

All three are essential for reconstructing AD activity during forensic analysis.

**4. What is the SYSTEM hive, and why is it important for AD forensics?**

The SYSTEM hive is a registry file that stores the boot key (also known as the syskey), which is required to decrypt password hashes stored in the NTDS.dit file. It is essential for tools like NTDSDumpEx and DSInternals to successfully extract and interpret encrypted data.

**5. Mention any three legal considerations when handling AD data in forensics.**

- Use tools like NTDSDumpEx or Impacket only in environments where you have explicit legal authorization.
- Handling NTDS.dit and SYSTEM hives can expose sensitive user credentials, so privacy must be preserved.
- Unauthorized analysis or extraction may lead to severe legal consequences or violation of ethical codes.

**6. What is a tombstoned object in AD, and how is it relevant in investigations?**

A tombstoned object is a deleted AD object (e.g., user or group) that is retained for a limited time (typically 60 days). During forensics, analyzing tombstoned metadata helps investigators recover or validate deleted accounts and understand deletion timelines.

**7. Write any two cmdlets used in DSInternals and their purposes.**

- `Get-ADDBAccount`: Extracts account data (including password hashes and metadata) from an offline NTDS.dit file.
- `Test-PasswordQuality`: Audits password hashes against a list of weak/common passwords to assess password strength.

## Section B: Medium-Length Questions

**1. Describe the structure of NTDS.dit and the process of extracting password hashes.**

The NTDS.dit file is an ESE (Extensible Storage Engine) database storing Active Directory data. It includes:

- `EDB.log`: Transaction log
- `EDB.chk`: Checkpoint log
- `EDB0000X.log`: Overflow logs

To extract password hashes:

1. Obtain NTDS.dit and the SYSTEM hive from the Domain Controller.
2. Decode the PEK using the boot key.
3. Use RC4 and DES to decrypt the password hashes (LM/NT).

Tools like NTDSDumpEx and DSInternals automate these steps.

**2. Explain how NTDSDumpEx works. Include its main features and a sample command.**

NTDSDumpEx is a forensic tool for extracting and analyzing data from NTDS.dit. Features include:

- Password hash extraction (NTLM, Kerberos)
- Account enumeration (SIDs, lockout status)
- Group membership analysis
- Output in CSV or JSON
- SYSTEM hive integration for decryption

Command example:

```
NTDSDumpEx.exe -ntds ntds.dit -system SYSTEM -out output.csv
```

This outputs account and hash data into a CSV for analysis.

**3. Discuss how Impacket can be used in Active Directory forensics, with focus on secretsdump.py and psexec.py.**

Impacket is a Python toolkit for manipulating network protocols, used in red teaming and forensics.

- `secretsdump.py`: Extracts NTLM password hashes from NTDS.dit, SAM, or LSASS dumps.
- `psexec.py`: Executes commands remotely on a target machine via SMB, useful for post-breach investigations.

Together, they allow credential dumping and remote execution for forensic reconstructions.

**4. Describe a practical scenario where an unauthorized account breach is investigated using forensic tools.**

Scenario: a breach is suspected via a high-privileged account.

1. Copy NTDS.dit and the SYSTEM hive from the Domain Controller.
2. Use NTDSDumpEx to extract accounts, login times, and password hashes.
3. Check for unexpected UAC flags or group memberships (e.g., Domain Admin).
4. Use DSInternals to audit password strength and detect misconfigurations.
5. Recover tombstoned (deleted) users if applicable.

**5. Explain the challenges faced while analyzing Active Directory artifacts. How can they be mitigated?**

Challenges:

- Large file sizes of NTDS.dit make analysis resource-intensive.
- Accessing the boot key (SYSTEM hive) is mandatory for decryption.
- Tombstoned objects expire after roughly 60 days, limiting historical data.

Mitigations:

- Use offline backups and image copies.
- Document all investigative steps and timestamps.
- Analyze data from multiple Domain Controllers.

## Section C: Practical Task

Not applicable — this cohort covers theoretical content only, no hands-on practical component for this section.

## Section D: Long-Form Question (Case-Based)

### Option A: Investigation with NTDSDumpEx, DSInternals, and Impacket

**NTDSDumpEx**

- Extract hashes and account details.
- Identify suspicious group memberships (e.g., Domain Admin).

**DSInternals**

- Extract Kerberos keys.
- Audit password policies and simulate attacks.
- Detect replication abuse.

**Impacket**

- Use `secretsdump.py` for remote hash extraction.
- Use `getTGT.py` to examine Kerberos Ticket Granting Tickets.
- Use `psexec.py` to remotely investigate suspicious endpoints.

**Artifacts to examine:**

- UAC flags
- Login timestamps
- Tombstoned objects
- Weak or reused passwords
- SID history and ACL misdelegation

### Option B: Forensic Readiness Plan for AD

- **Backups**: Regular backups of NTDS.dit, the SYSTEM hive, and SYSVOL.
- **Toolset**: Maintain updated copies of NTDSDumpEx, DSInternals, and Impacket.
- **Logs**: Enable auditing for logins, group changes, and deletions.
- **Access control**: Restrict Domain Controller access. Store keys securely.
- **Legal compliance**: Define SOPs for incident response, including legal authorization for data access.
- **Testing**: Run regular forensic drills using dummy AD environments.
- **Time-sensitive data**: Monitor tombstone periods and extract logs routinely.
