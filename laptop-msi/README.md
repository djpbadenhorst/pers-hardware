# README

## BOOTING
Install Xubuntu

## UPDATE
Set correct graphics settings:
```
sudo apt update 
sudo apt -y upgrade
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
```

