# Cisco Packet Tracer Lab - Multilayer Switching

This lab continues from a <a href= "https://github.com/ro-drick/VLAN-ROAS-Configuration">previous exercise</a>, with SW2 replaced by a multilayer switch. The goal of this lab is to configure inter-VLAN routing and ensure connectivity to external networks using a Layer 3 point-to-point connection between SW2 and R1.

## Lab Setup

The network consists of the following components:
- **SW1**: Layer 2 Switch
- **SW2**: Multilayer Switch (3560-24PS)
- **R1**: Router (2911)
- **PC1, PC2**: VLAN10 (10.0.0.0/26)
- **PC3, PC4**: VLAN30 (10.0.0.128/26)
- **PC5**: VLAN20 (10.0.0.64/26)
- **PC6, PC7**: VLAN10 (10.0.0.0/26)
- **Internet Cloud**: Simulated internet for testing external connectivity (ping 1.1.1.1)

<img src="https://github.com/ro-drick/VLAN-ROAS-Configuration-Part-2/blob/main/VLANs_part3.PNG">

## Task Summary

### 1. Replace ROAS with Point-to-Point Layer 3 Connection
- Remove the Router-on-a-Stick (ROAS) configuration on **R1** and **SW2**.
- Establish a Layer 3 connection between **R1** and **SW2** using the IP addresses shown in the network diagram.
    - **R1 (G0/0)**: 10.0.0.193/30
    - **SW2 (G1/0/1)**: 10.0.0.194/30
- Configure a default route on **SW2**, pointing to **R1's G0/0** interface (10.0.0.193) as the next hop.

### 2. Configure SVIs on SW2
- Configure **SVIs** (Switch Virtual Interfaces) on **SW2** for each VLAN.
- Use the last usable IP address from each subnet:
    - VLAN10: 10.0.0.62/26
    - VLAN20: 10.0.0.126/26
    - VLAN30: 10.0.0.190/26
- Assign these IPs to the respective SVIs for inter-VLAN routing.

### 3. Test Inter-VLAN Connectivity
- Use **ping** to test connectivity between hosts on different VLANs.
    - Example: Ping from **PC1 (10.0.0.1)** in **VLAN10** to **PC5 (10.0.0.65)** in **VLAN20**.

### 4. Test External Connectivity
- Test internet connectivity by pinging the external IP address **1.1.1.1** from any host.
- Ensure that default routes and internet access are properly configured through **R1**.

## Verification

- **SVI Configuration**: Ensure each VLAN has a correctly configured SVI on **SW2**.
- **Routing**: Verify that **SW2** can route between VLANs and has a default route to **R1**.
- **Trunking**: Confirm that the trunk between **SW1** and **SW2** is working properly and carries all VLANs.
- **Inter-VLAN Ping**: Test pings between hosts in different VLANs to verify inter-VLAN routing.
- **Internet Ping**: Test pinging **1.1.1.1** to confirm internet connectivity through **R1**.

## Commands Used

### On SW2 (Multilayer Switch):
```bash
# Configure Layer 3 point-to-point connection
interface GigabitEthernet1/0/1
 no switchport
 ip address 10.0.0.194 255.255.255.252
 no shutdown

# Configure SVIs for VLANs
interface Vlan10
 ip address 10.0.0.62 255.255.255.192
 no shutdown

interface Vlan20
 ip address 10.0.0.126 255.255.255.192
 no shutdown

interface Vlan30
 ip address 10.0.0.190 255.255.255.192
 no shutdown

# Configure default route to R1
ip route 0.0.0.0 0.0.0.0 10.0.0.193
```
### On R1 (Router):
```bash
# Configure point-to-point connection to SW2
interface GigabitEthernet0/0
 ip address 10.0.0.193 255.255.255.252
 no shutdown
```
## Results
- Inter-VLAN Routing: Successful ping between PCs in different VLANs.
- Internet Connectivity: Successful ping to external IP 1.1.1.1 from any host.
- Trunking: The trunk between SW1 and SW2 is fully functional.
## Conclusion
This lab demonstrates how to replace ROAS with a Layer 3 point-to-point connection, configure SVIs for VLANs, and ensure both inter-VLAN and external connectivity. All tasks were completed successfully.
## Acknowledgements


Special thanks to **Jeremy's IT Lab** for providing valuable resources and tutorials that greatly contributed to the completion of this exercise. His in-depth explanations and practical demonstrations have been instrumental in enhancing my understanding of Cisco networking concepts and the effective use of Packet Tracer.

For more information and additional resources, visit [Jeremy's IT Lab](https://jeremysitlab.com/) and check out his YouTube for the full course, [Jeremy's IT Lab Free CCNA 200-301 | Complete Course](https://www.youtube.com/playlist?list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ)
