# Install Z2M

## Installation

1. Pour créer un nouveau container LXC Zigbee2MQTT dans Proxmox VE, exécutez la commande suivante dans le shell de Proxmox VE :

   ````bash
   bash -c "$(wget -qLO - https://github.com/asylumexp/Proxmox/raw/main/ct/zigbee2mqtt.sh)"
   ````

1. Le script démarre l'installation de Zigbee2MQTT.
1. Répondez `<Yes>` à la question de la création d'un nouveau Zigbee2MQTT LXC.
1. Choisissez `<No>` pour l'envoi de données de diagnostics LXC.
1. Dans `Settings`, choisissez `3 Advanced Settings`.
1. Dans `Distribution`, choisissez `debian`.
1. Dans `Debian version`, choisissez `12 bookworm`.
1. Dans `Container type`, choisissez `0 Privileged`.
1. Dans `Password (leave black for automatic login)` et `Password verification`, saisissez le mot de passe du compte `root` pour ce container.
1. Dans `Container ID`, laissez l'ID proposé.
1. Dans `Hostname`, laissez le nom proposé `zigbee2mqtt` par défaut ou changer le (par exemple `z2m-proxmox`).
1. Dans `Disk size`, laissez `4` GB.
1. Dans `Core count`, laissez `2` CPU cores.
1. Dans `RAM`, laissez `1014` MiB.
1. Dans `Bridge`, laissez `vmbr0`.
1. Dans `IP address`, laissez `dhcp`.
1. Dans `APT-CAcher IP`, laissez à vide.
1. Dans `IPv6`, sélectionnez `<No>` pour ne pas désactiver IPv6.
1. Dans `MTU size`, laissez `1500`.
1. Dans `DNS Search Domain`, laissez à vide pour garder HOST.
1. Dans `DNS Server IP`, laissez à vide pour garder HOST.
1. Dans `MAC address`, laissez à vide pour générer une adresse MAC.
1. Dans `VLAN`, laissez à vide pour spécifier que vous n'utilisez pas de VLAN.
1. Dans `Advanced tags`, laissez la valeur par défaut (`community-script;smarthome;zigbee;mqtt`).
1. Dans `SSH Access`, laissez `No` pour ne pas activer le Root SSH Access.
1. Dans `Verbose mode`, laissez `No` pour ne pas activer le mode verbeux.
1. Dans `Advanced settings complete`, choisissez `<Yes>`.
1. Le script affiche un résultat de la forme suivante :

    ````bash
    _____   _       __             ___   __  _______  ____________
    /__  /  (_)___ _/ /_  ___  ___ |__ \ /  |/  / __ \/_  __/_  __/
    / /  / / __ `/ __ \/ _ \/ _ \__/ // /|_/ / / / / / /   / /
    / /__/ / /_/ / /_/ /  __/  __/ __// /  / / /_/ / / /   / /
    /____/_/\__, /_.___/\___/\___/____/_/  /_/\___\_\/_/   /_/
        /____/
    🧩  Using Advanced Settings on node pimox5
    🖥️  Operating System: debian
    🌟  Version: 12
    📦  Container Type: Privileged
    🔐  Root Password: ********
    🆔  Container ID: 102
    🏠  Hostname: z2m-proxmox
    💾  Disk Size: 4 GB
    🧠  CPU Cores: 2
    🛠️  RAM Size: 1024 MiB
    🌉  Bridge: vmbr0
    📡  IP Address: dhcp
    🌐  Gateway IP Address: Default
    📡  APT-Cacher IP Address: Default
    🚫  Disable IPv6: no
    ⚙️  Interface MTU Size: Default
    🔍  DNS Search Domain: Host
    📡  DNS Server IP Address: Host
    🏷️  Vlan: Default
    📡  Tags: community-script;smarthome;zigbee;mqtt
    🔑  Root SSH Access: no
    🔍  Verbose Mode: no
    🚀  Creating a Zigbee2MQTT LXC using the above advanced settings
    ✔️  Using local for Template Storage.
    ✔️  Using local for Container Storage.
    ✔️  LXC Template is ready to use.
    ✔️  LXC Container 102 was successfully created.
    ✔️  Started LXC Container
    ✔️  Set up Container OS
    ✔️  Network Connected: 192.168.1.50 2a01:cb00:1387:8e00:be24:12ff:fed9:7015
    ✔️  IPv4 Internet Connected
    ✔️  IPv6 Internet Connected
    ✔️  DNS Resolved github.com to 140.82.121.3
    ✔️  Updated Container OS
    ✔️  Installed Dependencies
    ✔️  Set up Node.js Repository
    ✔️  Installed Node.js
    ✔️  Installed pnpm
    ✔️  Installed Zigbee2MQTT
    ✔️  Created Service
    ✔️  Cleaned
    ✔️  Completed Successfully!

    🚀  Zigbee2MQTT setup has been successfully initialized!
    💡   Access it using the following URL:
        🌐  http://192.168.1.201:9442
    ````

## Configuration

1. Allez dans le container puis ouvrez `>_ Console`.
1. Connectez avec le mot de passe de root pour ce container.

    ````bash
    Connected to tty 1
    Type <Ctrl+4 q> to exit the console, <Ctrl+4 Ctrl+4> to enter Ctrl+4 itself

    Debian GNU/Linux 12 z2m-proxmox tty1

    z2m-proxmox login: root
    Password:

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.

    Zigbee2MQTT LXC Container
        🌐   Provided by: community-scripts & pimox-scripts | GitHub: https://github.com/asylumexp/ProxmoxVE

        🖥️   OS: Debian GNU/Linux - Version: 12
        🏠   Hostname: z2m-proxmox
        💡   IP Address: 192.168.1.201
    root@z2m-proxmox:~
    ````

1. Editez le fichier `configuration.yaml` avec la commande :

   ````bash
   vim /opt/zigbee2mqtt/data/configuration.yaml
   ````

1. Modifiez le contenu pour ressemble à ce contenu. Modifiez l'adresse IP de votre slzb et du serveur MQTT.

    ````yaml
    version: 4
    permit_join: true
    frontend:
      enabled: true
      port: 9442
    homeassistant:
      enabled: true
    mqtt:
      base_topic: zigbee2mqtt
      # Emplacement de MQTT
      server: 'mqtt://mqtt-proxmox.home:1883'
      user: z2m-proxmox
      password: ***REDACTED***
      keepalive: 60
      reject_unauthorized: true
      version: 4
    serial:
      # Emplacement de SLZB-06M
      port: tcp://SLZB-06M.home:6638
      baudrate: 115200
      adapter: ember
      # Optional: RTS / CTS Hardware Flow Control for serial port (default: false)
      rtscts: true
      # Désactiver la led verte ?
      disable_led: false
    advanced:
      # Réglez la puissance de sortie  de SLZB-06M sur 20 maximum
      transmit_power: 20
      # Force l'utilisation du channel Zigbee à 25 au lieu de 11
      channel: 25
      pan_id: ***REDACTED***
      ext_pan_id: ***REDACTED***
      network_key: ***REDACTED***
   ````

1. Démarrer Zigbee2MQTT avec la commande :

   ````bash
    cd /opt/zigbee2mqtt && npm start
   ````

    Le service est bien démarré lorsque vous pouvez voir la ligne :

    `[2025-03-09 17:23:21] info:     z2m: Zigbee2MQTT started!`

    Par exemple:

   ````bash
    root@z2m-proxmox:/opt/zigbee2mqtt# cd /opt/zigbee2mqtt && npm start

    > zigbee2mqtt@2.1.3 start
    > node index.js

    Starting Zigbee2MQTT without watchdog.
    [2025-03-09 17:23:16] info:     z2m: Logging to console, file (filename: log.log)
    [2025-03-09 17:23:16] info:     z2m: Starting Zigbee2MQTT version 2.1.3 (commit #unknown)
    [2025-03-09 17:23:16] info:     z2m: Starting zigbee-herdsman (3.2.7)
    [2025-03-09 17:23:16] warning:  zh:ezsp: 'ezsp' driver is deprecated and will only remain to provide support for older firmware (pre 7.4.x). Migration to 'ember' is recommended. If using Zigbee2MQTT see https://github.com/Koenkk/zigbee2mqtt/discussions/21462
    [2025-03-09 17:23:21] info:     z2m: zigbee-herdsman started (resumed)
    [2025-03-09 17:23:21] info:     z2m: Coordinator firmware version: '{"meta":{"maintrel":"1 ","majorrel":"7","minorrel":"3","product":12,"revision":"7.3.1.0 build 176"},"type":"EZSP v12"}'
    [2025-03-09 17:23:21] info:     z2m: Currently 0 devices are joined.
    [2025-03-09 17:23:21] info:     z2m: Connecting to MQTT server at mqtt://mqtt-proxmox.home:1883
    [2025-03-09 17:23:21] info:     z2m: Connected to MQTT server
    [2025-03-09 17:23:21] info:     z2m:mqtt: MQTT publish: topic 'zigbee2mqtt/bridge/state', payload '{"state":"online"}'
    [2025-03-09 17:23:21] info:     z2m: Started frontend on port 8080
    [2025-03-09 17:23:21] info:     z2m: Zigbee2MQTT started!
    [2025-03-09 17:23:26] info:     z2m:mqtt: MQTT publish: topic 'homeassistant/binary_sensor/1221051039810110150109113116116_0x9035eafffeb87213/connection_state/config', payload '{"device":{"hw_version":"EZSP v12 7.3.1.0 build 176","identifiers":["zigbee2mqtt_bridge_0x9035eafffeb87213"],"manufacturer":"Zigbee2MQTT","model":"Bridge","name":"Zigbee2MQTT Bridge","sw_version":"2.1.3"},"device_class":"connectivity","entity_category":"diagnostic","name":"Connection state","object_id":"zigbee2mqtt_bridge_connection_state","origin":{"name":"Zigbee2MQTT","sw":"2.1.3","url":"https://www.zigbee2mqtt.io"},"payload_off":"offline","payload_on":"online","state_topic":"zigbee2mqtt/bridge/state","unique_id":"bridge_0x9035eafffeb87213_connection_state_zigbee2mqtt","value_template":"{{ value_json.state }}"}'
    [2025-03-09 17:23:26] info:     z2m:mqtt: MQTT publish: topic 'homeassistant/binary_sensor/1221051039810110150109113116116_0x9035eafffeb87213/restart_required/config', payload '{"availability":[{"topic":"zigbee2mqtt/bridge/state","value_template":"{{ value_json.state }}"}],"availability_mode":"all","device":{"hw_version":"EZSP v12 7.3.1.0 build 176","identifiers":["zigbee2mqtt_bridge_0x9035eafffeb87213"],"manufacturer":"Zigbee2MQTT","model":"Bridge","name":"Zigbee2MQTT Bridge","sw_version":"2.1.3"},"device_class":"problem","enabled_by_default":false,"entity_category":"diagnostic","name":"Restart required","object_id":"zigbee2mqtt_bridge_restart_required","origin":{"name":"Zigbee2MQTT","sw":"2.1.3","url":"https://www.zigbee2mqtt.io"},"payload_off":false,"payload_on":true,"state_topic":"zigbee2mqtt/bridge/info","unique_id":"bridge_0x9035eafffeb87213_restart_required_zigbee2mqtt","value_template":"{{ value_json.restart_required }}"}'
    [2025-03-09 17:23:26] info:     z2m:mqtt: MQTT publish: topic 'homeassistant/button/1221051039810110150109113116116_0x9035eafffeb87213/restart/config', payload '{"availability":[{"topic":"zigbee2mqtt/bridge/state","value_template":"{{ value_json.state }}"}],"availability_mode":"all","command_topic":"zigbee2mqtt/bridge/request/restart","device":{"hw_version":"EZSP v12 7.3.1.0 build 176","identifiers":["zigbee2mqtt_bridge_0x9035eafffeb87213"],"manufacturer":"Zigbee2MQTT","model":"Bridge","name":"Zigbee2MQTT Bridge","sw_version":"2.1.3"},"device_class":"restart","name":"Restart","object_id":"zigbee2mqtt_bridge_restart","origin":{"name":"Zigbee2MQTT","sw":"2.1.3","url":"https://www.zigbee2mqtt.io"},"payload_press":"","unique_id":"bridge_0x9035eafffeb87213_restart_zigbee2mqtt"}'
    [2025-03-09 17:23:26] info:     z2m:mqtt: MQTT publish: topic 'homeassistant/select/1221051039810110150109113116116_0x9035eafffeb87213/log_level/config', payload '{"availability":[{"topic":"zigbee2mqtt/bridge/state","value_template":"{{ value_json.state }}"}],"availability_mode":"all","command_template":"{\"options\": {\"advanced\": {\"log_level\": \"{{ value }}\" } } }","command_topic":"zigbee2mqtt/bridge/request/options","device":{"hw_version":"EZSP v12 7.3.1.0 build 176","identifiers":["zigbee2mqtt_bridge_0x9035eafffeb87213"],"manufacturer":"Zigbee2MQTT","model":"Bridge","name":"Zigbee2MQTT Bridge","sw_version":"2.1.3"},"entity_category":"config","name":"Log level","object_id":"zigbee2mqtt_bridge_log_level","options":["error","warning","info","debug"],"origin":{"name":"Zigbee2MQTT","sw":"2.1.3","url":"https://www.zigbee2mqtt.io"},"state_topic":"zigbee2mqtt/bridge/info","unique_id":"bridge_0x9035eafffeb87213_log_level_zigbee2mqtt","value_template":"{{ value_json.log_level | lower }}"}'
    [2025-03-09 17:23:26] info:     z2m:mqtt: MQTT publish: topic 'homeassistant/sensor/1221051039810110150109113116116_0x9035eafffeb87213/version/config', payload '{"availability":[{"topic":"zigbee2mqtt/bridge/state","value_template":"{{ value_json.state }}"}],"availability_mode":"all","device":{"hw_version":"EZSP v12 7.3.1.0 build 176","identifiers":["zigbee2mqtt_bridge_0x9035eafffeb87213"],"manufacturer":"Zigbee2MQTT","model":"Bridge","name":"Zigbee2MQTT Bridge","sw_version":"2.1.3"},"entity_category":"diagnostic","icon":"mdi:zigbee","name":"Version","object_id":"zigbee2mqtt_bridge_version","origin":{"name":"Zigbee2MQTT","sw":"2.1.3","url":"https://www.zigbee2mqtt.io"},"state_topic":"zigbee2mqtt/bridge/info","unique_id":"bridge_0x9035eafffeb87213_version_zigbee2mqtt","value_template":"{{ value_json.version }}"}'
    [2025-03-09 17:23:26] info:     z2m:mqtt: MQTT publish: topic 'homeassistant/sensor/1221051039810110150109113116116_0x9035eafffeb87213/coordinator_version/config', payload '{"availability":[{"topic":"zigbee2mqtt/bridge/state","value_template":"{{ value_json.state }}"}],"availability_mode":"all","device":{"hw_version":"EZSP v12 7.3.1.0 build 176","identifiers":["zigbee2mqtt_bridge_0x9035eafffeb87213"],"manufacturer":"Zigbee2MQTT","model":"Bridge","name":"Zigbee2MQTT Bridge","sw_version":"2.1.3"},"enabled_by_default":false,"entity_category":"diagnostic","icon":"mdi:chip","name":"Coordinator version","object_id":"zigbee2mqtt_bridge_coordinator_version","origin":{"name":"Zigbee2MQTT","sw":"2.1.3","url":"https://www.zigbee2mqtt.io"},"state_topic":"zigbee2mqtt/bridge/info","unique_id":"bridge_0x9035eafffeb87213_coordinator_version_zigbee2mqtt","value_template":"{{ value_json.coordinator.meta.revision }}"}'
    [2025-03-09 17:23:26] info:     z2m:mqtt: MQTT publish: topic 'homeassistant/sensor/1221051039810110150109113116116_0x9035eafffeb87213/network_map/config', payload '{"availability":[{"topic":"zigbee2mqtt/bridge/state","value_template":"{{ value_json.state }}"}],"availability_mode":"all","device":{"hw_version":"EZSP v12 7.3.1.0 build 176","identifiers":["zigbee2mqtt_bridge_0x9035eafffeb87213"],"manufacturer":"Zigbee2MQTT","model":"Bridge","name":"Zigbee2MQTT Bridge","sw_version":"2.1.3"},"enabled_by_default":false,"entity_category":"diagnostic","json_attributes_template":"{{ value_json.data.value | tojson }}","json_attributes_topic":"zigbee2mqtt/bridge/response/networkmap","name":"Network map","object_id":"zigbee2mqtt_bridge_network_map","origin":{"name":"Zigbee2MQTT","sw":"2.1.3","url":"https://www.zigbee2mqtt.io"},"state_topic":"zigbee2mqtt/bridge/response/networkmap","unique_id":"bridge_0x9035eafffeb87213_network_map_zigbee2mqtt","value_template":"{{ now().strftime('%Y-%m-%d %H:%M:%S') }}"}'
    [2025-03-09 17:23:26] info:     z2m:mqtt: MQTT publish: topic 'homeassistant/switch/1221051039810110150109113116116_0x9035eafffeb87213/permit_join/config', payload '{"availability":[{"topic":"zigbee2mqtt/bridge/state","value_template":"{{ value_json.state }}"}],"availability_mode":"all","command_topic":"zigbee2mqtt/bridge/request/permit_join","device":{"hw_version":"EZSP v12 7.3.1.0 build 176","identifiers":["zigbee2mqtt_bridge_0x9035eafffeb87213"],"manufacturer":"Zigbee2MQTT","model":"Bridge","name":"Zigbee2MQTT Bridge","sw_version":"2.1.3"},"icon":"mdi:human-greeting-proximity","name":"Permit join","object_id":"zigbee2mqtt_bridge_permit_join","origin":{"name":"Zigbee2MQTT","sw":"2.1.3","url":"https://www.zigbee2mqtt.io"},"payload_off":"{\"time\": 0}","payload_on":"{\"time\": 254}","state_off":"false","state_on":"true","state_topic":"zigbee2mqtt/bridge/info","unique_id":"bridge_0x9035eafffeb87213_permit_join_zigbee2mqtt","value_template":"{{ value_json.permit_join | lower }}"}'
   ````

1. Le service est désormais accessible via l'adresse `http://z2m-proxmox.home:9442`

## Utilisation dans Home Assistant

1. Dans Home Assisant, allez dans `Paramètres > Appareils et services` puis cliquez `+ Ajouter une intégration`.
1. Cherchez `MQTT` et sélectionnez la première intégration (MQTT seul).
1. Sélectionnez `Saisir manuellement les informations de connexion`.
1. Entrez les informations demandées :

   * Courtier : mqtt-proxmox.home
   * Port : 1883
   * Nom utilisateur : z2m-proxmox
   * Mot de passe : ***REDACTED***


## Références

- [How to Separate Zigbee2MQTT from Home Assistant in Proxmox](https://smarthomescene.com/guides/how-to-separate-zigbee2mqtt-from-home-assistant-in-proxmox/)