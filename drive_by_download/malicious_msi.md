Phishing Investigation – Incident Report
This repository contains documentation and analysis related to a phishing incident involving malicious domains, a fraudulent Adobe update prompt, and a suspicious MSI payload. The investigation was conducted from a blue‑team security operations perspective.

📌 Overview
A user inadvertently accessed a malicious website that delivered a drive‑by download under the guise of an Adobe Reader update. Security controls in Edge initially blocked the website, while Chrome permitted access, leading to the execution of malicious redirects and an automatic file download.
This README documents the investigation steps, findings, IOCs, and remediation actions.

🔍 Investigation Summary
1. User Interaction & Browser Behavior
Microsoft Edge blocked access to:
https[:]//extstudios[.]com/projectbid
Google Chrome allowed the same URL, enabling further malicious activity.

2. Redirect & Payload Delivery
Accessing the site in Chrome redirected the user to:
https[:]//anydesignpoint[.]com/projectview/
The page displayed a fake “Adobe Acrobat Viewer Update” popup, triggering an automatic MSI download without user consent.

3. Malicious File Details
Downloaded File:
Filename: Adobe-Reader-installer-64.msi
SHA-256: 82085737c91fc2b6d669eace915c8dc7d11718210f8e6e5d9e59a2abb6ce3b45
SHA-1:   69ade388cabf3c7b5b48ad85cc2b13f2a2211822

VirusTotal Analysis:
5 / 62 security vendors flagged the file as malicious
Behavior is consistent with drive‑by malware delivery

4. Network Telemetry (Splunk)
DNS logs confirmed that outbound queries to the malicious domain were allowed:
123.456.789.101 → anydesignpoint[.]com

5. Endpoint Telemetry (SentinelOne)
Searches for related DNS and URL activity returned no matching events, indicating:
The malicious file likely did not execute, or
The agent did not detect/report activity related to the domains

Query used:
event.dns.request contains "anydesignpoint[.]com"OR url.address contains "anydesignpoint[.]com"

6. Domain Background
Both domains appear recently registered and strongly associated with malicious infrastructure:
DomainRegistration Dateanydesignpoint[.]com2025‑12‑20extstudios[.]com2026‑01‑03
No business purpose exists for either domain.

🛡️ Remediation & Containment
✔ Domain Blocking
Both domains were blocked at the network/security stack due to confirmed malicious activity and no business relevance.
✔ File Hash Blacklisting
The SHA‑256 hash of the malicious MSI was added to SentinelOne’s blacklist, preventing execution across all endpoints.

📁 Contents of This Repository

📎 Indicators of Compromise (IOCs)
Domains
extstudios[.]com
anydesignpoint[.]com

File Hashes
SHA‑256: 82085737c91fc2b6d669eace915c8dc7d11718210f8e6e5d9e59a2abb6ce3b45
SHA‑1:   69ade388cabf3c7b5b48ad85cc2b13f2a2211822
