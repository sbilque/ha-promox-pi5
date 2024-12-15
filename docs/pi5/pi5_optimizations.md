# Optimisations Pi 5

## Optimisation de la mémoire

1. Ouvrez une session SSH sur pimox5.
1. Exécutez la commande `sudo rpi-update`.

    Si une mise à jour est nécessaire, la commande affiche une question de la forme suivante :

    ```sh
    *** Raspberry Pi firmware updater by Hexxeh, enhanced by AndrewS and Dom
    *** Performing self-update
    *** Relaunching after update
    *** Raspberry Pi firmware updater by Hexxeh, enhanced by AndrewS and Dom
    FW_REV:b2e902bbf2dc5a288c36dccf1f9f856a0b32e079
    BOOTLOADER_REV:6b431180b8f1b16128f28fa3d0ee4d9141d6c163
    *** We're running for the first time
    *** Backing up files (this will take a few minutes)
    *** Backing up firmware
    *** Backing up modules 6.6.62+rpt-rpi-2712
    WANT_32BIT:0 WANT_64BIT:1 WANT_PI4:1 WANT_PI5:1
    ##############################################################
    WARNING: This update bumps to rpi-6.6.y linux tree
    See: https://forums.raspberrypi.com/viewtopic.php?p=2191175

    'rpi-update' should only be used if there is a specific
    reason to do so - for example, a request by a Raspberry Pi
    engineer or if you want to help the testing effort
    and are comfortable with restoring if there are regressions.

    DO NOT use 'rpi-update' as part of a regular update process.
    ##############################################################
    Would you like to proceed? (y/N)
    ```

1. Répondre `y` à la question `Would you like to proceed? (y/N)`.
   La commande affiche :

    ```sh
    Downloading bootloader tools
    Downloading bootloader images
    *** Downloading specific firmware revision (this will take a few minutes)
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
    100  145M  100  145M    0     0  38.3M      0  0:00:03  0:00:03 --:--:-- 39.8M
    *** PREPARING EEPROM UPDATES ***

    BOOTLOADER: update available
    CURRENT: Mon 23 Sep 13:02:56 UTC 2024 (1727096576)
        LATEST: Sat  7 Dec 12:42:23 UTC 2024 (1733575343)
    RELEASE: latest (/lib/firmware/raspberrypi/bootloader-2712/latest)
                Use raspi-config to change the release.
    CURRENT: Mon 23 Sep 13:02:56 UTC 2024 (1727096576)
        UPDATE: Sat  7 Dec 12:42:23 UTC 2024 (1733575343)
        BOOTFS: /boot/firmware
    '/tmp/tmp.EhG1TwIM2j' -> '/boot/firmware/pieeprom.upd'

    UPDATING bootloader. This could take up to a minute. Please wait

    *** Do not disconnect the power until the update is complete ***

    If a problem occurs then the Raspberry Pi Imager may be used to create
    a bootloader rescue SD card image which restores the default bootloader image.

    flashrom -p linux_spi:dev=/dev/spidev10.0,spispeed=16000 -w /boot/firmware/pieeprom.upd
    Verifying update
    VERIFY: SUCCESS
    UPDATE SUCCESSFUL
    *** Updating firmware
    *** Updating kernel modules
    *** depmod 6.6.64-v8+
    *** depmod 6.6.64-v8-16k+
    *** Updating VideoCore libraries
    *** Running ldconfig
    *** Storing current firmware revision
    *** Deleting downloaded files
    *** Syncing changes to disk
    *** If no errors appeared, your firmware was successfully updated to b2e902bbf2dc5a288c36dccf1f9f856a0b32e079
    *** A reboot is needed to activate the new firmware
    ```

2. Exécutez la commande `sudo rpi-eeprom-config -e`
3. Ajoutez la ligne `SDRAM_BANKLOW=1` à la fin, si nécessaire, pour obtenir le contenu suivant puis sauvez le fichier :

    ```ini
    [all]
    BOOT_UART=1
    POWER_OFF_ON_HALT=0
    BOOT_ORDER=0xf146
    SDRAM_BANKLOW=1
    ```

4. La commande doit ensuite afficher un résultat de la forme suivante

    ```sh
    Updating bootloader EEPROM
    image: /lib/firmware/raspberrypi/bootloader-2712/latest/pieeprom-2024-12-07.bin
    config_src: blconfig device
    config: /tmp/tmpitqorgqb/boot.conf
    ################################################################################
    [all]
    BOOT_UART=1
    POWER_OFF_ON_HALT=0
    BOOT_ORDER=0xf146
    SDRAM_BANKLOW=1

    ################################################################################
    *** CREATED UPDATE /tmp/tmpitqorgqb/pieeprom.upd  ***

    CURRENT: Mon 23 Sep 13:02:56 UTC 2024 (1727096576)
        UPDATE: Sat  7 Dec 12:42:23 UTC 2024 (1733575343)
        BOOTFS: /boot/firmware
    '/tmp/tmp.jVEZk8QIBt' -> '/boot/firmware/pieeprom.upd'

    UPDATING bootloader. This could take up to a minute. Please wait

    *** Do not disconnect the power until the update is complete ***

    If a problem occurs then the Raspberry Pi Imager may be used to create
    a bootloader rescue SD card image which restores the default bootloader image.

    flashrom -p linux_spi:dev=/dev/spidev10.0,spispeed=16000 -w /boot/firmware/pieeprom.upd
    Verifying update
    VERIFY: SUCCESS
    UPDATE SUCCESSFUL
    ```

1. Arrêter vos vms dans Proxmox puis redémarrez Proxmox. Il est fortement déconseillé de lancer la commande `reboot` directement puis la ligne de commande car vos machines virtuelles peuvent être corrompues.

## Références

* [Optimisez la mémoire de vos RPi5 et RPi4B (Décembre 2024)](https://community.jeedom.com/t/optimisez-la-memoire-de-vos-rpi5-et-rpi4b-decembre-2024/134625)
* [Boostez votre Raspberry Pi 4 ou 5 en affinant l’accès à la SRAM](https://www.framboise314.fr/boostez-votre-raspberry-pi-4-ou-5-en-affinant-lacces-a-la-sram/)
