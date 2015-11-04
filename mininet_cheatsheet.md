#Mininet installation testing

```bash
$ sudo mn --test pingall
$ sudo mn --test pingpair
```
for the documentation help

```bash
mn --help
```

##some basic commands in mininet

```bash

mn> help  #Display some help commands
mn> nodes #Display all the nodes controller, switch and hosts of the topology
mn> h2 ping h1 #Ping h1 form h2
mn> h2 ifconfig #Display the ip configuration of h2 host with h2-eth0
mn> xterm h2  #Connect to the terminal of h2 host, where we can run all the commands
mn> dump #Display all the information about the nodes
mn> s1 ifconfig #This will display all the physical interface of host as well as the logical interface of the switch
mn> s1 ping 8.8.8.8 #This will ping the Google public dns from the switch
mn> c0 ping -c 2 8.8.4.4
mn> pingall #This will ping two nodes in topology h1 and h2
mn> net #Show the links connection.
```

In mininet switch by default run as the root namespace, so it can has access to all the network interface of the physical host system.

In mininet it is possible to place all the hosts, switch, controller into their own namespace, using the flag `--innamespace` option.

In mininet the network is only virtualize, all the nodes in the mininet network share the same process, this can be verify using ps the commands on all nodes
```bash
mn> h1 ps -a 
mn> h2 ps -a 
mn> c0 ps -a 
mn> s1 ps -a 
```
On running the above command, the output will be the same, even on listing the files on each nodes will result in same info.

The real advantage of using mininet comes, each host on mininet can run any linux command that support by your hosts on which mininet is running. we can also run webserver in hosts.

Let us run simple web server in host h2 inside mininet topology.

```bash
$ h1 python -m SimpleHTTPServer 999 & 
```

The above command will run the simple HTTP webserver available in our linux system to the host h1, you can get the ip address of the host h1 by running the command `dump` 
To connect to the webserver running in host 

```bash 
$ h1 wget 10.0.0.1:999 -O data
```

If mininet crashes some reason, we need to clean up 

```bash
mn> exit
$ sudo mn -c 
```

```bash
sudo mn --test pingpair
```
The above test is called the regression test in mininet.

```bash
$ sudo mn --test iperf
```
The above command create the same minimalist topology and run the iperf server on h1 and run iperf

Ok, let us write some python code 

```python
from mininet.net import Mininet
from mininet.topolib import TreeTopo

tree4 = TreeTopo(depth=2, fanout=2)
netwok = Mininet(topo=tree4)
netwok.start()
h1, h4 = netwok.hosts[0], network.hosts[3]
print h1.cmd("pinging h1 from h4 %s" %h4.IP())
network.stop()
```

save the above code and run this as `python <file_name>`. This method is to creating the topology from python file

Ok, new let us create the topology from command line
```python
$ sudo mn --topo linear,3

```

This above command will create the linear topology, 3 switches are connected with each other having single host.

```python
$sudo mn --topo single,3
```
The above command will create a single switch with 3 hosts connected to the switch

##Setting up the link parameters in mininet.
```python
$sudo mn --link tc,bw=10,delay=10ms

```

##Vebosity in mininet for inspecting 

```python

$ sudo mn -v debug
$ sudo mn -v output
```

```bash
$ tcpdump -n #Don't do the name resolution.

```bash
mn> iperf
```

```bash
mn > iperfudp
```

##Creating the customs topology in mininet.

```python

"""Custom topology example

Two directly connected switches plus a host for each switch:

   host --- switch --- switch --- host

Adding the 'topos' dict with a key/value pair to generate our newly defined
topology enables one to pass in '--topo=mytopo' from the command line.
"""

from mininet.topo import Topo

class MyTopo( Topo ):
    "Simple topology example."

    def __init__( self ):
        "Create custom topo."

        # Initialize topology
        Topo.__init__( self )

        # Add hosts and switches
        leftHost = self.addHost( 'h1' )
        rightHost = self.addHost( 'h2' )
        leftSwitch = self.addSwitch( 's3' )
        rightSwitch = self.addSwitch( 's4' )

        # Add links
        self.addLink( leftHost, leftSwitch )
        self.addLink( leftSwitch, rightSwitch )
        self.addLink( rightSwitch, rightHost )


topos = { 'mytopo': ( lambda: MyTopo() ) }

```

By default, the hosts start with randomly assigned mac address, this make debugging tough. to set the mac address small and unique, mininet provide the special options

```bash
sudo mn --mac
```

To open all the xterm windo for all nodes, just pass the -x flag to mn command as shown below

```bash
$ sudo mn -x
```

To view the flow entries in the switch, we can use the dpctl commands in the xterm windows of the switches

```bash
switch$ dpctl dump-flows tcp:127.0.0.1:6634
```

`dpctl` is the command line utility to get the information about the switch. we can query about other options that dpctl take using 
```bash
mn> dpctl --help
```

To check the time that take to setup and teardown the mininet topology is called mininet benchmarking, which can be done by using `test none` command

```bash
$ sudo mn --test none
```

###Namespace in Mininet
By default in mininet `switch` and `Controller` are placed into the root name space, while hosts are kept into their own namespace.
To change the namespace we can use the options --innamespace 

```bash
$ sudo mn --innamespace --switch user 
```

###Mininet CLI
####Running Python interpeter in mininet

```python
$ sudo many
mn> py 'hello' + 'world'
mn> py h1.IP()
mn> py locals #Print the local variables.
```


##Link Managing in mininet.
###Link up and down

```bash
mn> link s1 h1 down
mn> h1 ping h2 #This won't ping h2 as the link from h1 to h2 is down.
mn> link h1 s1 up #This command will bring the link up.
mn> xterm h1 h2 #This will open the xterm of two host h1 and h2, 
```

##Using Remote Controller in Mininet.
This will be useful if we are running the the controller in the remote machines, having customs controller, which can be floodlight, Opendaylight, Pox, Beacon, Ryu and many other controller of your choice.

#Terms
- **Table miss**
- **Packet In ** Message send by the switch to the controller


- ** Flow table ** 

- ** Packet out message ** This message tell the switch what to do with this packet.

- ** flow modification message ** 
It instruct the switch to enter new entry flow table in the switch
- **Flow entry**
    have match fields to match the incomming packets, 
    and also have further instruction.
    flow intery given by the controller.
    flow entry have the priority, 

- ** Flow table ** 

- ** Group table **
    special kind of flow table, consisit of the identifier and action buckets.
    Action Bucket  --send out  ALL Ports 2,3,4,5

- ** Openflow channel ** 
        To connect for one or more controller. 
        it communication setup, perodic health checking and many other.