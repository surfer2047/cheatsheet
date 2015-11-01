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

Ok, let us write some