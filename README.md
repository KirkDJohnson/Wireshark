<h1>Wireshark Malware Analysis</h1>

<br />
<h2>Description</h2>
This lab I analyzed a known malicious packet capture on the exploit Log4J and was able to able to identify the exploit in clear text, and check to see if the server responsed to the attacker, i.e opened a command and control (c2) channel confirming that the exploit was successful, which was not the case. My approach to this was to initally see who (what IP addresses and where they were from) was communicating with the target server.

<h2>Utilities Used</h2>

- <b>Wireshark</b> 
- <b>Virus Total</b>
- <b>CyberChef</b>
- <b>Max Mind's GEOIP Database</b>

<h2>Environments Used </h2>

- <b>Kali Linux through VMBox

<h2>Lab Overview:</h2>

<p align="center">
Opening up the PCAP, the first thing I like to do is see how many devices are present/communicating (Statistics -> Endpoints) <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/16d6cb86-1406-4f22-bdcc-589a2e346eda" alt="Wireshark Mal Analysis"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/b5a20e87-9c74-409a-a7bb-76fe36420ca2" alt="Wireshark Mal Analysis"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/1c9cea18-0c98-40ea-9700-97f542ce0d89"  alt="Wireshark Mal Analysis"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/daee5119-30b0-4487-b10d-f9b0efa41c12"  alt="Wireshark Mal Analysis"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/9eb3698e-4f9a-4d1d-a694-c04845f6108d"  alt="Wireshark Mal Analysis"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/aafd9ff4-0806-46bb-9200-fbac229d7b32"  alt="Wireshark Mal Analysis"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/8b49d158-a6c8-4748-a4f1-e41de3998b11"  alt="Wireshark Mal Analysis"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/d726f84b-251f-4bfc-82cb-7183a8fa5faf"  alt="Wireshark Mal Analysis"/>
</p>
<h2>Thoughts</h2>
This lab provided hands on experience analyzing a packet capture (.pcapng file) rather than live analysis on the wire. Obtaining a real packet capture of an attempted malicious attack and analyzing it was incredntly intresting, especially because it was in clear text through http port 80, rather than encrypted. While I have used VirusTotal and CyberChef in previous labs and walkthroughs, knowing that the artificats pulled from the pcap were real made me we even more curious to dive into resources that others in the cybersecurity community have put together surrounding the attacking IP address and the exploit itself. Moreover, I have used Wireshark previously, but this was the first time I configured Max Mind's GEOIP database to resolve locations from IP addresses and then be able to export as a map, was another highlight from this lab. 
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
