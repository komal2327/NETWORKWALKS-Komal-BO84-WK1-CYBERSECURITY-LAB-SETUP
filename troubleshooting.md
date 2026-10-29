# 🐛 Troubleshooting Guide — Kali Linux Virtual Network

## 1. Purpose

This document provides a systematic approach to diagnosing common network connectivity and DNS resolution problems in a Kali Linux virtual machine running in Oracle VirtualBox.

The objective is to identify the affected component, investigate the configuration, apply an appropriate correction, and verify the result.

## 2. Common Issues and Diagnostic Steps

### Issue A: Kali Linux Has No IP Address

**Possible causes**
- Network adapter is disabled.
- The VM is connected to the wrong virtual network.
- DHCP is disabled or unavailable.
- The guest network interface is not active.

**Diagnostic command**

```bash
ip -br addr
```

**Investigation**
1. Identify the expected network interface.
2. Check whether it is active.
3. Check whether an IPv4 address has been assigned.
4. Review the VM's network adapter and NAT Network settings.

**Resolution:** Correct the network adapter or address assignment issue identified during investigation.

**Verification:** Repeat `ip -br addr` and confirm that the interface has the expected address.

### Issue B: No Default Route

**Possible causes**
- Gateway configuration is missing.
- DHCP did not provide routing information.
- The guest's network configuration is incorrect.

**Diagnostic command**

```bash
ip route
```

**Investigation**
1. Inspect the routing table.
2. Check whether the expected default route exists.
3. Compare the route with the configured virtual network.

**Resolution:** Correct the specific gateway or routing configuration issue.

**Verification:** Run `ip route` again and test the configured gateway.

### Issue C: External Connectivity Fails

**Diagnostic commands**

```bash
ping -c 4 <gateway-ip>
ping -c 4 1.1.1.1
```

Replace `<gateway-ip>` with the actual gateway address.

**Investigation**
- If gateway connectivity fails, investigate the local virtual network, interface, and gateway configuration.
- If the gateway responds but the external IP test fails, investigate NAT Network settings, upstream connectivity, and firewall rules.
- If ping fails, consider whether ICMP is filtered before concluding that connectivity is unavailable.

**Verification:** Repeat the relevant tests after correcting the identified issue.

### Issue D: DNS Resolution Fails

**Diagnostic command**

```bash
getent hosts example.com
```

**Investigation**
1. Check whether external IP connectivity works.
2. Inspect the configured DNS resolver.
3. Review `/etc/resolv.conf` where applicable.
4. Consider the network manager and resolver service responsible for DNS configuration.

**Resolution:** Correct the identified resolver or DNS configuration issue.

**Verification:** Repeat the hostname lookup and record the output.

## 3. Troubleshooting Workflow

1. Inspect the network interface.
2. Inspect IP addressing.
3. Inspect the routing table.
4. Test gateway connectivity.
5. Test external IP connectivity.
6. Test DNS resolution.
7. Review VirtualBox network settings.
8. Apply one targeted correction at a time.
9. Repeat the relevant tests and record the results.



## 5. Key Takeaway

Troubleshooting is more reliable when each component is tested independently. Separating interface, routing, external connectivity, and DNS checks helps narrow down the source of a problem without making unnecessary configuration changes.

*This guide describes diagnostic procedures. Record a problem as an actual incident only when you have observed it.*
