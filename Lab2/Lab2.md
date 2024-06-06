# DANZ Monitoring Fabric Lab2
## Introduction
In this lab we will explore the Packet Recorder Node of DANZ Monitoring Fabric. In this Lab the Fabric Switches and interfaces have already been configured. 

## Topology
The Packet Recorder Lab is only slighly different than the previous lab. The packet recorder is part of the tools section, and the available options under the client have an option to play a large amount of traffic. 

![alt text](media/PR_Topology.png)

## Login Info
Username: admin\
Password: rn_labs@2023

## Add the Recorder Node
Go to Monitoring > Packet Recorders

![alt text](media/nav_pr.png)

Under **Devices** Select the **+**

![alt text](media/add_pr1.png)

Fill in the below information:

Name: **packet-recorder814** \
MAC Address: **42:01:0a:0a:0c:28** \
Packet Age: **300 Mins**

Select ![alt text](media/save.png)

![alt text](media/add_pr2.png)

Back on the Packet Recorders Page you may see a Red X under Connected. Hit Refresh until you see a Green Check mark under connected.

![alt text](media/add_pr3.png)

## Add Packet Recorder Interface
Remaining on the Packet Recorders Menu.

Under Interfaces select the ***+**

![alt text](media/add_pr_int1.png)

Fill in with a interface information

Name: **Recorder-Interface** \
Switch **(Delivery)** \
Interface: **ethernet3**

Select ![alt text](media/save.png)


![alt text](media/add_pr_int2.png)

## Create a Policy
Navigate to Monitoring > Policies
![ICMP POLICY 1](media/ICMP_POLICY1.png)

There are currently no policies configured. Select the **+** icon to add a policy
![ICMP POLICY 2](media/ICMP_POLICY2.png)

Name the Policy **ICMP-POLICY** \
Select ![next](media/next.png)
![ICMP POLICY 3](media/ICMP_POLICY3.png)

Select ICMP from the list of Match Rule shortcuts \
Select ![next](media/next.png)
![ICMP POLICY 4](media/ICMP_POLICY4.png)

Select the **+** to add an interface

![ICMP POLICY 5](media/ICMP_POLICY5.png)

Select 1 Filter interface FILTER1 (ethernet2)

![ICMP POLICY 6](media/ICMP_POLICY6.png)

Select ![append](media/append.png)

Then ![next](media/next.png)

Instead of a delivery interface, the traffic for this policy will be send to the Packet Recorder.

On the left hand side of the policy select **6. Packet Recorder**

Select the **+**

![alt text](media/pr_policy1.png)

Select the previously created Packet Recorder Interface

![alt text](media/pr_policy2.png)

Select ![alt text](media/append.png)

Select ![alt text](media/save.png)

## Test the Policy
To Verify that the packets are being delivery to the Packet Broker



Open the Client CLI

![Client CLI](media/client_CLI.png)

Run the following command **ping -c 5 10.0.0.2**

![alt text](media/ping_5.png)

While the ping is running go back to the DMF Controller UI. 

Select the hyperlink of the newly created **ICMP-POLICY**

![alt text](media/pr_test1.png)

On the left of the page, scorll down Select Operational > Interfaces > Filter Interfaces

Review the Packet Count and Byte Count. 

![alt text](media/pr_test2.png)

Note: If you are seeing a zero value for Byte and Packet count, review the previosly created policy and steps.

Now that have confirmed that Policy is receiving and sending packets, we will review the information in the packet recorder.

Go to Monitoring > Packet Recorders

![alt text](media/nav_pr.png)

Scroll to the bottom of the page under **Query**

Let's run a basic ICMP Query

Query Type: **IPv4**
Last: **10 minutes**
IP Protocol: **ICMP (1)**

Select Submit

![alt text](media/pr_test3.png)

Next to the query section you will see the ouptut of the query:

![alt text](media/pr_test4.png)

## Capture All Traffic 
ICMP works great for testing purposes, but lets get some more interesting traffic into the Packet Recorder.

