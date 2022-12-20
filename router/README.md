# README

## INSTALL:
Rasbian64bit
WifiZone=GB

sudo raspi-config (set network-manager)

create network:
* pppoe
-- wijnlanden_6@crispfibre
-- Weblock123
-- Connect automatically
-- DNS: 8.8.8.8
* ethernet
-- device: eth1
-- method: shared to other


sudo apt update
sudo apt upgrade
sudo apt full-upgrade
sudo rpi-update



########################### 
# POTENTIAL DISPATCH FILE #
###########################

#!/bin/sh
#
# /etc/NetworkManager/dispatcher.d/05-batman

ESSID="Igloo mesh"
IFACE="wlp2s0"
ADDR="01:23:45:67:89:AB"

function current {
    nmcli -t -f GENERAL.CONNECTION d show $IFACE | cut -d\: -f2
}

interface=$1
status=$2

if [ ! "$interface" == $IFACE ]; then
  exit 0
fi

case $status in
  up)
    if [ "$(current)" == "$ESSID" ]; then
      ip link set dev $IFACE mtu 1560 # 1500+60
      modprobe batman-adv
      batctl if add $IFACE
      batctl gw_mode client
      # Keep the same MAC address (optional)
      ip link set dev bat0 address $ADDR
      ip link set dev bat0 up
      #sleep 10
      # DHCP (optional)
      dhclient -r
      dhclient -H $(hostname) bat0
    fi
    ;;
  down)
    if [ ! "$(current)" == "$ESSID" ]; then
      dhclient -r
      ip link set dev bat0 down
      batctl if del $IFACE
    fi
    ;;
esac
