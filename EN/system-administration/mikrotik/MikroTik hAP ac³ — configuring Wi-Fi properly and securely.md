# **📡 MikroTik hAP ac³ — configuring Wi-Fi properly and securely**


I bought a new router — **MikroTik hAP ac³** — and decided to configure it not in a “whatever works” way, but **properly and securely**, the way it’s done in conscious, well-designed networks.
  
The goal is simple and honest: to configure Wi-Fi **as correctly as possible from a security perspective**, without unnecessary magic, but with a clear understanding of _what_ we are doing and _why_.

In this guide, step by step, we will:

- set up basic Wi-Fi using **WPA3**
- carefully bring the MikroTik into a proper access point state
- prepare it for operation with **RADIUS** (802.1X / EAP)
- analyze common mistakes and typical pitfalls

This is not theory and not a rewritten manual.

This is **practice**, collected during the real configuration of a new device — exactly the way I would do it in my own network.

And this is just the beginning.

The next steps will make the network more mature.

---
## **🔌 Step 1. First connection and logging into MikroTik**

We start with the simplest part — **physical connection and initial access** to the router.

No magic, everything is as straightforward as possible.

### **1️⃣ Connecting the cables**

- Take the cable from your ISP and **plug it into port** **ether1** — this will be the WAN
- Connect the configuration computer to **any other port** (for example, ether2)
- Power on the router and wait for it to fully boot

At this stage, **Wi-Fi can be ignored completely** — we configure everything over cable, it’s more reliable.

---
### **2️⃣ Preparing the computer for connection**

Since the router is new (or reset to factory settings), we connect to it directly.

On the computer:

- open the network adapter settings
- manually set a **static IP address**


Use the following parameters:

- **IP address:** 192.168.88.2
- **Netmask:** 255.255.255.0 (or /24)
- Gateway and DNS can be left empty

This is temporary — only for initial setup.

---
### **3️⃣ Connecting via Winbox**

To manage MikroTik, we will use **Winbox** — this is the most convenient management method.

