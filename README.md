# Linux-split-access-routing
server settings for complex network access requirements 

Set up split access routing
1#########
First, create two routing tables, T1 and T2 to be used for packets sent to or from these NICs by adding the lines
252 Teth1
251 Teth2
to /etc/iproute2/rt_tables.
2##########
Next, set up the routing rules to route incoming and outgoing packets via these tables:


ip route add 172.16.1.0/24 dev eth1 src 172.16.1.233 table Teth1
ip route add default via 172.16.1.1 dev eth1 src 172.16.1.233 table Teth1
ip rule add from 172.16.1.233 table Teth1

ip route add 192.168.1.0/24 dev eth2 src 192.168.1.20 table Teth2
ip route add default via 192.168.1.1 dev eth2 src 192.168.1.20 table Teth2
ip rule add from 192.168.1.20 table Teth2

3###########

ip route flush cache

ref: https://www.suse.com/support/kb/doc/?id=000016626

Configure the Host System to use IPv4 Reverse Path Filtering
1#######
grep [01] /proc/sys/net/ipv4/conf/*/rp_filter|egrep "default|all"
2#######
net.ipv4.conf.all.rp_filter=1 
net.ipv4.conf.default.rp_filter=1 

to  /etc/sysctl.conf

3#######
applying config
sysctl -p

ref: https://docs.vmware.com/en/vRealize-Operations/8.6/com.vmware.vcom.core.doc/GUID-0AAA4D96-5FDE-49A7-8BB3-D7F56C89137C.html
