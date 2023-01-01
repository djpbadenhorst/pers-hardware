# README

## BOOTING
Install Xubuntu

## DISPLAY
Set correct resolution:
```
echo "display-setup-script=xrandr --output HDMI-0 --mode 1280x800" | sudo tee -a /etc/lightdm/lightdm.conf > /dev/null
```

Set correct graphics settings:
```
sudo apt update 
sudo apt -y upgrade
ubuntu-drivers devices
sudo apt install -y nvidia-driver-470
xfconf-query -c xsettings -p /Net/ThemeName -s "Greybird-dark"
```

Disable lock screen:
```
xfconf-query -c xfce4-screensaver -p /lock/enabled -t bool -s false -n
```

# REMOTE CONTROL
Install tightvnc server:
```
sudo apt install -y tightvncserver
mkdir -p /home/djpb/.vnc/

echo <PASSWORD> | vncpasswd -f > /home/djpb/.vnc/passwd
sudo chown -R djpb:djpb /home/djpb/.vnc
sudo chmod 0600 /home/djpb/.vnc/passwd

cat << EOF | tee -a /home/djpb/.vnc/xstartup > /dev/null
#!/bin/sh
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
exec dbus-launch startxfce4
EOF

chmod +x /home/djpb/.vnc/xstartup

cat << EOF | sudo tee -a /etc/systemd/system/vncserver.service > /dev/null
[Unit]
After=syslog.target network.target
[Service]
Type=forking
User=djpb
PAMName=login
PIDFile=/home/djpb/.vnc/%H:1.pid
ExecStartPre=-/usr/bin/vncserver -kill :1 > /dev/null 2>&1
ExecStart=/usr/bin/vncserver
ExecStop=/usr/bin/vncserver -kill :1
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl enable vncserver.service
sudo systemctl daemon-reload
```

Install x11vnc server:

```
sudo apt install -y x11vnc

cat << EOF | sudo tee -a /etc/systemd/system/x11vnc.service > /dev/null
[Unit]
After=multi-user.target
[Service]
Type=simple
ExecStart=/usr/bin/x11vnc -auth guess -forever -loop -noxdamage -repeat -rfbauth /home/djpb/.vnc/passwd -rfbport 5900 -shared
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl enable x11vnc.service
sudo systemctl daemon-reload
```

## SHELLINABOX
Install shellinabox:
```
sudo apt install -y openssh-server shellinabox

cat << EOF | sudo tee -a /etc/default/shellinabox > /dev/null
SHELLINABOX_DAEMON_START=1
SHELLINABOX_PORT=4200
SHELLINABOX_ARGS="--no-beep -t"
EOF

sudo systemctl restart shellinabox.service
```
