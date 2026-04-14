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
![](/img/attack.png)
2. After stopping the attack:<br>
![](/img/stopped.png)

⚠️ This PoC is intended for educational and authorized testing only.

## Impact

While the Slowloris attack is running:

The web service becomes unresponsive
The mobile application cannot receive the video stream
Communication between the device and the client is disrupted
