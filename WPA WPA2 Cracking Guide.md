## WPA/WPA2 Cracking Guide

### Overview

- **WPA/WPA2** can be cracked using the same method.
- These protocols were developed to resolve security issues found in WEP, offering more robust protection.
- **Encryption:** Each packet in WPA/WPA2 is encrypted using a temporary key, making the data in individual packets less useful for attackers.

### WPS (Wi-Fi Protected Setup)

- **Purpose:** Allows devices to connect to a router without needing a password.
- **Authentication:** Uses an 8-digit PIN for authentication.
- **Vulnerability:** Attackers can try all possible PIN combinations in a relatively short time.
- **Cracking Process:** Once the PIN is cracked, WPS can be used to obtain the actual password.

#### Limitations:

- The method works only if the router is not configured to use push-button authentication.
- **Example of Push-Button Authentication:** A user logs into their online banking account, enters their username and password, and receives a push notification on their smartphone. They approve the login attempt by pressing "Approve" on their phone, completing the authentication process.

### Method for Cracking WPA/WPA2 via WPS

1. **Open Two Terminals**
    
2. **Set Wireless Adapter to Monitor Mode:**
    
    bash
    
    Copy code
    
    `ifconfig wlan0 down airmon-ng check kill           iwconfig wlan0 mode monitor ifconfig wlan0 up`
    
3. **Find Networks Using WPS:**
    
    bash
    
    Copy code
    
    `wash --interface wlan0`
    
    - This command will show all networks near you that use WPS.
4. **Initiate Fake Authentication:**
    
    bash
    
    Copy code
    
    `aireplay-ng --fakeauth 30 -a [BSSID] -h [Your Device’s MAC Address] wlan0`
    
    - `30` represents 30 seconds; this time can be adjusted.
    - Note: The router might lock after several failed attempts.
5. **Start Reaver (Brute Force Attack):** In the second terminal, run:
    
    bash
    
    Copy code
    
    `reaver --bssid [BSSID] --channel 11 --interface wlan0 --vvv --no-associate`
    
    - **Reaver** is a program that uses brute force to crack the WPS PIN.
    - `-vvv` option provides detailed output, helpful in troubleshooting.
    - `--no-associate` tells Reaver not to handle the association as it’s done manually.
6. **Final Step:**
    
    - Once everything is set up, press Enter in the second terminal to start the attack.
    - **Note:** This method may not work on newer routers, but it’s worth trying.

### Capturing the WPA/WPA2 Handshake

- **Fixing WEP Weakness:** WPA/WPA2 encrypts packets with temporary keys, making individual packets less useful for attackers.
- **Handshake Importance:** The handshake involves 4 packets sent to a client connecting to the network. It contains information that can be used to verify the password, but not any useful data.

#### Steps to Capture a Handshake:

1. **Set Wireless Adapter to Monitor Mode**
    
2. **Open Two Terminals**
    
3. **Monitor Network:**
    
    bash
    
    Copy code
    
    `airodump-ng wlan0`
    
    - Use this command to see nearby networks.
4. **Capture the Handshake:** In the first terminal, run:
    
    bash
    
    Copy code
    
    `airodump-ng -bssid [BSSID] --channel [Channel Number] --write handshake wlan0`
    
    - `--write handshake` saves the handshake data to a file.
5. **Deauthenticate a Client:** In the second terminal, run:
    
    bash
    
    Copy code
    
    `aireplay-ng --deauth 4 -a [BSSID] -c [Client’s MAC Address] wlan0`
    
    - This forces the client to reconnect, making it possible to capture the handshake.
    - The handshake will be displayed in the first terminal once captured.

#### Note:

- **Handshake Data:** The handshake does not contain any useful data itself. It’s only used to verify whether a password is correct or not.

### Creating a Wordlist for Password Cracking

- **Wordlists** are essential for penetration testing as they contain potential passwords.
    
- **Syntax for Creating a Wordlist Using Crunch:**
    
    bash
    
    Copy code
    
    `crunch [min-length] [max-length] -t [pattern] -o [filename]`
    
    - Example:
        
        bash
        
        Copy code
        
        `crunch 6 8 123abc$ -o wordlist.txt -t a99j`
        
        - This creates a wordlist with passwords ranging from 6 to 8 characters using the characters `123abc$` and saves it as `wordlist.txt`.
- **View the Wordlist:**
    
    bash
    
    Copy code
    
    `cat wordlist.txt`
    
    - This command displays the passwords in the wordlist.

### Cracking the WPA/WPA2 Password

1. **Using Aircrack-ng to Unpack the Handshake:**
    
    - The **MIC (Message Integrity Code)** is used by the access point to verify if a password is correct.
    - If the MIC generated using a potential password matches the MIC in the handshake, the password is correct.
2. **Run Aircrack-ng:**
    
    bash
    
    Copy code
    
    `aircrack-ng wpa_handshake-01.cap -w test.txt`
    
    - This command tests each password in the wordlist `test.txt` against the captured handshake.
    - The process repeats until the correct password is found.