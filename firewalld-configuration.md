Here's a comprehensive step-by-step guide on configuring `firewalld` to use a custom `restricted` zone for the air-gap lab cluster, allowing specific services, ports, and ICMP traffic from the `10.101.194.0/24` network, while blocking all other traffic.

### Step 1: Create and Configure the `restricted` Zone
1. Create a new zone called `restricted`:
   ```bash
   sudo firewall-cmd --permanent --new-zone=restricted
   ```

2. Assign the `ocp` interface to the `restricted` zone:
   ```bash
   sudo firewall-cmd --permanent --zone=restricted --add-interface=ocp
   ```

3. Set the default target of the `restricted` zone to `DROP` to block all traffic by default:
   ```bash
   sudo firewall-cmd --permanent --zone=restricted --set-target=DROP
   ```

### Step 2: Allow Specific Services
1. Allow essential services like DHCP, DNS, HTTP, HTTPS, SSH, and TFTP in the `restricted` zone:
   ```bash
   sudo firewall-cmd --permanent --zone=restricted --add-service={dhcp,dns,http,https,ssh,tftp}
   ```

### Step 3: Allow Specific TCP and UDP Ports
1. Allow necessary TCP ports in the `restricted` zone:
   ```bash
   sudo firewall-cmd --permanent --zone=restricted --add-port={8443,111,2049,892,662,3260,8000}/tcp
   ```

2. Allow necessary UDP ports in the `restricted` zone:
   ```bash
   sudo firewall-cmd --permanent --zone=restricted --add-port={111,2049,662}/udp
   ```

### Step 4: Allow ICMP Traffic from the `10.101.194.0/24` Network
1. Add a rich rule to allow ICMP traffic from the `10.101.194.0/24` network:
   ```bash
   sudo firewall-cmd --permanent --zone=restricted --add-rich-rule='rule family="ipv4" source address="10.101.194.0/24" protocol value="icmp" accept'
   ```

### Step 5: Reload the Firewall to Apply All Changes
Reload the firewall configuration to apply all permanent changes:

```bash
sudo firewall-cmd --reload
```

### Step 6: Verify the Configuration
1. Check the configuration of the `restricted` zone to ensure all rules are applied correctly:
   ```bash
   sudo firewall-cmd --zone=restricted --list-all
   ```

You should see something similar to this:

```
restricted (active)
  target: DROP
  icmp-block-inversion: no
  interfaces: ocp
  sources: 10.101.194.0/24
  services: dhcp dns http https ssh tftp
  ports: 8443/tcp 111/tcp 2049/tcp 892/tcp 662/tcp 3260/tcp 8000/tcp 111/udp 2049/udp 662/udp
  protocols:
  forward: no
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
    rule family="ipv4" source address="10.101.194.0/24" protocol value="icmp" accept
```

### Summary of the Configuration:
1. **Created** a new `restricted` zone and **assigned** the `ocp` interface to it.
2. **Blocked all traffic by default** with `target=DROP`.
3. **Allowed specific services** (DHCP, DNS, HTTP, HTTPS, SSH, TFTP).
4. **Allowed specific ports** (TCP: 8443, 111, 2049, 892, 662, 3260, 8000; UDP: 111, 2049, 662).
5. **Allowed ICMP traffic** from the `10.101.194.0/24` network.
6. **Reloaded** the firewall and **verified** the configuration.

