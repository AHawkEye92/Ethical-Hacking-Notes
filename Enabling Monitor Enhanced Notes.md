## Enabling Monitor Mode

### Using `iwconfig` and `airmon-ng`

1. **Check Wireless Interface Mode:**
    
    - **Command:** `iwconfig`
    - **Purpose:** See the wireless interface and its mode (e.g., managed).
        - **Managed Mode:** Can only capture packets with the destination MAC address as the MAC address of the device.
        - **Monitor Mode:** Can capture all packets in range.
2. **Change from Managed to Monitor Mode:**
    
    - **Bring down the interface:**
        - **Command:** `ifconfig wlan0 down`
    - **Kill interfering processes:**
        - **Command:** `airmon-ng check kill`
            - **Purpose:** Kills all processes that could interfere with monitor mode. This will also disconnect WiFi, which is not needed for pre-connection attacks.
    - **Set interface to monitor mode:**
        - **Command:** `iwconfig wlan0 mode monitor`
    - **Bring up the interface:**
        - **Command:** `ifconfig wlan0 up`
    - **Verify mode:**
        - **Command:** `iwconfig`
            - **Purpose:** Confirm the interface mode is now monitor.
3. **Capture Packets:**
    
    - Now the interface is in monitor mode and can capture all packets in range.

### Alternate Method to Enable Monitor Mode

1. **Check Wireless Interface Mode:**
    
    - **Command:** `iwconfig`
2. **Bring Down the Interface:**
    
    - **Command:** `ifconfig wlan0 down`
3. **Kill Interfering Processes:**
    
    - **Command:** `airmon-ng check kill`
    - **Purpose:** Kill processes that could interfere with monitor mode, resulting in loss of internet connection.
4. **Start Monitor Mode:**
    
    - **Command:** `airmon-ng start wlan0`
    - **Message:** Should confirm "monitor mode enabled".
5. **Verify New Interface:**
    
    - **Command:** `iwconfig`
    - **Note:** The new interface will typically be named `wlan0mon`.
6. **Use New Interface:**
    
    - Use `wlan0mon` for capturing packets.

### Troubleshooting

- If neither method works, monitor mode might not be supported.
    
- **Possible Causes:**
    
    - Unsupported chipset.
- **Examples of Supported Chipsets:**
    
    - Realtek RTL8812AU
    - Atheros AR9271