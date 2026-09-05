# Assignment #5 — NTLM vs Kerberos in ADS

*Advanced Forensics, presented to Kaukab Jamal Zuberi. Submitted by Awais Ahmed, Criminology & Forensics Sciences, BS-DFCS, Sp-2023, Roll No. 008.*

## NTLM vs Kerberos in ADS

| Feature | NTLM (NT LAN Manager) | Kerberos |
|---|---|---|
| Type | Challenge-response protocol | Ticket-based authentication system |
| Introduced in | Older protocol (Windows NT) | Modern (default from Windows 2000 onwards) |
| Authentication | One-way (client authenticates to server) | Mutual (client and server authenticate each other) |
| Encryption | Weak hashing (MD4, HMAC-MD5) | Strong encryption (AES, RC4, etc.) |
| Dependency | No time synchronization required | Requires synchronized time (within 5 minutes) |
| Performance | More requests and slower | Faster after initial authentication |
| Security | Vulnerable to pass-the-hash, replay attacks | More secure and robust against modern threats |
| Credential storage | Password hash sent in challenge | Tickets and session keys used |
| Default in ADS | Used only when Kerberos is not available | Default authentication protocol |

### Which Is Better?

Kerberos is better for these reasons:

- Mutual authentication ensures both parties are verified.
- Uses tickets, reducing repeated password exchange.
- Supports delegation, smart card login, and single sign-on (SSO).
- Stronger encryption mechanisms, resistant to replay and man-in-the-middle attacks.

NTLM is still supported for backward compatibility, but it's considered outdated and should be disabled where possible.

## Partitions of Active Directory

Active Directory is logically divided into partitions, also called naming contexts (NCs), for efficient storage and replication. AD data is logically divided into three main partitions:

1. **Schema Partition**: stores definitions of all object classes and attributes within the directory.
2. **Configuration Partition**: contains information about the Active Directory topology and replication settings.
3. **Domain Partition**: stores domain-specific data, such as user accounts, groups, and computers.

The Schema and Configuration partitions are created during the initial installation of Active Directory.

## Summary

- **Kerberos > NTLM**: Kerberos is more secure, scalable, and preferred in modern networks.
- **ADS partitions** ensure data is logically organized and replicated efficiently:
  - Schema: defines object structure (forest-wide)
  - Configuration: forest structure and services
  - Domain: domain-specific data
  - Application: custom/app data
