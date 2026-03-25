
| Setting           | Value | Effect             | Why Use It                          |
| ----------------- | ----- | ------------------ | ----------------------------------- |
| `brctl setageing` | 0     | Disables MAC aging | Retains all MACs, good for sniffers |
| `brctl setfd`     | 0     | Zero forward delay | Sniff traffic instantly             |

**setageing** -- **0 seconds**, which means:
- The bridge will **not remember** any MAC addresses.
- It will **flood** all frames to **all ports** — including the one your Security Onion VM is on.
- This is essential when your monitoring interface (mirror port) is receiving traffic **not destined** to it — like a promiscuous sensor needs.

**setfd**
- Frames are forwarded **immediately**.
- Normally, STP delays this by ~15 seconds to prevent loops, but in a dedicated sniffing setup (e.g., Security Onion), **looping is not your concern** — performance and real-time visibility are.


> Notice ageing time and bridge forward delay

```txt
   ~ sudo brctl show
bridge name     bridge id               STP enabled     interfaces
br-blue         8000.720fe83d09f0       no              vnet3
                                                        vnet7
br-green        8000.c200b69fbb93       no              vnet1
                                                        vnet6
br-mon          8000.fe25c80003d9       no              vnet10
                                                        vnet5
                                                        vnet8
br-red          8000.f6d1e32cf17c       no              vnet4
                                                        vnet9
virbr0          8000.525400e4377a       yes             vnet2
   ~ sudo brctl setageing br-mon 0
   ~ sudo brctl setfd br-mon 0
   ~ sudo brctl show
bridge name     bridge id               STP enabled     interfaces
br-blue         8000.720fe83d09f0       no              vnet3
                                                        vnet7
br-green        8000.c200b69fbb93       no              vnet1
                                                        vnet6
br-mon          8000.fe25c80003d9       no              vnet10
                                                        vnet5
                                                        vnet8
br-red          8000.f6d1e32cf17c       no              vnet4
                                                        vnet9
virbr0          8000.525400e4377a       yes             vnet2
   ~ sudo brctl showstp br-mon
br-mon
 bridge id              8000.fe25c80003d9
 designated root        8000.fe25c80003d9
 root port                 0                    path cost                  0
 max age                  20.00                 bridge max age            20.00
 hello time                2.00                 bridge hello time          2.00
 forward delay             0.00                 bridge forward delay       0.00
 ageing time               0.00
 hello timer               0.00                 tcn timer                  0.00
 topology change timer     0.00                 gc timer                   0.00
 flags


vnet10 (3)
 port id                8003                    state                forwarding
 designated root        8000.fe25c80003d9       path cost                  2
 designated bridge      8000.fe25c80003d9       message age timer          0.00
 designated port        8003                    forward delay timer        0.00
 designated cost           0                    hold timer                 0.00
 flags

vnet5 (1)
 port id                8001                    state                forwarding
 designated root        8000.fe25c80003d9       path cost                  2
 designated bridge      8000.fe25c80003d9       message age timer          0.00
 designated port        8001                    forward delay timer        0.00
 designated cost           0                    hold timer                 0.00
 flags

vnet8 (2)
 port id                8002                    state                forwarding
 designated root        8000.fe25c80003d9       path cost                  2
 designated bridge      8000.fe25c80003d9       message age timer          0.00
 designated port        8002                    forward delay timer        0.00
 designated cost           0                    hold timer                 0.00
 flags

   ~ sudo brctl showstp br-red
br-red
 bridge id              8000.f6d1e32cf17c
 designated root        8000.f6d1e32cf17c
 root port                 0                    path cost                  0
 max age                  20.00                 bridge max age            20.00
 hello time                2.00                 bridge hello time          2.00
 forward delay            15.00                 bridge forward delay      15.00
 ageing time             300.00
 hello timer               0.00                 tcn timer                  0.00
 topology change timer     0.00                 gc timer                   7.81
 flags


vnet4 (1)
 port id                8001                    state                forwarding
 designated root        8000.f6d1e32cf17c       path cost                  2
 designated bridge      8000.f6d1e32cf17c       message age timer          0.00
 designated port        8001                    forward delay timer        0.00
 designated cost           0                    hold timer                 0.00
 flags

vnet9 (2)
 port id                8002                    state                forwarding
 designated root        8000.f6d1e32cf17c       path cost                  2
 designated bridge      8000.f6d1e32cf17c       message age timer          0.00
 designated port        8002                    forward delay timer        0.00
 designated cost           0                    hold timer                 0.00
 flags

```



---
## Resources
http://www.ryanhallman.com/kvm-configure-mirrored-ports-traffic-to-be-visible-in-guest-snort/
