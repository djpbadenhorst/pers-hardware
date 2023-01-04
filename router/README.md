# README

## BOOTING
Turn Boot Switch On

```
git clone --depth=1 https://github.com/raspberrypi/usbboot
cd usbboot
make
sudo ./rpiboot
rpi-imager
```

Install Rasbian (64bit) [Set WifiZone=GB]
Turn Boot Switch Off

## UPDATE
Update raspberry pi firmware:

```
sudo apt update
sudo apt upgrade -y
sudo apt full-upgrade
sudo rpi-update
```

## NETWORKING
Enable NetworkManager:

```
sudo raspi-config 
```

Advanced Options > Network Config > NetworkManager

```
nmcli connection delete Wired\ connection\ 1
nmcli connection delete Wired\ connection\ 2
nmcli connection add type pppoe ifname ppp0 con-name dsl parent eth0 username wijnlanden_6@crispfibre password $PPP_PASSWORD
nmcli connection add type ethernet ifname eth1 con-name ethernet autoconnect yes ipv4.method shared ipv6.method shared
cat << EOF | tee -a /etc/NetworkManager/dispatcher.d/pre-up.d/iptables > /dev/null
#!/bin/sh
LOGFILE=/var/log/iptables.log
if [ $1 = ppp0 ] && [ $2 = pre-up]; then
    echo "$0: clamp mss to pmtu" >> $LOGFILE
	/sbin/iptables -t mangle -o ppp0 -I FORWARD 1 -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu
fi
exit 0
EOF
chmod a+x /etc/NetworkManager/dispatcher.d/pre-up.d/iptables
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

## TAILSCALE
Install tailscale:

```
curl -fsSL https://tailscale.com/install.sh | sh
echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
sudo sysctl -p /etc/sysctl.d/99-tailscale.conf
sudo tailscale up --advertise-routes=10.42.0.0/16 --hostname=router --auth-key=$TAILSCALE_AUTHKEY
```

Activate Routes in www.tailscale.com

## VINO
Install Vino server:

```
gh gist-view ./install-vino | sh
```

## SHELLINABOX
Install shellinabox:

```
sudo apt install -y shellinabox
```

## DOCKER
Install docker:

```
gh gist-view ./install-docker | sh
```
