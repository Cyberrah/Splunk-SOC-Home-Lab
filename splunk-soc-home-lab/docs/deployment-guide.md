# Deployment Guide

This guide assumes Kali Linux runs Splunk Enterprise and Windows runs Splunk Universal Forwarder in VirtualBox. Menu labels and IP addresses can vary by VirtualBox and operating system version.

## 1. Create a private VM network

For each VM, open **Settings → Network** and configure:

- **Adapter 1:** NAT (optional, for updates).
- **Adapter 2:** Host-only Adapter (for Windows-to-Kali telemetry).

Start both VMs and record the host-only address on each. On Windows run `ipconfig`; on Kali run `ip -br addr`. Identify the host-only interface/subnet from the VirtualBox Host Network Manager. The Windows `10.0.2.15` address is commonly its NAT-side address; do not use it as the Splunk receiver destination unless it is genuinely the Kali address (it should not be).

Check the path from Windows to Kali:

```powershell
Test-NetConnection -ComputerName <KALI_HOST_ONLY_IP> -Port 9997
```

Run the test after enabling the Splunk receiver. A successful result should show `TcpTestSucceeded : True`. If it fails, confirm both VMs have an address on the same host-only subnet and that Kali is listening on TCP 9997.

## 2. Start Splunk and enable receiving on Kali

Start Splunk Enterprise if it is not already running:

```bash
sudo /opt/splunk/bin/splunk start
sudo /opt/splunk/bin/splunk status
```

In Splunk Web, open **Settings → Forwarding and receiving → Configure receiving → New Receiving Port**, enter `9997`, and save. You can also inspect the example in `configs/kali-receiver-inputs.conf.example`; do not overwrite an existing `inputs.conf`. After a configuration change, restart Splunk if required.

Verify the listening port on Kali:

```bash
sudo ss -lntp | grep ':9997'
```

If Kali firewall rules are enabled, allow TCP 9997 only from the host-only lab subnet. Avoid exposing this port on a public or physical network interface.

## 3. Create the destination index

In Splunk Web, open **Settings → Indexes → New Index**, create an index named `wineventlog`, and save it before enabling Windows inputs. Alternatively, use the Splunk CLI on Kali:

```bash
sudo /opt/splunk/bin/splunk add index wineventlog -auth <admin>:<password>
```

Avoid putting real passwords in shell history; the Web interface is preferable for a one-time lab setup.

## 4. Install and configure the Windows Universal Forwarder

Install the Windows Universal Forwarder from Splunk's official download page. During setup, install it as a service and configure a deployment/receiving target if prompted. Use the Kali host-only IP as the receiving host and port `9997`.

Copy `configs/windows-uf-inputs.conf` to the forwarder's local app configuration directory, for example:

```text
C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf
```

Create `outputs.conf` from `configs/windows-uf-outputs.conf.example` at:

```text
C:\Program Files\SplunkUniversalForwarder\etc\system\local\outputs.conf
```

Replace `<KALI_HOST_ONLY_IP>` with the Kali host-only address. If your installation uses a dedicated app directory, keep the settings there instead of duplicating them in multiple places. Do not replace unrelated existing settings.

Restart the forwarder from an elevated PowerShell window:

```powershell
Restart-Service SplunkForwarder
Get-Service SplunkForwarder
```

The sample collects Security, System, and Application events, beginning with new events after deployment (`current_only=1`). To replay historical logs in a lab, change this intentionally and understand the resulting volume first.

## 5. Enable Windows logon auditing

On a disposable lab endpoint, open `secpol.msc` → **Advanced Audit Policy Configuration → System Audit Policies → Logon/Logoff → Audit Logon**. Enable both **Success** and **Failure**. Policy names and availability can differ by Windows edition. Use Group Policy results if a domain policy controls the endpoint.

Security log access is typically available to the Universal Forwarder service when installed as Local System. Follow your organization's least-privilege policy outside a lab.

## 6. Verify ingestion in Splunk

In Splunk Search, start with:

```spl
index=* earliest=-30m
| stats count by index sourcetype host
| sort - count
```

Then check the expected index and event codes:

```spl
index=wineventlog earliest=-30m
| stats count by sourcetype host EventCode
| sort - count
```

Inspect one raw Security event and its fields:

```spl
index=wineventlog earliest=-30m (EventCode=4624 OR EventCode=4625)
| head 20
```

If your environment uses `EventID` or a different field name, inspect the raw event and adjust the searches. Confirm the user and source IP fields available with:

```spl
index=wineventlog earliest=-30m EventCode=4625
| fieldsummary
```

## 7. Import the dashboard

In Splunk Web, use **Dashboards → Create New Dashboard → Source** and paste the XML in `dashboards/windows_auth_overview.xml`, or import it using the dashboard management interface available in your Splunk version. Replace the index name if needed.

## 8. Evidence checklist for your portfolio

Capture redacted screenshots showing: (1) both VMs on the host-only network, (2) receiver port enabled, (3) a successful Windows-to-Kali TCP test, (4) Security events in Splunk, and (5) a detection result. Blur usernames, machine names, domain names, IP addresses, and event details that identify you or others. Never publish secrets or real-world data.
