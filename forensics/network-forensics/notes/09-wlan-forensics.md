# WLAN (Wireless) Forensics

## Overview

WLAN forensics focuses on capturing and analyzing wireless traffic, which comes with its own unique protocols, security mechanisms, and investigative challenges compared to wired networks.

## Capturing Wireless Traffic

- **Monitor Mode**: to capture all wireless traffic in the air (not just traffic destined for your own device), a wireless network adapter must be placed into a special "monitor mode." This is a prerequisite for any serious wireless packet capture and analysis.
- **802.11 Frame Types**: wireless traffic consists of several distinct frame types beyond just data:
  - **Management Frames**: used to establish and maintain communication, such as beacon frames (which advertise a network's presence, SSID, and capabilities), and authentication/association frames (used when a client connects to an access point).
  - **Control Frames**: help manage access to the shared wireless medium, coordinating the delivery of data frames and reducing collisions.
  - **Data Frames**: carry the actual payload of information (e.g., the web traffic, emails, etc.) being sent between the client and the access point.

## Analyzing Encrypted Wireless Traffic

- **The core challenge**: WPA/WPA2-encrypted traffic is unreadable without the correct decryption key.
- **The solution**: as covered in the encryption chapter, an investigator must first capture the initial four-way EAPOL handshake between the client and the access point. With this handshake captured, tools like Aircrack-ng can be used to perform an offline dictionary attack to recover the pre-shared key (PSK).
- **The result**: once the key is found, it can be added to Wireshark's 802.11 decryption preferences, allowing the previously encrypted data frames to be decrypted and analyzed as plain, readable traffic (e.g., viewing the actual HTTP requests a client made over the wireless network).
