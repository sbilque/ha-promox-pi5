# Install MQTT

## Installation

1. Exécutez la commande :

   ````bash
   bash -c "$(wget -qLO 1. https://github.com/community-scripts/ProxmoxVE/raw/main/ct/mqtt.sh)"
   ````

1. Utilisez le mode Advanced

   ````bash
    Using Advanced Settings
    Using Distribution: debian
    Using debian Version: 12
    Using Container Type: 1
    Using Root Password: ********
    Container ID: 101
    Using Hostname: mqtt-proxmox
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
    ✓ Network Connected: 192.168.1.200 2a01:cb00:1387:8e01:be24:11ff:fe78:d0df
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

## Création de l'utilisateur z2m-proxmox

1. Mosquitto est fourni avec un outil de création de mot de passe.
1. Cliquez sur le container MQTT puis ouvrez `>_ Console`
1. Exécutez la commande :

    ````bash
    sudo mosquitto_passwd -c /etc/mosquitto/passwd z2m-proxmox
    ````

1. Saisissez le mot de passe deux fois.

    ```bash
    root@mqtt:~#
    sudo mosquitto_passwd -c /etc/mosquitto/passwd z2m-proxmo
    Password:
    Reenter password:
    ````

1. Assurez vous que le fichier passwd possède les droits nécessaire avec la commande :

    ````bash
    chown mosquitto:mosquitto /etc/mosquitto/passwd
    ````

1. Crée le fichier de configuration pour que Mosquitto utilise le fichier de mot de passe que vous venez de créer.

    ````bash
    sudo vim /etc/mosquitto/conf.d/default.conf
    ````

1. Assurez vous que le fichier `default.conf` contient les lignes suivantes :

    ````bash
    allow_anonymous false
    persistence true
    password_file /etc/mosquitto/passwd
    listener 1883
    ````

1. Tapez `ESC` puis `i` pui éditez le texte, puis `ESC`+`:qw` pour sauver les modifications puis quitter `vim`.

1. Redémarrez Mosquitto avec la commande :

    ````bash
    sudo systemctl restart mosquitto
    ````

1. A partir de ce moment, votre serveur MQTT fonctionne et écoute le port 1883 sur l'adresse du container. Il utilise le compte `z2m-promox`.

## Mise à jour

1. Depuis Proxmox, allez dans `Datacenter > Pimox5 > mqtt-proxmox` puis `>_ Console`
2. Authentifiez vous avec le compte `root`
3. Exécutez la commande :

    ````bash
    update
    ````

4. Choisissez l'option `1 YES (Silent Mode)`

    ```bash
        __  _______  ____________
      /  |/  / __ \/_  __/_  __/
      / /|_/ / / / / / /   / /
    / /  / / /_/ / / /   / /
    /_/  /_/\___\_\/_/   /_/


      ✔️  Updated Successfully
    root@mqtt:~#
    ```

## Références

- [How to Separate Zigbee2MQTT from Home Assistant in Proxmox](https://smarthomescene.com/guides/how-to-separate-zigbee2mqtt-from-home-assistant-in-proxmox/)