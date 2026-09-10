# Email and Social Media Forensics

*Module reference: CHFI (Computer Hacking Forensic Investigator) exam 312-49, Module: Email and Social Media Forensics. Purpose: train investigators in investigating email crimes and performing social media forensics.*

**Learning objectives:** understand email basics; explain email crime investigation and its steps; understand U.S. laws against email crime; explain social media forensics.

## LO#01: Understand Email Basics

### Introduction to Email Systems

- Email: electronic mail for sending, receiving, and storing messages
- Client-server architecture: clients send/receive via email servers
- **Components:**
  - Mail User Agent (MUA) — email client (e.g. Gmail, Outlook)
  - Mail Transfer Agent (MTA) — mail server (e.g. Sendmail, Postfix)
  - Mail Delivery Agent (MDA) — delivers email to the recipient's mailbox (e.g. Dovecot)
  - SMTP Server — outgoing mail server (port 25, 587 TLS, 465 SSL)
  - POP3 Server — retrieves emails (port 110), downloads to the local system
  - IMAP Server — manages emails on the server (port 143, 993 SSL), allows remote access

### How Email Communication Works

1. Sender (Bob) composes email via MUA
2. Email sent via SMTP to MTA
3. MTA routes email through multiple servers
4. Email delivered to MDA
5. Recipient (Alice) retrieves email via POP3/IMAP

### Parts of an Email Message

1. **Header** — To, Cc, Bcc, From, Subject, Date, Message-ID, Reply-To, Sender, MIME-Version, Priority, Received, Content-Type, Attachments; security headers: SPF, DKIM, DMARC
2. **Body** — main message (text, HTML, images, hyperlinks)
3. **Signature** — sender's contact details
4. **Attachments** — files sent with the email

## LO#02: Explain Email Crime Investigation and Its Steps

### Introduction to Email Crime Investigation

**Purpose:** examine the origin and content of offensive/spoofed emails.

**Types of email crimes:**
- Crimes committed by sending emails: spamming, phishing, mail bombing, mail storms, malware distribution
- Crimes supported by emails: identity theft, cyberstalking, fraud, narcotics trafficking

### Steps to Investigate Email Crimes

**1. Seize the computer and email accounts**
- Obtain a search warrant
- Seize computers and email accounts
- Change email passwords (with permission)
- Collaborate with corporate IT if the victim is an organization

**2. Acquire email data**
- Desktop-based clients: locate local email files (.pst, .ost, mbox)
- Web-based accounts: use credentials, Google Takeout, or sync with email clients
- Tools: SysTools MailPro+, Kernel for OST to PST

**3. Examine email messages**
- Subject: often creates urgency in spoofed emails
- Sender email address: check for authenticity (e.g. a bank using a Gmail address)
- Email body: look for suspicious links, poor language
- Attachments: check for malicious extensions (.exe, .vbs, .js, .zip)

**4. Retrieve email headers**
- Outlook: File → Properties → Internet headers
- Gmail: More → Show original
- Yahoo: More → View raw message
- Apple Mail: View → Message → All Headers

**5. Analyze email headers**
- Key fields: timestamp, From, To, Message-ID, Subject, MIME-Version
- Received headers: show the path of the email
- Return-Path: bounce address (differs in spoofing)
- Received-SPF: sender policy framework result (Pass/Fail/Neutral/Softfail)
- DKIM signature: email authentication (`v=1`, `a=rsa-sha256`, `d=domain`, `s=selector`, `bh=body hash`, `b=signature`)
- X-Headers: `X-Originating-IP`, `X-Mailer`, `X-Spam-Status`, etc.
- Tools for validation: Email Dossier, Hunter's Email Verifier, ZeroBounce
- Tracing origin: WHOIS (ARIN, RIPE, APNIC), IP2Location's Email Header Tracer

