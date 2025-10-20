# Task 1 — Local Network Port Scan

## Objective
Discover open ports in the local network using Nmap and analyze exposure.

## Scan details
- Network scanned: `192.168.11.0/24`
- Command used: `sudo nmap -sS -T4 --top-ports 100 --min-rate 500 192.168.11.0/24 -oN scan_results.txt`
- Scan file: `scan_results.txt`

## Findings

| IP Address       | Open Port(s) | Service         | Notes |
|------------------|--------------|------------------|-------|
| 192.168.11.2     | 53/tcp       | DNS (domain)     | DNS service running — normal on LAN |
| 192.168.11.1     | none (filtered) | -             | Packets filtered (firewall/router) |
| 192.168.11.254   | none (filtered) | -             | Packets filtered |
| 192.168.11.128   | none (closed) | -              | No listening services on scanned ports |

*(Only hosts with ports in the top-100 list were reported.)*

## Analysis & Risk Assessment
- **DNS (53/tcp)** on `192.168.11.2`:
  - **What it is:** DNS resolves names to IPs; commonly run on routers or dedicated DNS servers.
  - **Potential risks:** If the DNS service is misconfigured or accessible from the internet it can be abused (DNS amplification or info leakage). On a private LAN this is usually expected and low-risk.
  - **Mitigations:** ensure the DNS server is only accessible to the LAN (no public exposure), run up-to-date software, restrict recursion if not needed, and monitor logs for abuse.

- **Filtered hosts** (`192.168.11.1`, `192.168.11.254`):
  - Likely a router or VM host with firewall rules. Filtered = firewall is blocking probes → good for reducing exposure.

## Tools used
- Nmap (scan output: `scan_results.txt`)
- (Optional) Wireshark for packet capture: `scan_capture.pcapng` (if captured)

## Files to submit
- `scan_results.txt` (or redacted copy)
- `scan_results.html` (if created via xsltproc)
- `README.md`
