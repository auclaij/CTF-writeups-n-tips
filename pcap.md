## PCAP Analysis

### strings
Possible quick win withtout loading into wireshark with: ```strings capture.pcap | grep -i flag```<br>

### binwalk
Possible quick win withtout loading into wireshark with: binwalk -e capture.pcap<br>
Lets you extract files (if there are some) from the PCAP

### wireshark
What to look for:<br>
- A file transfer
- A password passed in the clear (i.e., not encrypted)
- Traffic that doesn't fit in (such as a few IPv6 packets in a PCAP comprised mostly of IPv4)
- Anything that looks out of the ordinary is probably worth exploring.

What to do with wireshark:<br>
1. Look at the protocol hierarchy analysis: ```Statistics -> Protocol Hierarchy```, this will show you a distribution of the different protocols
2. Find irregulatiries such as a PCAP full of HTTPS traffic, but few packets are FTP data, you should probably start by looking at the FTP data
3. To do so, right click the category and click ```Apply as Filter -> Selected```

### quick wins
Files transferred via HTTP can be extracted from a PCAP in Wireshark with ```File -> Export Objects -> HTTP option```
Try searching for the flag in all TCP traffic with the following filter: ```frame contains flag```
