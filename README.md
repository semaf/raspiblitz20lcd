# Cloning the repository

In the home directory (`/home/admin/`) clone this repository: `git clone git@github.com:semaf/raspiblitz20lcd.git`, and go to `raspiblitz20lcd` directory.

# Installing driver for 2.0 inch LCD

Compile the device tree source directly into overlays directory on the sdcard:  
`dtc -O dtb -o /boot/overlays/waveshare20lcd.dtb waveshare20lcd/waveshare20lcd.dts`.

Deactivate the device-tree overlay for 3.5 inch LCD and activate for 2.0 LCD by replacing the following line in `/boot/config.txt`:
~~`dtoverlay=waveshare35a:rotate=270`~~ --> `dtoverlay=waveshare20`

Reboot the Raspberry Pi, It should now display everything on 2.0 LCD screen.

# Installing fan control script
Create a unit file for systemd service in the `/lib/systemd/system/fan_control.service`: 

```
[Unit]
Description=Fan control service
After=multi-user.target

[Service]
Type=simple
ExecStart=/usr/bin/python /home/admin/raspiblitz20lcd/fan_control.py
Restart=on-abort

[Install]
WantedBy=multi-user.target
```
Add admin into GPIO group: `usermod -a -G gpio admin`.
Then, activate it as systemd service:
```
sudo chmod 644 /lib/systemd/system/fan_control.service
chmod +x /home/admin/raspiblitz20lcd/fan_control.py
sudo systemctl daemon-reload
sudo systemctl enable fan_control.service
sudo systemctl start fan_control.service
```

# Setting what is seen on the LCD

Just replace the `/home/admin/00infoBlitz.sh` with the new script file in this repository: `mv /home/admin/raspiblitz20lcd/00infoBlitz.sh /home/admin/`.

Enjoy!