Navigate to Monitoring > Policies

![alt text](media/nav_policies.png)

Let add a new Policy that will capture all traffic.

![](media/capt_all_1.png)

Name: All-TRAFFIC

![alt text](media/next.png)

![alt text](media/pol_all_1.png)

Select the **+** under Rules

![alt text](media/pol_all_2.png)

Configure the policy by leaving the sequence number of 1. Uncheck Match Rule Group. Leave Match All Traffic checked.

![alt text](media/append.png)

![alt text](media/next.png)

![alt text](media/pol_all_3.png)

Select the **+** to add an interface

![ICMP POLICY 5](media/ICMP_POLICY5.png)

Select 1 Filter interface FILTER1 (ethernet2)

![ICMP POLICY 6](media/ICMP_POLICY6.png)

Select ![append](media/append.png)

Then ![next](media/next.png)

On the left hand side of the policy select **6. Packet Recorder**

Select the **+**

![alt text](media/pr_policy1.png)

Select Recorder Interface

![alt text](media/pr_policy2.png)

Select ![alt text](media/append.png)

Select ![alt text](media/save.png)

Since ICMP is included in all traffic DMF makes you aware of an overlapping Policies by creating a 3rd policy by default. 

![alt text](media/all_ver_1.png)

There may be some use cases where you want to have these overlapping policies. To simply our configuration we will simply delete the old ICMP-POLICY.

## Delete ICMP-POLICY
Locate the Hamburger next to ICMP-POLICY and select Delete

![alt text](media/all_ver_2.png)

Submit

![alt text](media/all_ver_3.png)




## Verify and Test the Policy


![Client CLI](media/client_CLI.png)

Run the following command **ping -c 5 10.0.0.2**

![alt text](media/ping_5.png)

While the ping is running go back to the DMF Controller UI. 

Select the hyperlink of the newly created **ALL-TRAFFIC**

![alt text](media/all_ver_4.png)

On the left of the page, scorll down Select Operational > Interfaces > Filter Interfaces

Review the Packet Count and Byte Count. 

![alt text](media/all_ver_5.png)

## Generate Traffic
Now that we have tested that our Policy is seeing traffic, let generate some traffic from the client and run some queiries.

Start to gnerate traffic from the client:

![alt text](media/send_traffic.png)

Navigate to the Packet Recorder


![alt text](media/nav_pr.png)

Clear any existing inputs and run a new query

New Query:
Query Type: **Size**
Last: **10 Minutes**

Submit

![alt text](media/query_pr_all.png)

*Note: The traffic can take a while to finishing running so if you run the query mutliple times, you may see the size increase*

The ouput of this query will simply provide how much traffic was collected during that

![alt text](media/query_pr_all_2.png)

Let run another query that will provide a report that will show the statistics of traffic on an application level.

Depending on how much time has elapsed you may need to increase to a 20 min timer:

Clear Inputs

Application Query:
Query Type: **Application**
Last: **10 Minutes**

Submit

![alt text](media/query_pr_all_3.png)

Review the Query results:

![alt text](media/query_pr_all_4.png)

*Depending on how much time has elapsed you may need to increase the time*

You can also query the packet recorder to produce a pcap file that you can download to your desktop.

Clear existing Inputs

Query Type: **Packets**
Last: **10 minutes**

Keep the rest default and Select Submit

![alt text](media/query_pr_all_5.png)

The output form that query will provide you with a hyperlink to the 10dot IP address used in the lab. The 10dot space for the lab is unreachable. You will need to copy the hyperlink and replace the 10dot IP address with the Public IP address displayed in your web browser.

![alt text](media/query_pr_all_6.png)

New Link based on the lab used to generate this output

https://34.16.54.230/pcap/__packet_recorder__/coalesced-packet-2024-04-16-21-50-28-52a55bc2.pcap

Put the updated URL into your web browser and it will download the PCAP to you Local Computer. 

You can review the pcap file in Wireshark locally.