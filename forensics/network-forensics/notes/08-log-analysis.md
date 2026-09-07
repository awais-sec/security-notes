# Log Analysis

## Common Log Sources

Investigators regularly turn to several different log types to build a picture of an incident:

- **DHCP Logs**: help map a specific IP address back to a physical device (via its MAC address) at a specific point in time, which is essential for attributing an action to a specific machine.
- **Web Proxy Logs**: record all web traffic passing through the proxy, revealing user browsing history, accessed URLs, and can help identify connections to malicious websites.
- **Firewall Logs**: show which connections were allowed or blocked by the firewall, providing a record of traffic attempting to cross network boundaries.

## Practical Application: Correlating DHCP and Proxy Logs

A key forensic technique is correlating data between different log sources to answer a specific question, such as "What websites did the user of a specific computer visit?"

**Process**

1. **Identify the target machine**: start with a known identifier, such as a hostname (e.g., "Bob-PC").
2. **Consult DHCP logs**: search the DHCP logs for that specific hostname to find the IP address that was assigned to it during the relevant time frame.
3. **Consult proxy logs**: take the identified IP address and use it to filter the web proxy logs.
4. **Result**: this filtering reveals all the specific websites and URLs that were accessed by that IP address (and therefore that machine) during the time it held that lease, effectively reconstructing the user's browsing activity for the investigation.
