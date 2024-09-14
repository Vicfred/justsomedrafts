```
cp /home/vicfred/Downloads/mullvad_config_linux_us_sjc/* /etc/openvpn/
```

```
mitsuha ~ # ls /etc/openvpn/
down.sh  mullvad_ca.crt  mullvad_userpass.txt  mullvad_us_sjc.conf  update-resolv-conf  up.sh

mitsuha ~ # chmod +x /etc/openvpn/update-resolv-conf

mitsuha ~ # ln -s /etc/init.d/openvpn /etc/init.d/openvpn.mullvad_us_sjc
mitsuha ~ # rc-update add openvpn.mullvad_us_sjc
 * service openvpn.mullvad_us_sjc added to runlevel default
```

