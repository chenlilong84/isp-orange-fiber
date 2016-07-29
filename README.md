# isp-orange-fiber
To replace the Orange Livebox by FreeBSD. (I adapted from sources that these patches work on FreeBSD 10.3)

The ISP Orange (fiber) uses authentication by DHCP to assign an IP address. The option "user-class" of dhclient is missing. The dhclient.patch file adds this option. This is not the only missing option also because the ISP uses vlanpcp 6 to enable the DHCP authentication.

Once you apply these patches, you must use the following configuration:

```
$ ifconfig vlan832 create
$ ifconfig vlan832 vlan 832 vlandev your_interface vlanpcp 6

$ vi /etc/dhclient.conf
  interface "vlan832" {
        send dhcp-class-identifier "sagem";
        send user-class "+FSVDSL_livebox.Internet.softathome.Livebox3";
        send option-90 00:00:00:00:00:00:00:00:00:00:00:66:74:69:2f:xx:xx:xx:xx:xx:xx:xx;
  }

$ dhclient vlan832
```

(Remplace the "xx:xx:xx:xx:xx:xx:xx" by your login fti/xxx in hexa)

You can use your favorite firewall as PF or IPFW.

Sources :
- https://reviews.freebsd.org/D801
- https://lafibre.info/remplacer-livebox/remplacer-la-livebox-sans-pppoe/
