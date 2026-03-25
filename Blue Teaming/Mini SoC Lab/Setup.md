### #0. Overview of the Lab
**VMs**
- pfSense
- kali
- SecurityOnion
- Windows
- Linux (optional)

**Network subnets**

| **Network Name** | **Machine**    | **Subnet**        |
| ---------------- | -------------- | ----------------- |
| Green            | Windows, Linux | `10.10.10.100/24` |
| Blue             | Security Onion | `10.10.20.100/24` |
| Red              | Kali           | `10.10.30.100/24` |
| VPN              | Host machine   | `10.10.3.2/24`    |
![[soc lab.png | 500]]

**System resources**
> We need not run all VMs at once. We can connect via VPN and run attacks from our host machine without the need of kali.

| VM              | RAM (GB) | CPU Cores | ROM (GB)      |
| --------------- | -------- | --------- | ------------- |
| SecurityOnion   | 8        | 4         | 90            |
| Kali            | 3        | 2         | VM image (80) |
| Windows 10 LTSC | 2        | 2         | 25            |
| pfSense         | 1        | 1         | 20            |

**My setup**
- Host machine OS: ArchCraft
- RAM: 14 Gigs
- Processor: Ryzen 5 (8 cores)
- Hypervisor: virt-manager/KVM
- ROM: 256 SSD

#### ⚠️ Notes from my side
> I suggest you to go through the full setup notes before proceeding, it could clear any doubts when setting it up.
>
> Sometimes, all configuration looks good, things don't work as they should be, just restart the VMs (literally) - that works!
>
> I enabled ssh on all VMs for easy troubleshooting. 

---
### #1 Networking

#### Assign these bridges to your VMs
Use the bridge setup script: [[#3 Scripts to make life easy]]

- **pfSense VM** interfaces:
    - WAN -> `NAT` to host machine
    - LAN → `br-green`
    - OPT1 → `br-blue`
    - OPT2 → `br-red`
    - OPT3 -> `VPN`
    - OPT4 -> `br-mon` (SPAN port)

#### Configure pfSense
- DHCP : To assign IPs automatically to the connected VMs
- OpenVPN : To access VMs without NAT-ing to host machine.
- DNS : Minimum host resolution config
- Firewall : To allow VPN clients 

#### Interfaces
- **OPT5, OPT3** are assigned but not enabled.

![[Pasted image 20250603201544.png]]

**Example**
- I chose same mac address as *Network port* above in configuring **GREEN** interface

![[Pasted image 20250603202321.png]]

##### Configure Mirroring/SPAN port on pfSense

- Rename one of the interfaces as Mirror and connect it to bridge `br-mon`.

![[Pasted image 20250603003044.png]]

- Go to : **Interfaces -> Bridges**.
- Click on *Advanced Options*.

![[Pasted image 20250603003006.png]]

### Firewall Rules
- I gave VPN access to all subnets, so I can ssh and troubleshoot easily.

**GREEN**

![[Pasted image 20250603200406.png]]

**BLUE**

![[Pasted image 20250603200428.png]]

**RED**

![[Pasted image 20250603234012.png]]

**Mirror**

![[Pasted image 20250603200822.png]]


#### VPN

- When setting up your VPN make sure you have a setting to access internal network subnets. Configure it in VPN servers.
- Go to : **VPNOpen -> VPN Servers -> Edit**

![[Pasted image 20250603202945.png]]

---
## #2 Security Onion
- There are a few caveats to be addressed in our setup.
- Make sure you have network *mirroring* working.

#### Iptables
- This one took me literally 2 days, I think, to get it right, because I tried messing with iptables manually from cli. It was a pain.
- Stop iptables:

```bash
sudo systemctl stop iptables
```

- Now you can access the soc platform on the host machine: `10.10.20.100` (I also reserved static ip on the pfsense via **DHCP Static Mappings**)
- Follow this guide: https://docs.securityonion.net/en/2.4/firewall.html#configuring-host-firewall
- For some reason after starting iptables, if you still don't access, restart the VM.


#### Test suricata alert
- Create ICMP test alert

![[Pasted image 20250603215158.png]]

- ping from a red machine to green machine. *It can take few mins before it shows up in alerts.*
```bash
ping -c4 -4 10.10.10.100 # from kali
```

![[Pasted image 20250604001304.png]]


---
## #3 Scripts to make life easy

I have these script in `~/.custom-scripts-bin`. I also added that path to `$PATH` in `.zshrc`

- **pfsense_vm**  : start pfsense vm from cli                     
- **setup_bridges_soc** : creates and runs bridges
- **kali_vm** : start kali vm from cli      
- **SecurityOnion** : runs setup_bridges_soc, pfsense_vm, start securityonion from cli
- **connect_VPN** : simple script to connect to vpn

__setup_bridges_soc__

```bash
#!/bin/bash

# List of bridge names
BRIDGES=("br-green" "br-blue" "br-red" "br-mon")

for BR in "${BRIDGES[@]}"; do

    # Check if bridge already exists
    if ip link show "$BR" &>/dev/null; then
        echo "  $BR already exists, skipping."
        continue
    fi

    # Create the bridge
    sudo ip link add name "$BR" type bridge

    # Bring the bridge up
    sudo ip link set "$BR" up

done

# Really important for traffic to reach VMs
# This is a hack. Shouldn't be used in production. Use openVswitch instead.
sudo brctl setageing br-mon 0
sudo brctl setfd br-mon 0
```

__kali_vm__
- vm name should be `kali`
```bash
#!/usr/bin/bash

virsh --connect qemu:///system start kali
```

__pfsense_vm__
- vm name should be `pfsense`
```bash
#!/usr/bin/bash

setup_bridges_soc

virsh --connect qemu:///system start pfSense
```

__SecurityOnion__
- vm name should be `SecurityOnion`
```bash
#!/usr/bin/bash

setup_bridges_soc

pfsense_vm

virsh --connect qemu:///system start SecurityOnion
```

__PATH__
```bash
export PATH=$PATH:$HOME/.custom-scripts-bin/
```
