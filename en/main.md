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

Then, start Zeek:

```
sudo su
cd /usr/local/zeek/bin
./zeekctl
```

The user can check if Zeek is running correctly by executing *status* in ZeekControl shell.

In another terminal run:

```
mnsecx fw0 iptables -A FORWARD -d 10.0.0.2 -p udp --dport 53 -j ACCEPT

mnsecx srv2 iodined -f -c -P ChangeMe-123 10.199.199.1/24 iodine.hackinsdn.ufba.br 
```

```
mnsecx o1 iodine -f -P ChangeMe-123 10.0.0.2 iodine.hackinsdn.ufba.br
```

