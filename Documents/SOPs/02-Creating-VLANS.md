# [SOP 02]: Creating VLANS

**Identifier:** `SOP-02`  
**Status:** Active
**Last Updated:** 2026-01-04  
**Maintainer:** Justin

---

## Overview
This SOP is created to show the steps when creating VLANS inside the Switch GUI for the homelab.

It will cover the following:
* Creating the 10, 20, and 30 VLAN IDs for our network per the IPAM
* Naming the 10, 20, and 30 VLANs per our IPAM
* Tagging ports with their respective VLANs

---

## Prerequisites & Warnings

* **Access Needed:** PC connected to switch on any of the 24 ports.
* **Dependencies:** Switch must be have power plugged into it.
* **Downtime Required:** No

---

## Step-by-Step Instructions

### 1. Preparation:
* We will be creating all VLANs to give them names and assign them to ports.
* You will log into the GUI of the swtich at http://192.168.2.10/
* You will navigate to "VLANs" in the left-hand navigation pane. You will then select "VLAN Configuration" in the drop-down menu.
* Once on the "VLAN Configuration" screen we will select "Create VLAN" checkbox, give it a number "10" is what we will use this is our management VLAN for our network.
* You will then select "Apply" and you will see under "VLAN ID" that "10" has been created. Repeat this process for "20" and "30" VLANs.

### 2. Naming VLANs:
* Once you have created all the VLAN IDs, in the same page select "Set Name" checkbox on VLAN ID 10, 20, and 30.
* You will see the text field under "VLAN NAME" become active.
* Type in VLAN ID 10: "MGMT" this is our management VLAN.
* Type in VLAN ID 20: "SERVERS" this is our VLAN that will house our servers.
* Type in VLAN ID 30: "CLIENTS" this is our VLAN that will house the client VMs and any test VMs.
* After you have typed the names in all the fields you will select "Apply" at the bottom and you will see the names in the "VLAN NAME" field.

### 3. Tagging ports with VLANs:
* You will then navigate to "VLANs" -> "Participation / Tagging".
* Inside this interface you will see a drop-down in the middle nad all of the ports on the switch below that.
* We will start with "10" in the drop-down which is our MGMT VLAN.
* We want all of our servers to have access to that VLAN so we will change the letter to "U" on ports 1-8.
* You will also need to change the port your management PC is on, mine is Port 13. So I will also set port 13 to "U" or "Untagged".
