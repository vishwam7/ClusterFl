
# Instructions to configure network for the Mobysis-ClusterFL federated learning system






## Author

- [@Vishwam Shah](https://github.com/vishwam7)


## Assign a static IP address

1) Determine the IP address of the machine:

```bash
  ifconfig
```

2) Edit the network configuration file /etc/network/interfaces:

```bash
sudo nano /etc/network/interfaces
```

3) Modify the file to include the following lines, replacing `<STATIC_IP_ADDRESS>` with the desired IP address:
```bash
auto eth0
iface eth0 inet static
address <STATIC_IP_ADDRESS>
netmask 255.255.255.0
gateway <GATEWAY_IP_ADDRESS>
dns-nameservers <DNS_SERVER_IP_ADDRESS>
```

4) Save the file and exit.

5) Restart the network service to apply the changes:
```bash
sudo systemctl restart networking
```

## Configure the firewall rules

### Using iptables

1) Install the iptables package if it is not already installed:

```bash
sudo apt-get install iptables
```

2) Open the necessary port by running the following commands, replacing `<PORT_NUMBER>` with the port number specified in the Mobysis-ClusterFL configuration files:
```bash
sudo iptables -A INPUT -p tcp --dport <PORT_NUMBER> -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport <PORT_NUMBER> -j ACCEPT
```

3) Save the iptables rules:
```bash
sudo iptables-save > /etc/iptables/rules.v4
```

### Using firewall configuration tool

Open the firewall configuration tool provided by your operating system.

Add a new rule to allow incoming and outgoing traffic on the specified port number, and save the configuration.

1) Check if a firewall is installed on your system:
Ubuntu comes with a firewall pre-installed called "ufw" (Uncomplicated Firewall). To check if it is installed, run the following command in the terminal:
```bash
sudo ufw status
```
If the firewall is not installed, you can install it with the following command:
```bash
sudo apt-get install ufw
```
2) Allow incoming traffic on a specific port:
To allow incoming traffic on a specific port, you can use the following command:
```bash
sudo ufw allow <port_number>/tcp
```
3) Allow outgoing traffic on a specific port:
To allow outgoing traffic on a specific port, you can use the following command:
```bash
sudo ufw allow out <port_number>/tcp
```
4) Check the status of the firewall:
To check the status of the firewall and see a list of allowed ports, run:
```bash
sudo ufw status
```
This will show you a list of the rules that are currently in effect, including the ports that are allowed and denied.

5) Disable the firewall:
If you need to disable the firewall temporarily, you can run:
```bash
sudo ufw disable
```

6) To re-enable the firewall, run:
```bash
sudo ufw enable
```

### Verify communication

1) Determine the IP address of the other machine.

2) Ping the IP address of the other machine from each machine:
```bash
ping <OTHER_MACHINE_IP_ADDRESS>
```
3) If the ping command succeeds, it means that the machines can communicate with each other over the network.