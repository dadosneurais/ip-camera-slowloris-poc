CVE-2026-88294 - Xiongmai Technology IP Camera Firmware (X3-WQ-B / iCSee) / Denial of Service (DoS) via a slowloris attack.

### Proof of Concept – Slowloris Vulnerability in IP Camera
![](/img/camera.png)<br>
This repository demonstrates a Proof of Concept (PoC) showing that a specific IP camera device is vulnerable to a Slowloris Denial of Service (DoS) attack.
The vulnerability allows an attacker to exhaust the server's available connections by sending partial HTTP requests, potentially making the web interface unavailable to legitimate users.

## Vulnerability Details
* **Type:** Denial of Service (DoS)
* **Technique:** Slowloris
* **CVE:** CVE-2007-6750
* **Affected Service:** HTTP (Port 80)

## Target Information
* **Device Type:** IP Wifi PTZ 360
* **Manufacturer:** iCSee
* **Model:** X3-WQ-B
* **Firmware:** V5.04.R02.000847FB.10010.346924.0000010, build 2025-08-28, latest version
* **IP Address:** 192.168.1.X (Lab environment)

## 🔍 Discovery Method

The vulnerability was identified through **manual testing** of the device's HTTP service.

During interaction with the web interface, it was observed that:
- scanning the ports with nmap:
```bash
nmap -sC 192.168.1.3
```
![](/img/nmap.png)
- The server keeps HTTP connections in the port 80 send a POST method to the TCPPort 34567, which makes communication with the App ICsee
![](/img/burp.png)<br>

## 🧪 Proof of Concept

To validate the vulnerability, the device was tested in a controlled lab environment.
The mobile application (iCSee) was mirrored using scrcpy to observe the impact in real time.
1. During attack:<br>
   1.1. Motion detection alerts and video streams freeze completely during the attack window.
![](/img/attack.png)
2. After stopping the attack:<br>
   2.1. Normal operations resume only after the attack script is terminated.
![](/img/stopped.png)
3. Impact Verification:
   3.1. Accessing the camera via the iCSee mobile application fails for both Record mode (SD Card playback) and Cloud storage mode.
   3.2. Local video recording and picture storage to both the SD card and cloud storage cease entirely.
<img src="/img/icsee.jpeg" width="50%">

⚠️ This PoC is intended for educational and authorized testing only.

## Impact
During testing, initiating a connection exhaustion attack (Slowloris technique) against port 34567 (NETIP media/control service) with 1,000 slow sockets completely incapacitated the device. While the attack was active, the camera stopped recording and saving images entirely—failing to save footage to both the local SD/memory card and cloud storage. Additionally, video streams froze, motion detection ceased functioning, and remote access via the iCSee application was rendered unavailable. Full functionality was restored only after the attack traffic stopped.
