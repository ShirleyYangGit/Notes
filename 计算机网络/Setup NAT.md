## **Set up eth12 & eth13**

-   Run 'iwconfig' to see the eth name
    ```
    `[root``@localhost`  `network-scripts]# iwconfig`
    
    `lo no wireless extensions.`
    
    `eth12 no wireless extensions.`
    
    `eth13 no wireless extensions.`
    ```
-   Create ifcfg-eth12 and ifcfg-eth13 in /etc/sysconfig/network-scripts
    
    1. Assign (dvpgVm) static IP to eth12 (example: 3821, Example: 172.26.0.11)
    
    2. eth13 (VM Network) should have routable IP
    ```
    `[root``@localhost`  `network-scripts]# cat ifcfg-eth12`
    
    `DEVICE=eth12`
    
    `BOOTPROTO=``static`
    
    `ONBOOT=yes`
    
    `TYPE=Ethernet`
    
    `DNS=``172.26``.``0.30`
    
    `IPADDR=``172.26``.``0.11`
    
    `NETMASK=``255.255``.``0.0`
    
    `USERCTL=no`
    
    `PEERDNS=yes`
    
    `[root``@localhost`  `network-scripts]# cat ifcfg-eth13`
    
    `DEVICE=eth13`
    
    `BOOTPROTO=dhcp`
    
    `ONBOOT=yes`
    
    `TYPE=Ethernet`
    
    `#DNS=``10.142``.``7.1`
    
    `NETMASK=``255.255``.``0.0`
    
    `IPADDR=``172.26``.``0.111`
    
    `USERCTL=no`
    
    `PEERDNS=yes`
    ```
-   Run 'ifup eth12' and 'ifup eth13', verify eth12 and eth13 are up by "ifconfig"

## Edit iptables

-   Running the following commands
    ```
    `iptables --table nat -A POSTROUTING -o eth13 -j MASQUERADE`
    
    `iptables -A FORWARD -i eth13 -o eth12 -m state --state RELATED,ESTABLISHED -j ACCEPT`
    ```
-   Edit /etc/sysconfig/iptables-config and add the following to make it persistent across reboot. Make sure to reboot gracefully, only then the existing rules will be saved.
    ```
    `IPTABLES_MODULES_UNLOAD=``"yes"`
    
    `IPTABLES_SAVE_ON_RESTART=``"yes"`
    
    `IPTABLES_SAVE_ON_STOP=``"yes"`
    ```
-   Verify the iptables
    ```
    `[root``@localhost`  `~]# iptables -nv -L`
    
    `Chain INPUT (policy ACCEPT` `144`  `packets,` `10272`  `bytes)`
    
    `pkts bytes target prot opt in out source destination`
    
    `Chain FORWARD (policy ACCEPT` `0`  `packets,` `0`  `bytes)`
    
    `pkts bytes target prot opt in out source destination`
    
    `0` `0`  `ACCEPT all -- eth0 eth1` `0.0``.``0.0``/``0` `0.0``.``0.0``/``0` `state RELATED,ESTABLISHED`
    
    `...`
    
    `0` `0`  `ACCEPT all -- eth13 eth12` `0.0``.``0.0``/``0` `0.0``.``0.0``/``0` `state RELATED,ESTABLISHED. <-------- New Added rule`
    
    `Chain OUTPUT (policy ACCEPT` `96`  `packets,` `15720`  `bytes)`
    
    `pkts bytes target prot opt in out source destination`
    ```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM4MDY0OTExM119
-->