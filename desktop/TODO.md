# TODO
## OTHER
```
sudo apt install net-tools
sudo apt install htop
sudo apt install git
```
```
sudo apt install -y docker.io docker-compose
sudo usermod -aG docker $USER
reboot
```

## KODI:
```
sudo apt install kodi

mkdir /home/djpb/.config/autostart
sudo nano /home/djpb/.config/autostart/kodi.desktop

[Desktop Entry]
Type=Application
Name=Kodi
Exec=/usr/bin/kodi
OnlyShownIn=XFCE;
RunHook=0
StartupNotify=false
Terminal=false
Hidden=false
```
# SPOTIFY
https://github.com/ldsz/plugin.audio.spotify/releases/tag/1.2.3
# NETFLIX
https://castagnait.github.io/repository.castagnait/repository.castagnait-2.0.0.zip
sudo apt install kodi-inputstream-adaptive 
# HULU
https://slyguy.uk/repository.slyguy.zip



#filemanager



pactl list short sinks
pactl set-default-sink 'alsa_output.pci-0000_0a_00.1.hdmi-stereo-extra1'


wget https://raw.githubusercontent.com/djpbadenhorst/install/main/docker.sh
