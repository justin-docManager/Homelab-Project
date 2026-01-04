# [SOP 01]: Initializing the Switch

**Identifier:** `SOP-01`  
**Status:** Active
**Last Updated:** 2026-01-04  
**Maintainer:** Justin

---

## Overview
This SOP is created to show the steps when Initializing the switch for the first time to prep it for use in our Homelab.

It will cover the following:
* Factory defaulting the switch.
* Setting a static on the management PC to access the switch
* Setting the correct static per the IPAM
* Setting the name so it matches our naming convention per the IPAM
* Setting a password for security reasons

---

## Prerequisites & Warnings
List requirements or risks before starting.

* **Access Needed:** PC connected to switch on any of the 24 ports.
* **Dependencies:** Switch must be have power plugged into it.
* **Downtime Required:** Yes

---

## Step-by-Step Instructions

### 1. Preparation:
* Prepare the switch by plugging in the power cable to a power source.
* Since we do not know if it already has a running config in it. You will need to reset to factory default settings.
* To do this we will take any object that has a thin point and is about 1 inch long. You will then proceed to press the "Reset" button on the front of the switch.
* After you do this all lights will light up, that is how you know you just initiated the factory reset

### 2. Changing the IP on your PC:
* After the switch has been defaulted and all the lights have turned off again (other than any that are currently connected to machines).
* Go to your management PC and open your "Ethernet" settings.
* Navigate to "Internet Protocol Version 4 (TCP/IPv4)" select this and click the options just under that of "Properties".
* You will then set a static IP address by selecting "Use the following IP Address".
* The default IP of the switch will be 192.168.2.10, we will need to put our IP in the same subnet.
* You will type in the "IP address" field: 192.168.2.X "X" being any number from 2 to 254 that is not 10 (that is our switch).
* I am using 192.168.2.25 for this example.
* You will then fill in "Subnet Mask" with: 255.255.255.0
* You will then leave the "Default Gateway" blank.
* You can now click on "OK" at the bottom, you will then select "Close" at the bottom. At that time your ethernet adapater will then have an IP Address on the subnet of the switch

### 3. Logging into the Web GUI of the switch:
* Open any browswer and navigate to http://192.168.2.10/
* This will open the Web GUI of the switch, there is no default password for this brand of switch so you can just click on "Login"
* After you have logged in on the home page you will change "System Name" to JHL-SW-01 as per our IPAM
* You will then go to "Network Setup" on the left-hand navigation menu. Select "Get Connected" from the drop-down.
* You will change the "IP Address" to 192.168.10.2 per our IPAM
* The Subnet mask should stay the same, but if it does not it is 255.255.255.0
* You will change the "Gateway Address" to 192.168.10.1 (this will be our Sophos later)
* You will then click "Apply" at the bottom. You will lose connection with the switch as our IP is no longer in that subnet.
* You will repeat Step 2 with a new IP in the range of 192.168.10.3-254.
* After you have done that you can log back into the switch via http://192.168.10.2/

### 4. Setting a Password for login:
* Once you are logged back into the switch you will go to: "Maintenance" -> "Password Manager".
* Once there you will type in a password that you can remember and that is secure in both the "New Password" and "Confirm New Password" fields.
* You will leave the "Old Password" blank as there is no password on the switch.
* You will then click "Apply" and then you will relog into the switch with the password you created.
