## Network Hacking - How to Change MAC Address

### Listing Network Interfaces

1. **ifconfig**
    
    - **Purpose:** List internet interfaces.
    - **Example Output:**
        
        
        
        Copy code
        
        `wlan0     Link encap:Ethernet  HWaddr 00:11:22:33:44:55             inet addr:192.168.1.2  Bcast:192.168.1.255  Mask:255.255.255.0           UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1           RX packets:1024 errors:0 dropped:0 overruns:0 frame:0           TX packets:512 errors:0 dropped:0 overruns:0 carrier:0           collisions:0 txqueuelen:1000            RX bytes:1048576 (1.0 MB)  TX bytes:524288 (512.0 KB)`
        
2. **Identifying Network Device:**
    
    - `wlan0` might be your USB network device.

### Changing MAC Address

1. **Bring down the interface:**
    
    - **Command:** `ifconfig wlan0 down`
2. **Change the MAC address:**
    
    - **Command:** `ifconfig wlan0 hw ether [new MAC address]`
        - **Example:** `ifconfig wlan0 hw ether 00:AA:BB:CC:DD:EE`
3. **Bring up the interface:**
    
    - **Command:** `ifconfig wlan0 up`
4. **Verification:**
    
    - The MAC address in memory should now be changed.

### Troubleshooting MAC Address Change in Kali Linux If MAC  Address Not Changing

1. **Edit NetworkManager Configuration:**
    
    - **Open Terminator terminal.**
    - **Command:** `leafpad /etc/NetworkManager/NetworkManager.conf`
2. **Modify the Configuration File:**
    
    - Add the following lines after leaving a blank line:
        
        csharp
        
        Copy code
        
        `[device] wifi.scan-rand-mac-address=no  [connection] ethernet.cloned-mac-address=preserve wifi.cloned-mac-address=preserve`
        
3. **Save and Quit:**
    
    - Save the changes and close the text editor.
4. **Restart NetworkManager:**
    
    - **Command:** `service NetworkManager restart`
    - If the above command does not work, try:
        - **Command:** `service network-manager restart`