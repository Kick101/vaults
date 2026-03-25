# Enable MDI

1. Create an instance on MDI management portal.
2. Provide an **on-prem AD service account**.
3. Download and install the sensor package.
4. Install sensors on **all Domain Controllers**.
5. _(Optional)_ Integrate VPN (RADIUS accounting).
6. Exclude **sensitive accounts** from monitoring.
7. Configure **SAM-R permissions** for the sensor.
8. Integrate with **Defender for Cloud Apps**.
9. _(Optional)_ Integrate with **Defender XDR**.

 >MDI sends only the parsed information to the Microsoft Defender for Identity cloud service (only a percentage of the logs are sent).

### MDI sensor Core functionalities

- Capture and inspect domain controller network traffic (local traffic of the DC)
- Receive Windows events directly from the DCs
- Receive RADIUS accounting information from your VPN provider
- Retrieve data about users and computers from the Active Directory domain
- Perform resolution of network entities (users, groups, and computers)
- Transfer relevant data to the Microsoft Defender for Identity cloud service

### ⚙️ Sensor Requirements

- **OS**:
    - WS 2008 R2 SP1 (not Core)
    - WS 2012 / 2012 R2 / 2016 / 2019 (Core ok, no Nano)
- **Hardware**:
    - ≥ 2 CPU cores, 6 GB RAM, 10 GB disk
- **Power plan**: High Performance
- **No support for dynamic memory** on VMs
- **Patch**: KB4487044 required on WS 2019

---
# Install MDI sensor

1. Download and extract the sensor file. Run `Microsoft Defender for Identity sensor setup.exe` and follow the setup wizard.
2. On the Welcome page, select your language and click **Next**.

![Install steps: Choose Language.](https://learn.microsoft.com/en-us/training/m365/m365-threat-safeguard/media/install-choose-language.png)

3. Server Type:
	- DC -> installs sensor
	- Dedicated Server -> installs standalone sensor

For example, for a MDI sensor, the following screen is displayed to let you know that a MDI sensor is installed on your dedicated server:

![Install steps: Determine server type.](https://learn.microsoft.com/en-us/training/m365/m365-threat-safeguard/media/install-server-type.png)
    
4. Under **Configure the sensor**, enter the installation path and the access key, based on your environment:
    
    - **Installation path**: By default, the path is `%programfiles%\Microsoft Defender for Identity sensor`
    - **Access key**: Retrieved from the MDI portal.

![Install steps: Configure the sensor.](https://learn.microsoft.com/en-us/training/m365/m365-threat-safeguard/media/install-configure-sensor.png)

5. Click **Install**.

### Configure MDI sensor settings

1. Sign into the MDI portal.
2. Go to **Configuration**. Under the System section, select **Sensors**.
3. Click on the sensor you want to configure and enter the following information:
    - **Description** (optional)
    - **Domain Controllers** (`FQDN`) (required for the MDI standalone sensor, this can't be changed for the MDI sensor)

>Use a standalone sensor when you can't install MDI directly on domain controllers. Instead, deploy it on a separate server and `port mirror DC traffic to it`.

[![Install steps: Select sensors in Microsoft Defender for Office 365 portal.](https://learn.microsoft.com/en-us/training/m365/m365-threat-safeguard/media/install-select-sensors.png)](https://learn.microsoft.com/en-us/training/m365/m365-threat-safeguard/media/install-select-sensors-magnify.png#lightbox)

The following information applies to the servers you enter in the DC list:

- All DCs monitored via port mirroring by the MDI standalone sensor must be listed in the Domain Controllers list.
- *At least one DC in the list should be a global catalog*. This enables MDI to resolve computer & user objects in other domains in the forest.
- **Capture Network adapters** (*required*):
	- For MDI sensors, all network adapters that are used for communication with other computers in your organization.
	- For MDI standalone sensor on a dedicated server, select the network adapters that are configured as the destination mirror port. These network adapters receive the mirrored domain controller traffic.


![Install steps: Enter information to configure sensor.](https://learn.microsoft.com/en-us/training/m365/m365-threat-safeguard/media/install-configure-sensor-info.png)

4. Click **Save**.
