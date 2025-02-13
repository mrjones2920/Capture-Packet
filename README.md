# Capture-Packet

<h1>Activity overview </h1>

As a security analyst, it’s important to know how to capture and filter network traffic in a Linux environment. You’ll also need to know the basic concepts associated with network interfaces.

In this lab activity, you’ll perform tasks associated with using tcpdump to capture network traffic. You’ll capture the data in a packet capture (p-cap) file and then examine the contents of the captured packet data to focus on specific types of traffic.

Let’s capture network traffic!

This exemplar is a walkthrough of the previous Qwiklab activity, including detailed instructions and solutions. You may use this exemplar if you were unable to complete the lab and/or you need extra guidance in competing lab tasks. You may also refer to this exemplar to prepare for the graded quiz in this module.

<h1>Scenario</h1>
You’re a network analyst who needs to use tcpdump to capture and analyze live network traffic from a Linux virtual machine.

The lab starts with your user account, called analyst, already logged in to a Linux terminal.

Your Linux user's home directory contains a sample packet capture file that you will use at the end of the lab to answer a few questions about the network traffic that it contains.

Here’s how you’ll do this: First, you’ll identify network interfaces to capture network packet data. Second, you’ll use tcpdump to filter live network traffic. Third, you’ll capture network traffic using tcpdump. Finally, you’ll filter the captured packet data.

<h1>Task 1. Identify network interfaces</h1>
In this task, you must identify the network interfaces that can be used to capture network packet data.

1.Use ifconfig to identify the interfaces that are available:

