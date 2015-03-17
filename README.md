# OpenStackOnNuc
Configuration files for opendevstack on intel NUC

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
