Mininet Network Topology Experiments (Experiment-3)

👤 Author: Suvankar Pramanik 🎓 Bachelor of Technology (B.Tech) in Computer Science and Engineering 🏫 Adamas University, West Bengal, India | 4th Year | CGPA: 8.6/10

📌 Objective
To create and configure a simple network topology using an open-source network virtualization tool (Mininet) and verify communication between the virtual network nodes.

📋 Task Overview
Setup an Ubuntu Linux Virtual Machine (using VirtualBox).

Install Mininet.

Create network topologies with two different types of node numbers.

Inspect links and test connectivity using basic Linux networking commands (ping, pingall, ip addr).

Stop and clean up the network after completion.

🛠️ Environment Setup
Prerequisites
Ubuntu Linux (running in VirtualBox)

Terminal access with sudo privileges

Installation
Mininet and its dependencies were installed using the following command:

bash
sudo apt install mininet -y
Dependencies installed include: openvswitch-switch, python3-netifaces, socat, iperf, etc.

🧪 Lab Experiments & Results
The lab was conducted in two main phases:

Built-in Topologies: Using Mininet's command-line arguments.

Custom Python Topologies: Creating custom scripts for specific network structures.

Part 1: Built-in Topologies (mn command)
Experiment 1.1: Minimal Topology (2 Hosts)
A simple topology with 1 switch and 2 hosts.

bash
sudo mn --topo minimal
Nodes: h1, h2, s1

Links: h1-eth0 <-> s1-eth1, h2-eth0 <-> s1-eth2

Connectivity: pingall resulted in 0% dropped (2/2 received).

Experiment 1.2: Single Topology (4 Hosts)
A topology with 1 switch and 4 hosts connected to it.

bash
sudo mn --topo single,4
Nodes: h1, h2, h3, h4, s1

Links: All hosts connected to s1 (ports 1-4).

Connectivity: pingall resulted in 0% dropped (12/12 received).

Experiment 1.3: Linear Topology (2 & 4 Switches)
A chain of switches, each with a host attached.

bash
sudo mn --topo linear,2
sudo mn --topo linear,4
Linear 2: h1 - s1 - s2 - h2. Ping test: 0% dropped (2/2).

Linear 4: h1 - s1 - s2 - s3 - s4 - h4 (with hosts on each switch). Ping test: 0% dropped (12/12).

Experiment 1.4: Tree Topology (Depth 2 & 3)
A hierarchical tree topology.

bash
sudo mn --topo tree,depth=2,fanout=2
sudo mn --topo tree,depth=3,fanout=2
Tree Depth 2: 3 switches, 4 hosts. Ping test: 0% dropped (12/12).

Tree Depth 3: 7 switches, 8 hosts. Ping test: 0% dropped (56/56).

Part 2: Custom Python Topologies
Custom scripts were created using nano and executed with sudo python3 <filename>.py.

Experiment 2.1: Reversed Topology
A custom script (reversed_topology.py) was written to create a specific 2-switch, 2-host topology with non-standard connections.

Code Snippet:

python
from mininet.topo import Topo
from mininet.net import Mininet
from mininet.cli import CLI
from mininet.node import OVSController

class ReversedTopo(Topo):
    def build(self):
        s1 = self.addSwitch('s1')
        s2 = self.addSwitch('s2')
        h1 = self.addHost('h1')
        h2 = self.addHost('h2')
        
        # Custom Links
        self.addLink(s1, s2)
        self.addLink(s1, h2)
        self.addLink(s2, h1)

def run():
    topo = ReversedTopo()
    net = Mininet(topo=topo, controller=OVSController)
    net.start()
    CLI(net)
    net.stop()

if __name__ == '__main__':
    run()
Verification: links command confirmed the reversed connection (s1-eth2 <-> h2-eth0, s2-eth2 <-> h1-eth0).

Connectivity: pingall resulted in 0% dropped.

Experiment 2.2: Torus Topology (4x4 Grid)
A script (torus_topology.py) was created to build a 4x4 torus network (16 switches, 16 hosts).

Key Features:

Iterates through a 4x4 grid of switches.

Connects each switch to its right neighbor (wrapping around).

Connects each switch to its down neighbor (wrapping around).

Attaches one host to each switch.

Note: The script enables Spanning Tree Protocol (STP) on all switches to prevent broadcast storms caused by loops in the torus topology.

Execution & Results:

Nodes: h0 through h15, s0 through s15.

STP: Enabled successfully. The script waited 30 seconds for STP to converge before starting the network.

Connectivity: pingall resulted in 0% dropped (240/240 received).

🧹 Cleanup
After each experiment, the network was stopped using exit in the Mininet CLI, followed by a cleanup command to remove any stale processes or virtual interfaces:

bash
sudo mn -c
This ensures no residual OpenFlow controllers or OVS bridges interfere with subsequent experiments.

📝 Conclusion
The lab successfully demonstrated the creation and configuration of various network topologies (Minimal, Single, Linear, Tree, and custom Torus) using Mininet on an Ubuntu VM. Communication between nodes was verified using ping and pingall, confirming 0% packet loss across all tested topologies. The custom Torus topology required STP configuration to handle network loops correctly.

👤 Author

| Field       | Details                                                        |
|-------------|----------------------------------------------------------------|
| Name        | Suvankar Pramanik                                              |
| Degree      | Bachelor of Technology (B.Tech) — Computer Science and Engineering |
| University  | Adamas University, West Bengal, India                          |
| Current Year | 4th Year                                                      |
| CGPA        | 8.6/10                                                         |
| Location    | Kolkata, West Bengal, India                                    |

This project was created as part of practical learning in Cloud Computing using Mininet.
