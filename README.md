<h1>Wireshark Malware Analysis</h1>

<br />
<h2>Description</h2>
This lab I analyzed a known malicious packet capture on the exploit Log4J. I begun my investigation with the same playbook I use for packet capture analysis, which is to examine the statistics and see how many and how much endpoints are communicating. Noticing the sheer amount IP addresses I wanted to map the addresses on the globe so I installed and configured Max Mind's GEOIP database which resolves IP addresses to their country and city. This amount of IP addresses signifies a potential attack from a botnet for the Log4J expliot or other malicioius scanning. Due to the traffic in capture using http and not https (encrypted), I was able to identify the exploit in clear text filtering for "jndi" which is the Java Naming and Direcotry Interface. The first packet from this filter was a HTTP POST request and upon examing the User-Agent field, revealed an ldap request to an IP followed by encoded text. Decoding the text in CyberChef showed the the code to be a wget to an IP to download a shell script, use the chmod command to give it execute permissions and then run the script. Moreover, to discover if the host machine made outbound connection attempts to public IPs signifying a command and control server, I filtered for it as the source address and TCP SYN packets for TCP connections to begin and confirmed that there were no connections. This could be the result of the machine being patched or a implicit firewall rule blocking outbound connections. Once it was clear the attack was unsuccessful I conducted further threat intelligence on the IP in the wget command and found the IP be known malicious and under the community tab in VirusTotal, the IP was linked to Log4shell attacks further confirming the attack. My approach to this was to initally see who was communicating with the target server. Then it was crucial to determine whether the server that was attacked (198.71.247.91) begun any new outbound connections.

<h2>Utilities Used</h2>

- <b>Wireshark</b> 
- <b>Virus Total</b>
- <b>CyberChef</b>
- <b>Max Mind's GEOIP Database</b>

<h2>Environments Used </h2>

- <b>Kali Linux through Oracle Virtual Box

<h2>Lab Overview:</h2>

<p align="center">
Opening up the PCAP, the first thing I like to do is see how many devices are present/communicating (Statistics -> Endpoints) <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/16d6cb86-1406-4f22-bdcc-589a2e346eda" alt="Wireshark Mal Analysis"/>
<br />
<br />
After noticing an unusual amount of traffic and different IPv4 conversations orginating from different countries I exported endpoints to be shown on a global map using Max Mind's GEOIP database.(Endpoints -> Map -> Open in Browser) <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/b5a20e87-9c74-409a-a7bb-76fe36420ca2" alt="Wireshark Mal Analysis"/>
<br />
<br />
Knowing that I dealing with a Log4J exploit PCAP, the exploit leverages the Java Naming and Directory Interface (jndi) vulnerability, so I thought that would be a good filter to start with (ip contains "jndi") <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/1c9cea18-0c98-40ea-9700-97f542ce0d89"  alt="Wireshark Mal Analysis"/>
<br />
<br />
The filter for "jndi" work well, and I expanded the first packet which was a an HTTP POST request and examined the User-Agent field which showed an ldap request with an IP followed by base-64 encoded text.   <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/daee5119-30b0-4487-b10d-f9b0efa41c12"  alt="Wireshark Mal Analysis"/>
<br />
<br />
  Decoding the the end of the intial post request in the attack, shows a wget which a web request to an IP to download lh[.]sh a shell script and the command chmod which changes privileges to add x which is execute privliegs. Lastly, it would start the shell script <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/9eb3698e-4f9a-4d1d-a694-c04845f6108d"  alt="Wireshark Mal Analysis"/>
<br />
<br />
I then determined if the host server at 198.71.247.91 made any connections to outside server, specically to the wget IP address in decoded string. I filtered for SYN packets which establish a TCP connection and found the server to have not connected with outside addresses likely meaning that the server was patched and therefore the expliot failed. <br/>
<img src="https://github.com/KirkDJohnson/Wireshark/assets/164972007/aafd9ff4-0806-46bb-9200-fbac229d7b32"  alt="Wireshark Mal Analysis"/>
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
This lab provided hands on experience analyzing a packet capture (.pcapng file) rather than live analysis on the wire. Obtaining a real packet capture of an attempted malicious attack and analyzing it was incredibly interesting, especially because it was in clear text through http port 80, rather than encrypted. While I have used VirusTotal and CyberChef in previous labs and walkthroughs, knowing that the artifacts pulled from the pcap were real made me we even more curious to dive into resources that others in the cybersecurity community have put together surrounding the attacking IP address and the exploit itself. Moreover, I have used Wireshark previously, but this was the first time I configured Max Mind's GEOIP database to resolve locations from IP addresses and then be able to export as a map, which was another highlight from this lab. 
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
