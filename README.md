# OpenStackOnNuc
Configuration files for opendevstack on intel NUC

# Get the stable Juno release of devstack

    git clone https://github.com/openstack-dev/devstack.git -b stable/juno

# Local.conf

Due to the name of the interface its important to set it as em1 on config file.
Change your passwords :)

    [[local|localrc]]
    FLOATING_RANGE=192.168.1.224/27
    FIXED_RANGE=10.11.12.0/24
    FIXED_NETWORK_SIZE=256
    HOST_IP_IFACE=em1
    PUBLIC_INTERFACE=em1
    VLAN_INTERFACE=em1
    FLAT_INTERFACE=em1
    ADMIN_PASSWORD=supersecret
    MYSQL_PASSWORD=iheartdatabases
    RABBIT_PASSWORD=flopsymopsy
    SERVICE_PASSWORD=iheartksl
    SERVICE_TOKEN=xyzpdqlazydog
    
# On host machine with devstack allow forwarding and masquerading with

ip forwarding changes affect on reboot of the NUC

    echo 1 > /proc/sys/net/ipv4/ip_forward
    echo 1 > /proc/sys/net/ipv4/conf/eth0/proxy_arp
                                     em1
    iptables -t nat -A POSTROUTING -o br100 -j MASQUERADE
    
# When using Centos

Disable or open ports INSIDE the vm with iptables. EX open 8080
    
    sudo iptables -I INPUT 5 -i eth0 -p tcp --dport 8080 -m state --state NEW,ESTABLISHED -j ACCEPT

save the iptables for reboots.

    service iptables save
    
Disable requiretty to particular user not to use tty.
    
    $ sudo /usr/sbin/visudo

    Defaults:username !requiretty
    
If using the contextBroker image, stepst to be made:

start mongod

    sudo service mongod start
    
add iptables port

    sudo iptables -I INPUT 5 -i eth0 -p tcp --dport 1026 -m state --state NEW,ESTABLISHED -j ACCEPT
    service iptables save

start contextBroker

    sudo contextBroker -port 1026
