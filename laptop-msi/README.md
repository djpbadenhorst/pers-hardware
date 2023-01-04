# README

## BOOTING
Install Xubuntu

## DISPLAY
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
Install x11vnc server:

```
gh gist-view ./install-x11vnc | sh
```

## SHELLINABOX
Install shellinabox:

```
sudo apt install -y shellinabox
```

