# BORN 2 BE ROOT

### Puertos y servicios:
 - 4242: ssh
 - 80: wordpress
 - 7080: admin panel OpenLiteSpeed
 - 8088: demo OpenLiteSpeed

### Script:
``` bash
    #!/bin/bash

    # ARCH
    arch=$(uname -a)

    # CPU PHYSICAL
    cpuf=$(lscpu | grep "Socket(s):" | wc -l)

    # CPU VIRTUAL
    cpuv=$(nproc --all)

    # RAM
    ram_total=$(awk '/MemTotal/ {printf "%.0f", $2/1024}' /proc/meminfo)
    ram_use=$(awk '/MemAvailable/ {printf "%.0f", ($2/1024)}' /proc/meminfo)
    ram_percent=$((100 - ram_use * 100 / ram_total))

    # DISK
    disk_total=$(df -BG --total | grep total | awk '{print $2}')
    disk_use=$(df -BG --total | grep total | awk '{print $3}')
    disk_percent=$(df --total | awk '/total/ {print $5}')

    # CPU LOAD
    cpul=$(vmstat 1 2 | tail -1 | awk '{printf $15}')
    cpu_op=$(expr 100 - $cpul)
    cpu_fin=$(printf "%.1f" $cpu_op)

    # LAST BOOT
    lb=$(uptime -s)

    # LVM USE
    lvmu=$(if [ $(lsblk | grep "lvm" | wc -l) -gt 0 ]; then echo yes; else echo no; fi)

    # TCP CONNEXIONS
    tcpc=$(ss -nt | grep ESTAB | wc -l)

    # USER LOG
    ulog=$(who | wc -l)

    # NETWORK
    ip=$(hostname -I)
    mac=$(cat /sys/class/net/$(ip route get 1 | sed -n 's/^.*dev \([^ ]*\).*$/\1/p')/address)

    # SUDO
    cmnd=$(sudo grep COMMAND /var/log/auth.log | wc -l)

    wall "    Architecture: $arch
        CPU physical: $cpuf
        vCPU: $cpuv
        Memory Usage: $ram_use/${ram_total}MB ($ram_percent%)
        Disk Usage: $disk_use/${disk_total} ($disk_percent)
        CPU load: $cpu_fin%
        Last boot: $lb
        LVM use: $lvmu
        Connections TCP: $tcpc ESTABLISHED
        User log: $ulog
        Network: IP $ip ($mac)
        Sudo: $cmnd cmd"
```

### Users and Passwds:

- Root: slegaris42:*******
- Mainusr: slegaris:42madrid.com
- Decrypt: 42madridHelloWorld
