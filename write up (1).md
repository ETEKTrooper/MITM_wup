We initiate by running the command ipconfig /all in the PowerShell
window to gather the network configuration details of our Target VM.
It's noteworthy that while "physical address" and "MAC address" are
often used interchangeably, on an Ethernet network like this scenario,
"MAC address" is the preferred term. During this process, we also focus
on noting the MAC and router IP, essential for the Man-in-the-Middle
(On-Path Attack) setup.

<img src="media/image1.png" style="width:6.5in;height:4.8125in" />

To review the existing network mappings on the target machine, we input
arp -a in PowerShell. This command reveals the IP-to-MAC address
mappings that the target machine
recognizes.<img src="media/image2.png" style="width:6.4375in;height:4.72917in" />

Within the attack machine using Kali Linux, we launch the Terminal and
run the command ip addr show eth0 to identify the MAC address of eth0,
specifically recording the final four digits,
'80:10'.<img src="media/image3.png" style="width:6.48958in;height:4.94792in" />

We initiate Ettercap, a network security tool for Man-in-the-Middle
attacks, monitoring, and packet capturing. Following its launch, we
proceed with its configuration for our security analysis.

<img src="media/image4.png" style="width:6.4375in;height:4.83333in" />

We use Ettercap to scan the network and find all devices. We pay special
attention to the device with the IP 10.1.16.254 to learn its role and
MAC address.

<img src="media/image5.png" style="width:6.5in;height:4.46875in" />

We set the target machine with the IP address 10.1.16.2 as Target 1.

<img src="media/image6.png" style="width:6.42708in;height:4.39583in" />

We use Ettercap to scan for devices on the network and pay special
attention to the one with IP 10.1.16.254. We note its MAC address and
set up Ettercap to listen in on the traffic between our main target and
this device by tricking them into thinking our attacking machine is the
other. We add this router address to Target 2.

<img src="media/image7.png" style="width:6.45833in;height:4.4375in" />

We select "ARP poisoning" from the MITM menu, a method that tricks
devices into sending their data to us by faking network addresses.

<img src="media/image8.png" style="width:6.47917in;height:4.41667in" />

ARP poisoning is a cyber attack where the attacker sends false ARP
messages to a network. This tricks the network into sending data to the
attacker instead of the correct device. It allows the attacker to
intercept, change, or block data.

<img src="media/image9.png" style="width:6.44792in;height:4.41667in" />

We start here by capturing the traffic with Wireshark, making two
attempts to connect to the router at 10.1.16.254, enabling us to
intercept this communication.

<img src="media/image10.png" style="width:6.39583in;height:4.83333in" />

In this part of the lab, we select the browser to attempt a connection
to the router at the address http://10.1.16.254, investigating whether
the router is configured with a web administration interface. This step
is crucial for understanding the accessible interfaces of network
devices, which can vary based on configuration and security policies.

<img src="media/image11.png" style="width:6.38542in;height:3.42708in" />

Following the web access attempt, we proceed to test another common
administration method by launching PUTTY, a tool for establishing secure
connections. We aim to check if an SSH (Secure Shell) connection to the
router at 10.1.16.254 is possible. After entering the router's IP
address in the Host Name field of PUTTY and selecting Open, the attempt
to establish a connection fails. This step helps us understand the
security configuration of the router, indicating that it does not accept
SSH connections, further emphasizing the importance of knowing the
available and secure methods for managing network devices.

<img src="media/image12.png" style="width:6.42708in;height:4.77083in" />

Both of these connection attempts fail because the router isn't
configured to allow web or SSH access. This setup is common in many
networks as a security measure to minimize potential risks.

<img src="media/image13.png" style="width:6.47917in;height:4.34375in" />

We can observe here that both the MAC address associated with the target
VM and the MAC address associated with the router have been altered,
indicating a successful ARP poisoning attack.

<img src="media/image14.png" style="width:6.41667in;height:4.72917in" />

Next, we launch Wireshark and input "arp" into the display filter box.
Numerous captures will appear, signaling the duplicate use of 10.1.16.2.

<img src="media/image15.png" style="width:6.42708in;height:2.14583in" />

In Wireshark's middle pane, we observe the report indicating that the IP
address is being utilized by two MAC addresses, which suggests ARP
poisoning.

<img src="media/image16.png" style="width:6.39583in;height:2.3125in" />

<img src="media/image17.png" style="width:6.42708in;height:0.75in" />

Next, we open Ettercap and stop the MITM attack.

<img src="media/image18.png" style="width:6.45833in;height:4.39583in" />

We switch back to the target VM and type "arp -a" to view the cached ARP
mappings. This indicates that the MAC address has reverted to its
original state due to the termination of the MITM attack.

<img src="media/image19.png" style="width:4.44792in;height:5.32292in" />

In this tutorial, we explored the basics of MAC and IP addresses,
pivotal for comprehending network communication. Furthermore, we
examined the concept of a Man-in-the-Middle (MitM) attack, a technique
where an attacker intercepts and potentially alters communication
between two parties. For penetration testers, MitM attacks serve as
invaluable tools for simulating real-world threats, aiding in the
identification of network vulnerabilities and the formulation of robust
security measures.
