# Combatting Tunneling and Encryption

This chapter covers overcoming the hurdles of data encrypted via TLS/SSL, custom encryption mechanisms, or wireless standards like WEP/WPA2 to obtain meaningful forensic evidence.

## 1. Decrypting TLS Using Browsers

- **The key log feature**: modern browsers like Chrome can log symmetric session keys to a file, allowing an investigator to decrypt captured traffic later.
- **Setup (Windows)**: add a user environment variable named `SSLKEYLOGFILE` and set its value to a specific file path (e.g., `C:\Users\Apex\ssl.log`).
- **Setup (Linux)**: use the command `export SSLKEYLOGFILE=PATH_OF_FILE`.
- **Wireshark configuration**:
  1. Navigate to Edit → Preferences → Protocols → SSL (or TLS in version 3.0.0+).
  2. Set the file path in the (Pre)-Master-Secret log filename field.
- **Result**: encrypted TLS sessions will appear as plain text, such as standard HTTP data.

## 2. Decoding Malicious DNS Tunnels

- **Tunneling concept**: DNS tunneling encapsulates normal TCP traffic within DNS queries/responses to bypass security restrictions.
- **Red flags**: a PCAP may show numerous DNS query responses without corresponding requests, often using the TXT record to carry base64-encoded data.
- **Extraction with Scapy**: Scapy is a Python-based packet manipulation tool used to decode and forge packets.
- **Forensic process**: scripts can iterate through packets, filter by a specific Transaction ID (e.g., `0x1337`), extract the `rdata` field, and decode it from base64 to reveal system commands like `iwlist scan`. The script specifically checks for the DNSQR (DNS Query) packet type and that transaction ID before decoding.

## 3. Decrypting 802.11 (Wireless) Packets

**WEP decryption**

- WEP is a weak encryption standard where the key can often be found quickly using the Aircrack-ng suite.
- In Wireshark, add the found key under IEEE 802.11 Preferences → Decryption Keys to remove the wireless encapsulation and see the actual traffic.

**WPA/WPA2 decryption**

- Unlike WEP, WPA/WPA2 requires a four-way handshake (EAPOL) to be captured.
- The key is typically found through an offline brute-force dictionary attack using Aircrack-ng with the `-w` switch for a password list. The default dictionary file in Kali Linux for brute-forcing handshakes is located at `/usr/share/dict/words`.

## 4. Decoding Keyboard Captures (USB)

- **USB forensics**: when a USB keyboard is used, "leftover capture data" containing keystrokes can be found in a PCAP file.
- **Extraction**: use Tshark to filter for the `usb.capdata` field to harvest the raw hex bytes.
- **Mapping to text**: raw bytes are mapped to characters based on HID documentation (e.g., `09` = 'f', `0F` = 'l', `04` = 'a', `0a` = 'g').
- **Automation**: Python scripts using Scapy can automate this mapping to reconstruct what the user typed. A specific `sed` command is used to strip null bytes and colons when extracting: `sed -e 's/00//g' -e 's/://g' -e 's/20//g'`.
