> You must be a *Security Administrator* for the tenant.

**Device discovery** *uses onboarded endpoints*, in your network to collect, probe, or scan your network to discover unmanaged devices. The device discovery capability allows you to discover:

- Enterprise endpoints (workstations, servers, and mobile devices)
- Network devices like routers and switches
- IoT devices like printers and cameras


---
## 🔍 Device Discovery Modes

### 1. Basic Discovery

- **Passive** method: uses `SenseNDR.exe`.
- No network traffic initiated.
- Collects device info from **observed** network events.
- **Limited visibility** into unmanaged devices.
- **Low impact** on network.

### 2. Standard Discovery (Recommended)

- **Active** and **passive** collection.
- Uses **multicast queries** and **probing** to find more devices.
- Enriches inventory with **deeper device insights**.
- **Minimal network activity** might be seen.
- Helps build a **more complete and accurate** device map.

>Devices that are **discovered** but **not yet onboarded** to MDE appear in the **device inventory** under the **Computers and Mobile** tab.

---
## 🖥️ Device Inventory

Use the `Onboarding status` filter to assess discovered devices:
- **Onboarded**: Device is fully integrated with Defender for Endpoint.
- **Can be onboarded**: OS is supported but not yet onboarded.
- **Unsupported**: OS is not compatible with Defender for Endpoint.
- **Insufficient info**: Device details are incomplete. Enable **Standard Discovery** to enrich data.
