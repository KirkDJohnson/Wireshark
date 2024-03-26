<h1>Wireshark Malware Analysis</h1>

<br />
<h2>Description</h2>
This lab I analyzed a known malicious packet capture on the exploit Log4J and was able to able to identify the exploit in clear text, and check to see if the server responsed to the attacker, i.e opened a command and control (c2) channel confirming that the exploit was successful, which was not the case. My approach to this was to initally see who (what IP addresses and where they were from) was communicating with the target server.Then it was crucial to determine whether the server that was attacked (198.71.247.91) begun any nre connections

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
After noticing an unusual amount of traffic and different Ipv4 conversations orginating from different countries I exported endpoints to be shown on a global map (Endpoints -> Map -> Open in Browser) <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/b5a20e87-9c74-409a-a7bb-76fe36420ca2" alt="Wireshark Mal Analysis"/>
<br />
<br />
Knowing that I dealing with a Log4J exploit PCAP, the exploit leveerage the Java Naming and Directory Interface (jndi) vulnerbility, so I thought that would be a good filter to start with (ip contains "jndi") <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/1c9cea18-0c98-40ea-9700-97f542ce0d89"  alt="Wireshark Mal Analysis"/>
<br />
<br />
After seeing a response from the filter through http (clear text), I was able to further examine the http POST packet and found the User-Agent that first tried to deploy the exploit.  <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/daee5119-30b0-4487-b10d-f9b0efa41c12"  alt="Wireshark Mal Analysis"/>
<br />
<br />
Knowing that connections need to make the TCP handshake I filtered to see if the server syncronished (step 1) or created a new conenction signaling a potentioanl C2 server and successful exploit which was not the case. <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/aafd9ff4-0806-46bb-9200-fbac229d7b32"  alt="Wireshark Mal Analysis"/>
<br />
<br />
After confirming no compromise, I went back to the User-Agent field and decoded the base-64 script using the online tool CyberChef <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/9eb3698e-4f9a-4d1d-a694-c04845f6108d"  alt="Wireshark Mal Analysis"/>
<br />
<br />
With the decoded script I further investigated it using VirusTotal to learn more about the IP the script was calling and found it to be malicious<br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/8b49d158-a6c8-4748-a4f1-e41de3998b11"  alt="Wireshark Mal Analysis"/>
<br />
<br />
In the Community section of VirusTotal, there was further evidence that it was a Log4J attack as that IP was known to attempt the exploit in the past.<br/>
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
