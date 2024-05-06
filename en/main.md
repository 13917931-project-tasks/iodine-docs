# Iodine usage documentation

Iodine is a tool which can be used to create DNS Tunnels. This text documentates the usage of Iodine along with Zeek and [Mininet-sec](https://github.com/mininet-sec/mininet-sec?tab=readme-ov-file#mininet-sec), in order to promote the analysis of DNS Tuneling attacks using Zeek logs. Zeek was installed by [source code](https://zeek.org/get-zeek/).

The system used was Ubuntu 22.04.

## Installation

```
sudo apt-get install iodine
```

## Usage

Iniciate mnsec using the topology defined in *firewall.py* file. [More informations](https://github.com/mayara-santos01/mnsec-docs/blob/main/en/activation.md#2-iniciate-mnsec).

In another terminal, edit the interface listened by Zeek, in order to track the traffic of the mnsec topology. In order to do this, the user must edit the node.cfg file, which is located in the directory /usr/local/zeek/etc/, in case installation from source code, and make the following changes:

```
[zeek]
type=standalone
host=localhost
interface=nettap1-eth2
```

Then, in root mode, start Zeek:

```
cd /usr/local/zeek/bin
./zeekctl
```

Then, run *deploy* on ZeekControl shell to activate Zeek. The user can check if Zeek is running correctly by executing *status* in ZeekControl shell.

In another terminal run:

```
mnsecx fw0 iptables -A FORWARD -d 10.0.0.2 -p udp --dport 53 -j ACCEPT

mnsecx srv2 iodined -f -c -P ChangeMe-123 10.199.199.1/24 iodine.hackinsdn.ufba.br 
```
Due to firewall configurations, perhaps it will be necessary to make some adjusts in order to allow the traffic which will be used to establish a DNS Tunnel.

```
iptables -I FORWARD -i s1+ -j ACCEPT
iptables -I FORWARD -i s2+ -j ACCEPT
```

```
mnsecx o1 iodine -f -P ChangeMe-123 10.0.0.2 iodine.hackinsdn.ufba.br
```



Mininet-sec, such as other versions of Mininet, provides no connexion between its internal components and internet by default, a fact that limitates the possibilities of experimentation in order to the operation of a DNS Tunnel. Nevertheless, it is possible to verify the functioning of the DNS Tunnel through other commands.

```
mnsecx o1 ping -s 1200 -f 10.199.199.1
```

