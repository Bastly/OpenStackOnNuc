# OpenStackOnNuc
Configuration files for opendevstack on intel NUC

Install x64 ubuntu server

We need to add a user to install DevStack. (if you created a user during install you can skip this step and just give the user sudo privileges below)

adduser stack

apt-get install sudo -y || yum install -y sudo
echo "stack ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

if echo does not work use tee to add stack to sudoers or use visudo
logout and login with that user.

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
    
# Get into de folder and install

    ./stack.sh
    
# On host machine with devstack allow forwarding and masquerading with

ip forwarding changes affect on reboot of the NUC

    echo 1 > /proc/sys/net/ipv4/ip_forward
    echo 1 > /proc/sys/net/ipv4/conf/eth0/proxy_arp
                                     em1
    iptables -t nat -A POSTROUTING -o br100 -j MASQUERADE
    
# When using Centos

Disable or open ports INSIDE the vm with iptables. EX open 1026
    
    sudo iptables -I INPUT 5 -i eth0 -p tcp --dport 1026 -m state --state NEW,ESTABLISHED -j ACCEPT

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
    
#If stack goes down restablish with

https://opensource.ncsa.illinois.edu/confluence/display/BD/Persist+DevStack+After+Host+Reboot

    retain the DevStack configurations across rebooting of a hosting machine. Do:
    Run "./unstack.sh".
    This cleanly stops the stack services and screen processes.
    Reboot the machine.
    Manually start the RabbitMQ server.
    Somehow the RabbitMQ server is not automatically started. Do "sudo service rabbitmq-server start".  Then do        "sudo service rabbitmq-server status" to verify that it is started successfully.
    Run "./rejoin-stack.sh", do NOT use "./stack.sh" to start the stack.
    This will use the "stack-screenrc" file, restart the stack, and keep all the configurations that have been         made, such as uploaded images, VM instances in suspended (or shelved, but Rui did not test the shelved state)     state, and uploaded key pairs. (sonrisa) For example, the VM instances can be seen both in the "nova list"         result and the web UI, and "nova resume ..." of suspended VM instances work.
