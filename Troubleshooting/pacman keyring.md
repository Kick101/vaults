```bash
sudo fuser -v /etc/pacman.d/gnupg
```

```txt
                     USER        PID ACCESS COMMAND
/etc/pacman.d/gnupg: root     kernel mount /etc/pacman.d/gnupg
```

```bash
sudo umount -l /etc/pacman.d/gnupg
sudo rm -rf /etc/pacman.d/gnupg/
sudo pacman-key --init
sudo pacman-key --populate archlinux
```
