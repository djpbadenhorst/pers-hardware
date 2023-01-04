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

## INSTALL LPASS & GH
Install lastpass and gh for install scripts:

```
sudo apt install -y lastpass-cli
export LPASS_DISABLE_PINENTRY=1
echo <PASSWORD> | lpass login djpbadenhorst@gmail.com
eval $(lpass show --notes pers/github/token)
eval $(lpass show --notes script/bootstrap)
```

# REMOTE CONTROL
Install vnc servers:

```
gh gist-view ./install-tightvnc | sh
gh gist-view ./install-x11vnc | sh
```

## SHELLINABOX

Install shellinabox:

```
sudo apt install -y shellinabox
```
