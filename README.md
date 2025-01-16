# Steps from R7

```shell
# Show connected routes
show ip route # And look for C (if there is a B -> it's a border router)
# Show the ospf neighbors (routers)
show ip ospf nei # from here we can get the addresses of the interfaces
# If BGP use 
show ip bgp nei # shows the id and ip of nei
# We can use summary for shorter answer but it doesn't show the id
show ip bgp summ # It also shows the AS numbers
# show the ip of the interface with which we are connected
show interface [FastEthernet x/y] # the name of the interface is shown by show ip route C
# show the devices if any
ping [network]
# Show the areas if any
show ip ospf database
```

## Hack to find if the connected device is router or PC 

```shell
# Find the connected:
show ip route C
# Find the ip addr of each one:
ping [network]
# connect to device:
telnet [ip-addr]
```

## bonus

```shell
# Only if enabled
show cdp nei 
show lldp nei
```

## General info

The output of the `show ip ospf database` command provides information about the various OSPF LSAs (Link-State Advertisements) that are part of the router's link-state database. Below is an explanation of the sections in your output:

------

### **1. Router Link States (Type 1 LSAs)**

#### **Fields Explanation**:

- **Link ID**: The Router ID of the router that originated this LSA.
- **ADV Router**: The Router ID of the router advertising this LSA (same as Link ID in most cases for Router LSAs).
- **Age**: The time (in seconds) since the LSA was last updated or originated.
- **Seq#**: The sequence number, used to identify the most recent version of the LSA.
- **Checksum**: A checksum value for error detection.
- **Link count**: The number of links (interfaces) advertised by the router in the LSA.

#### **Interpretation**:

- **2.2.2.2**: The current router (R2) is advertising 3 links (connected interfaces) in Area 0.
- **1.1.1.1**: Another router with Router ID 1.1.1.1 is advertising 1 link in Area 0.
- **3.3.3.3**: A third router with Router ID 3.3.3.3 is also advertising 1 link in Area 0.

Router LSAs describe a router's directly connected links and their metrics.

------

### **2. Net Link States (Type 2 LSAs)**

#### **Fields Explanation**:

- **Link ID**: The IP address of the Designated Router (DR) for the multi-access network.
- **ADV Router**: The Router ID of the DR advertising this LSA.
- **Age**: The time (in seconds) since the LSA was last updated or originated.
- **Seq#**: The sequence number to identify the most recent version of the LSA.
- **Checksum**: A checksum value for error detection.

#### **Interpretation**:

- **10.1.1.2**: The DR for the network with IP 10.1.1.2 (likely an Ethernet or multi-access network) is R2 (ADV Router = 2.2.2.2).
- **10.1.2.2**: The DR for the network with IP 10.1.2.2 is R3 (ADV Router = 3.3.3.3).

Net Link States describe multi-access networks and the routers connected to them.

------

### **3. Summary Net Link States (Type 3 LSAs)**

#### **Fields Explanation**:

- **Link ID**: The destination network being advertised.
- **ADV Router**: The Router ID of the router advertising the summary route.
- **Age**: The time (in seconds) since the LSA was last updated or originated.
- **Seq#**: The sequence number of the LSA.
- **Checksum**: A checksum value for error detection.

#### **Interpretation**:

- **10.1.3.0**: The summary route for the 10.1.3.0 network is being advertised by R3 (ADV Router = 3.3.3.3).
- **10.1.4.0**: The summary route for the 10.1.4.0 network is also being advertised by R3.

Summary LSAs are used to advertise inter-area routes between OSPF areas.

------

### **4. Type-5 AS External Link States (External Routes)**

#### **Fields Explanation**:

- **Link ID**: The external network being advertised.
- **ADV Router**: The Router ID of the Autonomous System Boundary Router (ASBR) advertising the external route.
- **Age**: The time (in seconds) since the LSA was last updated or originated.
- **Seq#**: The sequence number of the LSA.
- **Checksum**: A checksum value for error detection.
- **Tag**: An optional field used for policy-based routing or administrative tagging.

#### **Interpretation**:

- Networks like **10.2.1.0**, **10.2.2.0**, etc., are external networks being advertised by R1 (ADV Router = 1.1.1.1).
- Networks **10.3.1.0**, **10.3.2.0**, etc., are also external networks advertised by R1.

Type-5 LSAs are used to advertise external routes injected into OSPF from other routing protocols (e.g., RIP, EIGRP) or static routes.

------

### **Summary of Output**

1. **Router LSAs**: Show information about routers in Area 0 and the number of interfaces they advertise.
2. **Net Link States**: Indicate the designated routers for multi-access networks in Area 0.
3. **Summary Net Link States**: Display inter-area routes advertised by other routers.
4. **Type-5 AS External Link States**: Advertise external routes injected into the OSPF domain by an ASBR.

This information helps visualize the network topology and routing details in an OSPF domain.