**6. Recover deleted email messages**
- Outlook: Deleted Items folder → Recover Deleted Items From Server
- Thunderbird: Trash folder recovery via forensic tools
- Gmail: Trash folder recovery
- Tools: Recover My eMail, Thunderbird Forensics Wizard, EaseUS Email Recovery Wizard

### Forensic Tools Mentioned

- SysTools MailPro+ — acquire, preview, export email data
- Kernel for OST to PST — convert .ost to .pst
- Autopsy & Paraben's E3 — recover deleted emails
- Recover My eMail & Thunderbird Forensics Wizard — email recovery tools
- EaseUS Email Recovery Wizard — recover Outlook PST files

## LO#03: Understand U.S. Laws Against Email Crime

### CAN-SPAM Act

- **Full name**: Controlling the Assault of Non-Solicited Pornography and Marketing Act
- **Purpose**: regulates commercial email, gives recipients the right to opt out
- **Requirements for senders**:
  - No false/misleading headers
  - No deceptive subject lines
  - Identify the email as an ad
  - Include a valid physical address
  - Provide opt-out instructions
  - Honor opt-out requests within 10 business days
- **Penalties**:
  - Fines up to $50,120 per violation
  - Criminal penalties for: unauthorized access to send spam; false registration of email/domain; relaying spam to mislead; email harvesting/dictionary attacks; using open relays/proxies without permission

## LO#04: Explain Social Media Forensics

### Introduction to Social Media Forensics

**Purpose:** identify, collect, preserve, and analyze social media artifacts for investigations.

**Artifacts collected:** images, videos, tweets, location tags, shared links; direct messages, group chats, comments; user profiles, friend lists, followers; metadata (timestamps, geolocation, device info).

### Social Media Crimes

1. **Photo morphing** — altering images to blackmail/embarrass
2. **Cyberstalking** — monitoring a victim's activity/location
3. **Cyberbullying** — posting falsified/discrediting content
4. **Identity theft** — using personal data for fraud
5. **Digital art theft** — illegally copying/distributing artwork
6. **Online scams** — fake ads, dating scams, job frauds
7. **Flash robs** — using social media to organize crimes

### Challenges in Social Media Forensics

- Large volume of data with edits/deletions
- Diverse data formats (text, images, videos)
- Strict privacy laws and legal authorization issues
- Dynamic evidence; chain of custody maintenance
- Cross-jurisdictional data collection
- Evolving platforms and security measures
- Use of anonymity services, encryption, steganography

### Data Collection Techniques

1. **Manual collection** — capture publicly available info (profiles, posts, comments); use screenshots and documentation
2. **Automated tools**:
   - WebPreserver — preserve web/social media pages as PDF/MHTML
   - Social Network Harvester (SNH) — automated evidence collection
3. **Extracting footage** — Save As option, Inspect tool, browser developer tools, VLC Media Player

### Tracking User Activities

- **Social Searcher** — real-time search across social media platforms (by username, hashtag, mentions)
- **Google Social Search** — integrated social media search

### Social Network Analysis

**Purpose:** understand relationships and interactions.

**Tools:** Gephi, SocNetV, NodeXL

**Steps:**
1. Collect data from social media
2. Identify nodes (individuals) and edges (relationships)
3. Use centrality measures (degree, betweenness, closeness)
4. Detect clusters/communities
5. Visualize the network for patterns

### Social Media Forensics Tools

- WebPreserver — evidence preservation
- Social Network Harvester (SNH) — automated data collection
- Gephi — network visualization and analysis
- Others: Cellebrite Pathfinder, PLX by PenLink, Jatheon, Hunchly, X1 Social Discovery

## Key Takeaways

- **Email forensics**: requires understanding of email systems, headers, and legal procedures
- **Social media forensics**: involves collecting digital artifacts from platforms, overcoming legal/technical challenges
- **Legal compliance**: adherence to laws like CAN-SPAM is critical
- **Tool proficiency**: investigators must be skilled with forensic tools for data acquisition, analysis, and recovery

---
*My notes from the Email and Social Media Forensics module.*
