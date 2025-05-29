# scapy
#Python #scapy #Network #Forensic #CTF

## Example
```python
from scapy.all import *
import re

# === Configuration ===
PCAP_FILE = "test.pcap"
OUTPUT_FILE = "reconstructed.png"

# === Load pcap ===
packets = rdpcap(PCAP_FILE)

chunks = {}
total_size = 0

sessions = packets.sessions()

for _session_key, session_packets in sessions.items():
    # Variable to hold the start position of the current chunk
    chunk_id = 0
    i = 0
    for pkt in session_packets:
        if pkt.haslayer(TCP) and pkt.haslayer(Raw):
            i += 1
            raw = pkt[Raw].load

            # Parse the Content-Range header
            match = re.search(
                br'(?P<header>Content-Range:\s+bytes\s+(\d+)-(\d+)/(\d+))\r\n\r\n(?P<body>.*)',
                raw,
                flags=re.MULTILINE | re.DOTALL
            )

            if match:
                body = match.group('body')
                start = int(match.group(2))
                end = int(match.group(3))
                total_size = max(total_size, int(match.group(4)))  # Update total expected size
                chunk_id = start

                expected_len = end - start + 1
                print("start: " + str(start) + "\tend: " + str(end) + "\tend - start: " + str(end-start+1) + "\tlen(body):" + str(len(body)))

                chunks[chunk_id] = body
                if len(body) >= expected_len:
                    break
                else:
                    print(f"[!] short of length: {start}-{end}, expect: {expected_len}, actual: {len(body)}")

            # If this is not the first packet in the session, append the payload
            if i > 1:
                chunks[chunk_id] += raw
                print(f"[-] appended chunk size: {len(chunks[chunk_id])}")

# === Reconstruct the image ===
print("-------------- Reconstruct the image -------------")
image_data = b''
for start in sorted(chunks):
    data = chunks[start]
    print(f"start: {start}\tsize: {len(data)}")
    image_data += data

with open(OUTPUT_FILE, "wb") as f:
    f.write(image_data)

print(f"Done: {OUTPUT_FILE} (size={len(image_data)} bytes, expected size from packet={total_size} bytes)")
```