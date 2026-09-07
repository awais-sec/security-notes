# Assignment 01 — Network Forensics CTF Challenges

*Network Forensics, submitted by Awais Ahmed, SP-23/BS DFCS/008.*

Three CTF-style network forensics challenges, each requiring reconstruction of a covert data-exfiltration channel from a packet capture to recover a flag.

## 1. NF_Challenge_01 — Sidebar / DNS TXT Beaconing

### What Happened (Reconstruction)

The workstation (internal host observed as `10.20.30.45`) loaded a malicious/ad landing resource from a CDN-like host (observed `cdn-updates.site` / `cdn-cache.updates-app.net` pattern). The chain included an HTML/docx (`SidebarUpdate.docx`-style) and a small binary stage (`win_update.exe`-style) retrieved from the CDN.

Shortly after, the host began issuing DNS queries to attacker-controlled domains under `telemetry.updates-app.net` using subdomains with numeric suffixes (`s01.telemetry.updates-app.net`, `s02.…`). The authoritative DNS server returned TXT records; each TXT record contained a small base64 fragment.

Concatenating the fragments in numeric order reconstructed a full base64 blob which decodes to `CTF{sidebar_dns_beacon}` — confirming DNS TXT-based beaconing (a covert channel).

### How I Knew / Evidence

- HTTP GETs to CDN hosts observed immediately before the DNS activity (timestamps in the pcap show the download, then the DNS queries).
- DNS responses of type TXT with `sNN.telemetry.updates-app.net` in `dns.qry.name` and base64-looking text in the answers. Fragments followed a clear numeric naming convention indicating ordering.
- Assembled base64 string + base64 decode → flag.

### Wireshark Steps Performed

1. Open `NF_Challenge_01.pcap`.
2. Filter: `http.request` — locate suspicious download URIs (look for `/dl/`, `/assets/`, `win_update.exe`, `SidebarUpdate.docx`). Use File → Export Objects → HTTP to save suspected files.
3. Filter: `dns && dns.resp.type == 16` (TXT records). Add columns: `dns.qry.name`, `dns.txt`.
4. Export displayed DNS packets: File → Export Packet Dissections → As CSV.
5. In a spreadsheet: extract the subdomain `sNN`, extract the TXT token, sort by numeric `sNN`, concatenate tokens, base64-decode. Result: `CTF{sidebar_dns_beacon}`.
6. Record packet numbers/timestamps of each fragment for the evidence chain.

### IOCs & Artifacts

- **Victim IP**: `10.20.30.45` (internal).
- **CDN/stager domains**: `cdn-updates.site`, `cdn-cache.updates-app.net` (example names from capture).
- **Beacon/C2 domain**: `telemetry.updates-app.net` (subdomains `s01..sNN` carrying TXT fragments).
- **Extracted artifact names**: `SidebarUpdate.docx`, `win_update.exe` (saved from HTTP objects). Compute hashes for these files and include in the final report.

## 2. NF_Challenge_02 — Helpdesk Email / Tracking-Pixel Exfil

### What Happened (Reconstruction)

A helpdesk-style email or web content caused the workstation to request small tracking pixels from a domain such as `img.helpdesk-tools.net`. Each pixel GET used a query string: `/pixel.gif?id=<base64_fragment>&p=<sequence_number>`.

The `id` query parameter contained base64 fragments and `p` was the sequence index. Assembling `id` fragments ordered by `p` and base64-decoding the result revealed the flag `CTF{helpdesk_pixel_beacon}`.

### How I Knew / Evidence

- HTTP GETs for `pixel.gif` with query strings were visible in the pcap. The query parameters consistently carried short base64-like strings and a `p` ordering parameter.
- Sorting by `p` and concatenating `id` values produced a valid base64 string which decodes to the flag.

### Wireshark Steps Performed

1. Open `NF_Challenge_02.pcap`.
2. Filter: `http.request.uri contains "pixel.gif"` to quickly find pixel GETs.
3. Add columns: `http.host`, `http.request.uri`. Verify GET → `/pixel.gif?id=...&p=...`.
4. Export displayed packets to CSV; extract `id` and numeric `p` from the URI.
5. In a spreadsheet: ensure `p` is sorted numerically, de-duplicate retries, concatenate `id` fragments, base64-decode. Result: `CTF{helpdesk_pixel_beacon}`.

### IOCs & Artifacts

- **Pixel domain**: `img.helpdesk-tools.net` (observed host).
- **URL pattern**: `/pixel.gif?id=<base64>&p=<seq>`.
- The method is a small-file HTTP covert channel (tracking pixels used as slice carriers).

## 3. NF_Challenge_03 — Noise-Heavy, ICMP/XOR Covert Channel

### What Happened (Reconstruction)

The capture contained many noisy flows but analysis revealed a covert fragment channel using marker bytes and sequence numbers. The implementer used a binary marker (`id=0xBEE5`), two terminator sequences (e.g., bytes `FB A2 97` / `FB A2 98`), and a fixed XOR key `0x37`. Payload slices were labeled with sequence numbers (2001–2020). Reassembling slices (applying XOR with `0x37` and ordering by sequence) recovered plaintext which contained the flag `CTF{icmp_xor_beacon}`.

### How I Knew / Evidence

- Repeating binary markers (`0xBEE5`) adjacent to small payload fragments and terminator bytes indicated a structured fragment protocol.
- Observed sequence number range 2001–2020 matched contiguous fragments.
- Applying `XOR(0x37)` to collected payload bytes produced readable segments that jointly contained the CTF token.

### Wireshark / Analysis Steps Performed

1. Open `NF_Challenge_03.pcap`. Use binary search for sequences containing bytes `\xBE\xE5` and following payload/terminators (in Wireshark: use Find Packet → Hex Value).
2. Extract payload bytes for each occurrence and note the sequence number encoded in the two bytes after `0xBE 0xE5`.
3. Export payload bytes (Follow → TCP/UDP/ICMP stream or export packet bytes), group by sequence number.
4. For each fragment: XOR with `0x37` and inspect the resulting ASCII. Concatenate fragments ordered by sequence number. The decoded string included the flag: `CTF{icmp_xor_beacon}`.

### IOCs & Artifacts

- **Marker/Protocol**: `0xBEE5` marker, terminators `FB A2 97` / `FB A2 98`, XOR key `0x37`.
- **Sequence range**: 2001–2020 (useful for detection: look for repeated packets containing `0xBE E5` and these sequence IDs).

## Timeline (Concise Cross-Challenge)

- **NF_Challenge_01**: initial HTTP download of a sidebar doc/exe → DNS TXT beaconing begins (`s01..sNN`) → flag extracted.
- **NF_Challenge_02**: email/web load → immediate pixel GETs carrying `id` fragments → fragments assembled → flag extracted.
- **NF_Challenge_03**: irregular/outbound noise → find repeating binary marker and sequence → fragments extracted, XOR decoded → flag extracted.

## Flags (Final)

1. NF_Challenge_01 → `CTF{sidebar_dns_beacon}`
2. NF_Challenge_02 → `CTF{helpdesk_pixel_beacon}`
3. NF_Challenge_03 → `CTF{icmp_xor_beacon}`
