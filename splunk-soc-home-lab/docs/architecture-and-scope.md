# Architecture and Scope

## Objective

Build a compact, repeatable SOC lab that shows the path from Windows telemetry to a Splunk alert and a documented analyst decision. The first iteration focuses on Windows authentication and Security log events.

## Components

| Component | Role | Lab notes |
|---|---|---|
| Kali Linux VM | Splunk Enterprise receiver and search head | Use the host-only address for forwarder traffic. Splunk Web normally uses TCP 8000; forwarder receiving normally uses TCP 9997. |
| Windows 10/11 VM | Endpoint telemetry source | Universal Forwarder collects selected Windows event logs and forwards them to Kali. |
| VirtualBox host-only network | Isolated telemetry path | Keep this interface private to the lab. NAT can provide outbound updates but is not the inter-VM receiver path. |
| Analyst browser | Search, dashboard, and triage | Access Splunk Web at `http://<KALI_HOST_ONLY_IP>:8000`. |

## Data flow

1. Windows writes authentication and audit activity to local Windows event logs.
2. Universal Forwarder reads the configured channels.
3. The forwarder sends events over TCP 9997 to Kali.
4. Splunk indexes events in `wineventlog`.
5. Analyst runs the detection searches and records alert evidence and disposition.

## Scope and guardrails

- Use only VMs and test accounts you control.
- Keep the host-only segment isolated from physical or public networks.
- Avoid automated password guessing. Use a few controlled test events and stop well before account lockout.
- Do not include credentials, unredacted logs, personal data, or externally routable IP addresses in a public repository.
- Treat all detections as educational prototypes; validate and tune before any operational use.

## Suggested baseline resources

Start with 2 vCPUs and 4 GB RAM for the Kali VM if available, and monitor disk use. Splunk indexing needs free disk space; avoid giving the lab VM more workload than the host can support. These are practical starting points, not formal product minimums.
