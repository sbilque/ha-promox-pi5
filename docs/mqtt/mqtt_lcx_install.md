# Install MQTT

## Installation

- Exécutez la commande :
  
    ````bash
    bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/mqtt.sh)"
    ````

- Utilisez le mode Advanced

  ````bash
    Using Advanced Settings
    Using Distribution: debian
    Using debian Version: 12
    Using Container Type: 1
    Using Root Password: ********
    Container ID: 101
    Using Hostname: mqtt
    Using Disk Size: 2
    Allocated Cores: 1
    Allocated RAM: 512
    Using Bridge: vmbr0
    Using IP Address: dhcp
    Using Gateway IP Address: Default
    Using APT-Cacher IP Address: Default
    Disable IPv6: no
    Using Interface MTU Size: Default
    Using DNS Search Domain: Host
    Using DNS Server IP Address: Host
    Using Vlan: Default
    Enable Root SSH Access: no
    Enable Verbose Mode: no
    Creating a MQTT LXC using the above advanced settings
    ✓ Using local for Template Storage.
    ✓ Using local for Container Storage.
    ✓ Downloaded LXC Template
    ✓ LXC Container 101 was successfully created.
    ✓ Started LXC Container
    ✓ Set up Container OS
    ✓ Network Connected: 192.168.1.57 2a01:cb00:1387:8e00:be24:11ff:fe15:d0df 
    ✓ IPv4 Internet Connected
    ✓ IPv6 Internet Connected
    ✓ DNS Resolved github.com to 140.82.121.4
    ✓ Updated Container OS
    ✓ Installed Dependencies
    ✓ Installed Mosquitto MQTT Broker
    ✓ Cleaned
    ✓ Completed Successfully!

    root@pimox5:~# ^C
    ````

    ````bash
  Connected to tty 1
    Type <Ctrl+4 q> to exit the console, <Ctrl+4 Ctrl+4> to enter Ctrl+4 itself

    Debian GNU/Linux 12 mqtt tty1

    mqtt login: root
    Password: 
    MQTT LXC provided by https://pimox-scripts.com/

    root@mqtt:~# sudo mosquitto_passwd -c /etc/mosquitto/passwd root
    Password: 
    Reenter password: 
    root@mqtt:~# chown mosquitto:mosquitto /etc/mosquitto/passwd
    root@mqtt:~# sudo vim /etc/mosquitto/conf.d/default.conf
    root@mqtt:~# sudo systemctl restart mosquitto
    ````

## Références

- [How to Separate Zigbee2MQTT from Home Assistant in Proxmox](https://smarthomescene.com/guides/how-to-separate-zigbee2mqtt-from-home-assistant-in-proxmox/)