- Download Winbox [**from the official MikroTik website**](https://mikrotik.com/download/winbox)**![Attachment.tiff](file:///Attachment.tiff)**
- Launch the program on your computer
- Select the router from the device list (via MAC address) or enter the address manually
- Connect

Default login credentials:

- **Login:** admin
- **Password:** empty
  
After logging in, you will see a clean factory configuration — from this point, we start putting things in order 😌

---

At this point, we **fully control the device** and can move forward:

cleaning up the base configuration and preparing the groundwork for secure Wi-Fi.

---
## **🌍 Step 2. WAN configuration via Winbox (MikroTik)**

We are already connected to the router via **Winbox** and can see its interface.

Now we configure the **WAN connection**, meaning the internet connection from the ISP.

> In our case, WAN is port **ether1**, where the provider’s cable is connected.

---

### **2.1 Checking the physical interface status**

1. In Winbox, go to the menu
    **Interfaces**
2. Find **ether1** in the interface list
3. Check the following:
    - the interface is **enabled** (no red cross)
    - the **R (running)** status is present — this means the link is up


📌 _If_ _ether1_ _is not running, first check the cable and the port._

---
### **2.2 Enabling DHCP Client on WAN**

Since in most cases the ISP provides internet via DHCP, we configure a DHCP client.

1. Go to
    **IP** **→** **DHCP Client**
2. Click **+** **(Add New)**
3. In the opened window, set:
    - **Interface:** ether1
    - **Use Peer DNS:** ✅ enabled
    - **Use Peer NTP:** ✅ enabled
    - **Add Default Route:** ✅ enabled

Leave all other parameters **at their default values**.

4. Click **OK**

📌 _After this, MikroTik should automatically obtain an IP address from the ISP._

---
### **2.3 Verifying internet connectivity**

Now it’s important to make sure the router actually has internet access.

1. In **IP → DHCP Client**:
    - the status should be **bound**
    - a received IP address should be displayed

2. Go to
    **IP** **→** **Routes**
    - verify that route **0.0.0.0/0** exists
    - the gateway points to the ISP address

3. Final check:
    - open **New Terminal**
    - try to ping any external IP address

If the ping succeeds, then:

> WAN is configured correctly, and the router already has internet access.

---
### **Important note**

At this stage:

- ❌ we do not configure Wi-Fi
- ❌ we do not touch the firewall
- ❌ we do not complicate the LAN yet

Our only task here is to **make sure WAN works reliably**.
This is the foundation for all further steps.

---
## **🏠 Step 3. Basic LAN configuration (internal network)**

  

Now that WAN is working, we configure **LAN** — the internal network to which devices will connect.

  

For now, without VLANs, without RADIUS, and without complications. Our goal is a **clean and understandable base**.

---
### **3.1 Checking and configuring the Bridge**

In MikroTik, all LAN ports are usually combined into a **bridge** — this is standard switch logic.

1. In Winbox, go to
    **Bridge**
2. In the **Bridge** tab:
    - usually there is already a bridge named bridge
    - if it does not exist — click **+** and create a new one

Parameters:

- **Name:** bridge
- Everything else — default

Click **OK**.

---
### **3.2 Adding LAN ports to the bridge**

Now we connect physical ports to the bridge.

1. Go to
    **Bridge** **→** **Ports**
2. Add ports:
    - click **+**
    - **Interface:** ether2
    - **Bridge:** bridge
    - **OK**

3. Repeat for the remaining LAN ports:
    - ether3
    - ether4
    - ether5

📌 _Port_ _ether1_ _(WAN)_ **must not** _be added to the bridge._

---
### **3.3 Assigning an IP address to the bridge**

Now we assign an IP address for the internal network.


![[Pasted image 20260128124805.png]]

1. Go to
    
    **IP** **→** **Addresses**
    
2. Click **+**
    
3. Set:
    
    - **Address:** 192.168.88.1/24
        
    - **Interface:** bridge
        
    
4. Click **OK**
    

  

This will be:

- the router’s LAN IP address
    
- the default gateway for clients
    

---

### **3.4 Configuring DHCP server for LAN**

  

To allow devices to obtain addresses automatically, we set up DHCP.

1. Go to
    
    **IP** **→** **DHCP Server**
    
2. Click **DHCP Setup**
    
3. In the wizard:
    
    - **DHCP Server Interface:** bridge
        
    - **DHCP Address Space:** 192.168.88.0/24
        
    - **Gateway:** 192.168.88.1
        
    - **Address Pool:** leave default
        
    - **DNS Servers:** can be left automatic
        
    
4. Confirm all wizard steps
    

  

After completion, the DHCP server will be active.

---

### **3.5 Verifying LAN operation**

  

Connect a computer to any LAN port:

- verify that it receives an IP address automatically
    
- try opening:
    
    - the MikroTik web interface
        
    - any website on the internet
        
    

  

If everything works, then:

  

> the internal network is configured correctly and ready for further steps.

---

### **What matters at this stage**

- ✔️ simple and transparent LAN layout
    
- ✔️ everything is based on the bridge
    
- ✔️ no unnecessary complexity
    

  

This is the base from which we will continue to:

- build Wi-Fi
    
- strengthen security
    
- move to WPA3 and RADIUS
    

---

## **📦 Step 4.0. Installing the Wi-Fi package**

  

Before configuring modern and secure Wi-Fi, it’s important to understand one thing:

  

> the standard **wireless** package in RouterOS is **limited**

> and **does not allow proper WPA3 usage**

  

That’s why for further configuration we will use the **new MikroTik Wi-Fi stack — wifi-qcom-ac**.

---

### **What it is and why it matters**

  

wifiwave2 is a new MikroTik wireless package that:

- supports **WPA3**
    
- works correctly with **802.11ac / ax**
    
- is the current and recommended option
    

  

If it’s not installed, there is simply **no point in going further**.

---

### **4.0.1 Checking installed packages**

  

![[Pasted image 20260128125529.png]]

1. In Winbox, go to
    
    **System** **→** **Packages**
    
2. In the package list:
    
    - look for **wifi-qcom-ac**
        
    - if it’s **not present** — it’s not installed
        
    - if wireless is present — that’s fine, we will logically replace it
        
    

  

📌 _Do not remove anything at this stage._

---

### **4.0.2 Installing the package**

  

![[Pasted image 20260128125827.png]]

  

Instead of **Enable**, you will see **Install**.

---

Now in the Winbox menu:

- instead of the old **Wireless**
    
- a new **WiFi** section appears
    

  

👉 This means the router is ready for modern Wi-Fi configuration.

---

### **Important note**

  

From this point:

- ❌ **do not use the old** **wireless** **package**
    
- ✅ **all further Wi-Fi configuration will be done via** **WiFi**
    

  

With this package we will:

- configure **WPA3**
    
- prepare the groundwork for **RADIUS / 802.1X**
    

---

## **📶 Step 4. Wi-Fi configuration via**

## **WiFi**

## **(PSK / WPA3)**

  

After installing the **wifiwave2** package, all further wireless configuration is done **via the new WiFi section**, not the old Wireless.

  

Our goal at this step is:

  

👉 to bring up a **working and secure PSK-based Wi-Fi**, which will serve as the foundation for the later transition to RADIUS.

---

### **4.1 Checking Wi-Fi interfaces**

1. In Winbox, go to
    
    **WiFi**
    
2. In the **WiFi Interfaces** tab, two interfaces should be visible:
    
    - 2.4 GHz
        
    - 5 GHz
        
    

  

If the interfaces are **disabled**, enable them (**Enable**).

---

### **4.2 Creating a security profile (Security)**

  

Now we configure Wi-Fi security.

1. Go to
    
    **WiFi** **→** **Security**
    
2. Click **+** **(Add New)**
    
3. Set the parameters:
    

  

- **Name:** wpa3-psk
    
- **Authentication Types:**
    
    - WPA2-PSK ✅
        
    - WPA3-PSK ✅
        
    
- **Encryption:**
    
    - CCMP (AES)
        
    
- **Passphrase:**
    
    - set a strong password (not short)
        
    

  

📌 _Keeping WPA2 + WPA3 improves compatibility with older devices._

  

Click **OK**.

---

### **4.3 Creating a Wi-Fi configuration (Configuration)**

  

Now we bind the SSID, security, and operation mode.

1. Go to
    
    **WiFi** **→** **Configurations**
    
2. Click **+**
    
3. Main parameters:
    

  

- **Name:** wifi-main
    
- **SSID:** your Wi-Fi network name (for example — SUDO_MAKE_ME_A_SANDWICH 😂)
    
- **Country:** your country
    
- **Mode:** ap
    
- **Security:** wpa3-psk
    
- **Disable PMKID:** ❌ (leave disabled)
    

  

Click **OK**.

---

### **4.4 Binding Wi-Fi to the bridge (Datapath)**

1. In the same configuration, open the
    
    **Datapath** tab
    
2. Set:
    
    - **Bridge:** bridge
        
    - **Client Isolation:** ❌ (disabled for now)
        
    

  

📌 _This allows Wi-Fi clients to access the LAN._

---

### **4.5 Applying the configuration to interfaces**

1. Go back to
    
    **WiFi** **→** **WiFi Interfaces**
    
2. For each interface (2.4 and 5 GHz):
    
    - open the interface
        
    - select **wifi-main** in the **Configuration** field
        
    - click **OK**
        
    

  

After this:

- the SSID becomes visible
    
- Wi-Fi starts working with the configured security
    

---

### **4.6 Verifying the connection**

  

Connect from any device:

- the network is visible
    
- the password is accepted
    
- the device receives an IP via DHCP
    
- internet access is available
    

  

If all of this works, then:

  

> PSK-based Wi-Fi is configured correctly and is stable.

---

### **The key idea of this step**

  

This setup is a **deliberate base**, not the final state:

- ✔️ modern Wi-Fi stack
    
- ✔️ WPA3
    
- ✔️ clean LAN integration
    

  

Next, we will:

  

👉 strengthen security

👉 disable unnecessary features

👉 **move to RADIUS / 802.1X**

---

## **🔐 Step 6. Hardening MikroTik and Wi-Fi security**

  

At this stage, we assume:

- WAN is working
    
- LAN is stable
    
- **WPA3-PSK Wi-Fi** is tested and in use
    

  

Now we bring everything into a **more secure state**.

---

### **6.1 Protecting access to the MikroTik itself**

  

#### **Changing the administrator password**

1. **System** **→** **Users**
    
2. Open the admin user
    
3. Set a **strong password**
    
4. OK
    

  

📌 _An empty password is only acceptable for the first login. Never after._

---

#### **Restricting management access**

1. **IP** **→** **Services**
    
2. Pay attention:
    
    - telnet ❌ disable
        
    - ftp ❌ disable
        
    - www ❌ disable
        
    - www-ssl — optional
        
    - ssh — only if needed
        
    - winbox — keep enabled ✅
        
    

  

👉 Ideally:

- management **only from LAN**
    
- no services exposed externally
    

---

### **6.2 Basic Wi-Fi security**

  

Go to **WiFi → Configurations**

Open our configuration (wifi-main).

  

#### **Check and enable:**

- **WPS:** ❌ disabled
    
- **Management Protection:** ✅ enabled (if available)
    
- **FT / Fast Transition:** ❌ (not needed yet)
    
- **PMKID:** ❌ (unless there’s a reason to enable it)
    

  

📌 _The fewer “smart” features, the smaller the attack surface._

---

### **6.3 Restricting Wi-Fi cryptography**

  

In **WiFi → Security**:

- use **only CCMP (AES)**
    
- no TKIP
    
- no legacy algorithms
    

  

This is especially important for WPA3 —

  

> security must be **real**, not “on paper”.

  

##### **📝 Footnote: Wi-Fi encryption options (Ciphers)**

![[Pasted image 20260128132013.png]]

  

Wi-Fi security settings offer different encryption algorithms. Briefly — **what they are and what to choose**.

  

#### **❌ TKIP**

- Obsolete algorithm (from early WPA days)
    
- Considered **insecure**
    
- Significantly weakens protection
    

  

#### **✅ CCMP (AES)**

- Modern and **stable standard**
    
- Fully supported by all devices
    
- Recommended for:
    
    - WPA2-PSK
        
    - WPA3-PSK
        
    - WPA-Enterprise
        
    

  

👉 **Optimal default choice**

  

#### **⚠️ GCMP**

- Newer algorithm
    
- Used in modern WPA3 implementations
    
- May cause:
    
    - compatibility issues
        
    - instability with older clients
        
    

  

👉 **Can be used**, but only if all clients are modern

  

#### **⚠️ CCMP-256**

- Strengthened version of CCMP
    
- Higher cryptographic strength
    
- Requires client support
    

  

👉 **Overkill for home and SOHO networks**

  

#### **⚠️ GCMP-256**

- Maximum encryption level
    
- Used in strict corporate environments
    
- Often causes compatibility issues
    

  

👉 **Only for specialized scenarios**

  

### **✅ Recommendation for this guide**

  

For a stable, secure, and compatible network:

  

> **Use: CCMP (AES)**

  

> No TKIP, no legacy algorithms.

  

This provides:

- high security
    
- excellent compatibility
    
- predictable network behavior
    

---

### **6.4 Clients and isolation (optional)**

  

If the network will include:

- guests
    
- IoT devices
    
- untrusted clients
    

  

👉 you can enable **Client Isolation** in the Datapath.

  

For now:

- primary network ❌ disabled
    
- guest networks — we’ll enable it later via a separate SSID
    

---

### **6.5 Overall logic of this step**

  

What we did:

- ✔️ closed unnecessary services
    
- ✔️ protected management access
    
- ✔️ removed outdated protocols
    
- ✔️ minimized the attack surface
    

  

This is **basic security hardening** — without fanaticism, but already at a level of:

  

> “not embarrassing to leave in a real network”

  

The most interesting part comes next 😌

  

👉 **Step 7: transition to RADIUS / 802.1X**

👉 per-user authentication

👉 mature network architecture