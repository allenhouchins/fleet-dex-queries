# Fleet DEX Query Pack

Digital Employee Experience (DEX) query pack for [Fleet](https://fleetdm.com/) + osquery. Collects endpoint telemetry equivalent to commercial DEX platforms (Lakeside SysTrack, Nexthink, 1E) for feeding into a data lake and building Digital Experience dashboards.

## Quick Start

```bash
fleetctl apply -f dex-query-pack.yml
```

## Query Categories

| Category | Queries | Description |
|----------|---------|-------------|
| System Performance & Health | 9 | CPU, memory, disk, uptime, load averages, kernel panics, BSODs, unexpected shutdowns |
| Application Experience | 7 | Process resource usage, app crashes, energy impact, installed apps, browser extensions, startup items |
| Network Experience | 6 | Interface status, WiFi signal quality, active connections, DNS, VPN, listening services |
| User Sentiment Proxies | 4 | Responsiveness indicators, reboot frequency, sleep/wake failures, high resource processes |
| Hardware Inventory | 7 | System info, device details, battery health, USB devices, displays, disk drives, PCI devices |
| Security & Compliance | 21 | Encryption, firewall, SIP/Gatekeeper, AV, OS updates, user accounts, certificates, MDM, TPM, Secure Boot, services, XProtect |

**Total: 54 queries**

## Platform Coverage

| Platform | Coverage |
|----------|----------|
| macOS | Full — includes Apple Silicon detection, FileVault, SIP, Gatekeeper, XProtect, unified_log |
| Windows | Full — includes BitLocker, TPM, Secure Boot, Windows Events, performance counters, services |
| Linux | Core — system performance, hardware inventory, network, disk encryption, user accounts |
| ChromeOS | Partial — via osquery tables available on Chrome (network, system info) |

## Query Naming Convention

All queries follow the pattern:

```
DEX - {Category} - {Specific Metric}
```

Examples:
- `DEX - System Performance - CPU Utilization Snapshot`
- `DEX - Security Compliance - Disk Encryption Status`
- `DEX - Network Experience - WiFi Signal Quality`

## Dashboard Categories

These queries are designed to power dashboards across these dimensions:

- **Device Health Score** — CPU, memory, disk, battery, uptime, crashes
- **Application Performance** — resource hogs, crash rates, startup bloat
- **Network Quality** — WiFi signal, VPN connectivity, DNS health
- **Security Posture** — encryption, firewall, patching, admin accounts, MDM
- **Hardware Lifecycle** — model, age, battery degradation, drive health
- **User Experience Index** — composite of responsiveness, crashes, reboots, resource pressure

## Data Lake Integration

All queries use `logging: snapshot` mode, which means Fleet collects point-in-time results at each scheduled interval. The snapshot data can be forwarded to your data lake via:

- **Fleet log destinations** — S3, Kinesis, Lambda, Kafka, or filesystem
- **fleetctl** — export query results via API
- **Fleet REST API** — pull results programmatically

The consistent `DEX - Category - Metric` naming convention makes it straightforward to parse and route data into tables by category.

## Scheduling

Queries are not pre-scheduled in this pack. When applying via Fleet, configure intervals based on your needs:

| Use Case | Recommended Interval |
|----------|---------------------|
| Real-time dashboards | 60–300 seconds |
| Hourly health checks | 3600 seconds |
| Daily compliance audits | 86400 seconds |
| Weekly inventory | 604800 seconds |

## License

MIT