![image](https://github.com/user-attachments/assets/fb98298f-01bd-4144-8678-a1f55ff5c8a0)

This command returns output similar to the following:

![image](https://github.com/user-attachments/assets/2ee9ba5f-3fbd-4fb0-b23b-d36fcccc5974)


The Ethernet network interface is identified by the entry with the eth prefix.

So, in this lab, you'll use eth0 as the interface that you will capture network packet data from in the following tasks.

2. Use tcpdump to identify the interface options available for packet capture:

![image](https://github.com/user-attachments/assets/2823a9d7-733d-4643-accd-ac55a61e6547)

This command will also allow you to identify which network interfaces are available. This may be useful on systems that do not include the ifconfig command.

<h1>Task 2. Inspect the network traffic of a network interface with tcpdump</h1>
In this task, you must use tcpdump to filter live network packet traffic on an interface.

- Filter live network packet data from the eth0 interface with tcpdump:

![image](https://github.com/user-attachments/assets/888f6827-9471-4c67-972d-b16076eef1d1)

This command will run tcpdump with the following options:

- -i eth0: Capture data specifically from the eth0 interface.
- -v: Display detailed packet data.
- -c5: Capture 5 packets of data.

Now, let's take a detailed look at the packet information that this command has returned.

Some of your packet traffic data will be similar to the following:

![image](https://github.com/user-attachments/assets/96950fbe-f1a2-46cd-a71a-8b9b6efb99db)

The specific packet data in your lab may be in a different order and may even be for entirely different types of network traffic. The specific details, such as system names, ports, and checksums, will definitely be different. You can run this command again to get different snapshots to outline how data changes between packets.

<h1>Exploring network packet details</h1>
In this example, you’ll identify some of the properties that tcpdump outputs for the packet capture data you’ve just seen.

1. In the example data at the start of the packet output, tcpdump reported that it was listening on the eth0 interface, and it provided information on the link type and the capture size in bytes:

![image](https://github.com/user-attachments/assets/7782d39d-afd8-4467-a978-0c6ca8a74420)

2. On the next line, the first field is the packet's timestamp, followed by the protocol type, IP:

![image](https://github.com/user-attachments/assets/34bd3686-ffa8-49be-8ac1-7c38c0b58120)

3. The verbose option, -v, has provided more details about the IP packet fields, such as TOS, TTL, offset, flags, internal protocol type (in this case, TCP (6)), and the length of the outer IP packet in bytes:

![image](https://github.com/user-attachments/assets/5d4445cc-07d4-40a9-8b8a-c9d68e78782a)

The specific details about these fields are beyond the scope of this lab. But you should know that these are properties that relate to the IP network packet.

4. In the next section, the data shows the systems that are communicating with each other:

![image](https://github.com/user-attachments/assets/894592ae-be0c-449f-b399-5ad27c3e0b6f)

By default, tcpdump will convert IP addresses into names, as in the screenshot. The name of your Linux virtual machine, also included in the command prompt, appears here as the source for one packet and the destination for the second packet. In your live data, the name will be a different set of letters and numbers.

The direction of the arrow (>) indicates the direction of the traffic flow in this packet. Each system name includes a suffix with the port number (.5000 in the screenshot), which is used by the source and the destination systems for this packet.

5. The remaining data filters the header data for the inner TCP packet:

![image](https://github.com/user-attachments/assets/00f7c9f6-0a2b-4ec3-b818-ca607ba1e306)

The flags field identifies TCP flags. In this case, the P represents the push flag and the period indicates it's an ACK flag. This means the packet is pushing out data.

The next field is the TCP checksum value, which is used for detecting errors in the data.

This section also includes the sequence and acknowledgment numbers, the window size, and the length of the inner TCP packet in bytes.

<h1>Task 3. Capture network traffic with tcpdump</h1>
In this task, you will use tcpdump to save the captured network data to a packet capture file.

In the previous command, you used tcpdump to stream all network traffic. Here, you will use a filter and other tcpdump configuration options to save a small sample that contains only web (TCP port 80) network packet data.

1. Capture packet data into a file called capture.pcap:

![image](https://github.com/user-attachments/assets/73bd254e-dc44-411b-967f-45b1a535816b)

You must press the ENTER key to get your command prompt back after running this command.

This command will run tcpdump in the background with the following options:

- -i eth0: Capture data from the eth0 interface.
- -nn: Do not attempt to resolve IP addresses or ports to names.This is best practice from a security perspective, as the lookup data may not be valid. It also prevents malicious actors from being alerted to an investigation.
- -c9: Capture 9 packets of data and then exit.
- port 80: Filter only port 80 traffic. This is the default HTTP port.
- -w capture.pcap: Save the captured data to the named file.
- &: This is an instruction to the Bash shell to run the command in the background.

This command runs in the background, but some output text will appear in your terminal. The text will not affect the commands when you follow the steps for the rest of the lab.

2. Use curl to generate some HTTP (port 80) traffic:

![image](https://github.com/user-attachments/assets/624cadfa-03dd-4bfe-9e46-bedef0c329a7)

When the curl command is used like this to open a website, it generates some HTTP (TCP port 80) traffic that can be captured.

3. Verify that packet data has been captured:

![image](https://github.com/user-attachments/assets/cd5e7947-0ede-4f3e-b727-6081fcfebfff)

Note: The "Done" in the output indicates that the packet was captured.

<h1>Task 4. Filter the captured packet data</h1>
In this task, use tcpdump to filter data from the packet capture file you saved previously.

1.Use the tcpdump command to filter the packet header data from the capture.pcap capture file:

![image](https://github.com/user-attachments/assets/94e16cff-132d-4652-8130-e069bdd8a843)

This command will run tcpdump with the following options:

- -nn: Disable port and protocol name lookup.
- -r: Read capture data from the named file.
- -v: Display detailed packet data.

You must specify the -nn switch again here, as you want to make sure tcpdump does not perform name lookups of either IP addresses or ports, since this can alert threat actors.

This returns output data similar to the following:

![image](https://github.com/user-attachments/assets/4be3ea37-e6ee-476d-b780-f065b5dd7285)

As in the previous example, you can see the IP packet information along with information about the data that the packet contains.

2. Use the tcpdump command to filter the extended packet data from the capture.pcap capture file:

![image](https://github.com/user-attachments/assets/e325bdfe-8b2a-4403-8fe7-daec0e7c13fd)

This command will run tcpdump with the following options:

- -nn: Disable port and protocol name lookup.
- -r: Read capture data from the named file.
- -X: Display the hexadecimal and ASCII output format packet data. Security analysts can analyze hexadecimal and ASCII output to detect patterns or anomalies during malware analysis or forensic analysis.

Note: Hexadecimal, also known as hex or base 16, uses 16 symbols to represent values, including the digits 0-9 and letters A, B, C, D, E, and F. American Standard Code for Information Interchange (ASCII) is a character encoding standard that uses a set of characters to represent text in digital form.
