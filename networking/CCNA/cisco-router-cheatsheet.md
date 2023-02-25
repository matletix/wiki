# Cisco Routers Configuration Basics

- Enter privileged mode : `enable`
- Enter global config mode : `configure terminal`
- Set router hostname to R1 : `hostname R1`
- Do a non-config command in config mode : `do <COMMAND>`
- Show interfaces : `show ip interface brief`
- Show routes : `show ip route`
- Show running configuration : `show running-config`
- Configure interface g0/1 : `interface g0/1`
- Set interface IP : `ip address <IP> <NETMASK>`
- Set interface description : `description ## DESCRIPTION ##`
- Enable the interface : `no shutdown`
- Add a route (in global config mode) : `ip route <DEST NET IP> <DEST NETMASK> <NEXTHOP IP | ROUTER INTERFACE NAME>`
- Remove a route : 
  - check routes in the running config : `do sh run | include ip route`
  - remove the relevant route : `no ip route ...`
