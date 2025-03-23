# NAT Configuration Examples

This repository provides examples of Dynamic NAT and Static NAT configurations for routers.

## Dynamic NAT Configuration

### Instructions

1. **Configure Router R1 (Boundary Router)**:
    ```plaintext
    interface FastEthernet0/0
    ip address 192.168.10.1 255.255.255.0
    no shutdown

    interface Serial2/0
    ip address 209.165.200.225 255.255.255.224
    ```

2. **Configure Router R0 (ISP)**:
    ```plaintext
    interface Serial3/0
    ip address 209.165.200.226 255.255.255.224
    ```

3. **Define Static Route on ISP Router**:
    ```plaintext
    ip route 192.168.10.0 255.255.255.0 Serial2/0
    ```

4. **Define Default Route on Router R1**:
    ```plaintext
    ip route 0.0.0.0 0.0.0.0 Serial2/0
    ```

5. **Configure NAT on Router R1**:
    ```plaintext
    ip nat pool NAT_POOL1 209.165.200.226 209.165.200.240 netmask 255.255.255.224
    ip access-list standard ACL1
    permit 192.168.10.0 0.0.0.255
    ip nat inside source list ACL1 pool NAT_POOL1 overload
    interface fa0/0
    ip nat inside
    no shutdown
    interface ser2/0
    ip nat outside
    no shutdown
    ```

## Static NAT Configuration

### Instructions

1. **Configure Router R1 (Boundary Router)**:
    ```plaintext
    interface FastEthernet0/0
    ip address 192.168.10.1 255.255.255.0
    no shutdown

    interface Serial2/0
    ip address 209.165.200.225 255.255.255.224
    clock rate 64000
    no shutdown
    ```

2. **Configure Router R0 (ISP)**:
    ```plaintext
    interface Serial3/0
    ip address 209.165.200.226 255.255.255.224
    ```

3. **Define Static Route on ISP Router**:
    ```plaintext
    ip route 192.168.10.0 255.255.255.0 Serial3/0
    ```

4. **Define Default Route on Router R1**:
    ```plaintext
    ip route 0.0.0.0 0.0.0.0 Serial2/0
    ```

5. **Configure NAT on Router R1**:
    ```plaintext
    ip nat inside source static 192.168.10.2 209.165.200.227
    ip nat inside source static 192.168.10.3 209.165.200.228
    interface fa0/0
    ip nat inside
    no shutdown
    interface ser2/0
    ip nat outside
    no shutdown
    ```

### Verification

To verify the NAT configuration, use the following commands:
```plaintext
show ip nat translations
show ip nat statistics
```

## Troubleshooting

For troubleshooting common issues with NAT configurations, refer to the [Troubleshooting Guide](docs/Troubleshooting.md).
