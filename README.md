# Computer Networks Practicals — Code & Concept Explanations

---

## Practical 4 — Hamming Code (Error Detection & Correction)

### Concept

**Hamming Code** is an error-detecting and error-correcting code. It adds **parity bits** at specific positions (powers of 2: 1, 2, 4, …) to a block of data bits so that, if a single bit flips during transmission, the receiver can calculate which position is wrong and flip it back.

**Key idea:**
- Data bits are placed at non-power-of-2 positions (3, 5, 6, 7).
- Parity bits are placed at positions 1, 2, 4.
- Each parity bit covers a specific subset of positions (those whose binary representation has that bit set).
- The receiver recomputes the parity checks. If all are 0 → no error. If not → their combination gives the error position (syndrome).

**Positions covered:**
| Parity Bit | Covers positions |
|---|---|
| P1 (pos 1) | 1, 3, 5, 7 |
| P2 (pos 2) | 2, 3, 6, 7 |
| P4 (pos 4) | 4, 5, 6, 7 |

---

### Code

```cpp
#include <iostream>
using namespace std;

int main() {
    int data[4], ham[8] = {}, received[8];

    cout << "Enter 4 data bits: ";
    cin >> data[0] >> data[1] >> data[2] >> data[3];

    // Place data bits at non-power-of-2 positions (1-indexed)
    ham[3] = data[0];
    ham[5] = data[1];
    ham[6] = data[2];
    ham[7] = data[3];

    // Compute parity bits using XOR
    ham[1] = ham[3] ^ ham[5] ^ ham[7];   // P1 covers pos 1,3,5,7
    ham[2] = ham[3] ^ ham[6] ^ ham[7];   // P2 covers pos 2,3,6,7
    ham[4] = ham[5] ^ ham[6] ^ ham[7];   // P4 covers pos 4,5,6,7

    cout << "Hamming Code: ";
    for (int i = 1; i <= 7; i++) cout << ham[i];
    cout << "\n";

    cout << "Enter received 7 bits: ";
    for (int i = 1; i <= 7; i++) cin >> received[i];

    // Syndrome: recompute checks on received bits
    int c1 = received[1] ^ received[3] ^ received[5] ^ received[7];
    int c2 = received[2] ^ received[3] ^ received[6] ^ received[7];
    int c4 = received[4] ^ received[5] ^ received[6] ^ received[7];

    // Error position = binary number formed by c4 c2 c1
    int errorPos = c4 * 4 + c2 * 2 + c1;

    if (errorPos == 0) {
        cout << "No error detected!\n";
    } else {
        cout << "Error at position: " << errorPos << "\n";
        received[errorPos] ^= 1;   // Flip the erroneous bit
        cout << "Corrected code: ";
        for (int i = 1; i <= 7; i++) cout << received[i];
        cout << "\n";
    }
}
```

### Code-Level Explanation

| Line / Block | What it does |
|---|---|
| `ham[8] = {}` | 1-indexed array; index 0 unused. Initialized to 0. |
| `ham[3]=data[0]` etc. | Places 4 data bits at positions 3, 5, 6, 7 (not powers of 2). |
| `ham[1] = ham[3]^ham[5]^ham[7]` | XOR of the covered bits gives even parity for P1. |
| Syndrome `c1, c2, c4` | Re-XOR the same groups on the received word. If a bit flipped, exactly the checks covering that position will be 1. |
| `errorPos = c4*4 + c2*2 + c1` | Reconstructs the binary position from the three check bits. |
| `received[errorPos] ^= 1` | XOR with 1 flips the bit — corrects the error. |

---

## Practical 5 — Sliding Window Protocols (Go-Back-N & Selective Repeat)

### Concept

Sliding window protocols are flow-control mechanisms that allow a sender to transmit multiple frames before needing an acknowledgement (ACK), improving throughput over high-latency links.

**Go-Back-N (GBN):**
- Sender sends up to `WIN` (window size) frames continuously.
- If ANY frame in the window is lost, ALL frames from that lost frame onwards are retransmitted.
- Simple but wasteful for high error rates.

**Selective Repeat (SR):**
- Sender retransmits ONLY the lost/unacknowledged frame.
- More efficient but requires more memory (receiver must buffer out-of-order frames).

---

### Code

```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;

const int TOTAL = 8, WIN = 4;

// Simulates sending a frame; returns true (ACK) with 70% probability
bool transmit(int seq) {
    bool ok = rand() % 10 < 7;
    cout << "Frame " << seq << (ok ? " sent & ACKed\n" : " LOST\n");
    return ok;
}

void goBackN() {
    int base = 0;
    while (base < TOTAL) {
        bool allAck = true;
        // Send a full window starting from 'base'
        for (int i = base; i < base + WIN && i < TOTAL; i++) {
            if (!transmit(i)) { allAck = false; break; }
        }
        if (allAck) base += WIN;   // Slide the window forward
        else cout << "Retransmitting from frame " << base << "\n";
    }
}

void selectiveRepeat() {
    bool ack[TOTAL] = {};
    // First pass: try each frame once
    for (int i = 0; i < TOTAL; i++)
        if (!ack[i]) ack[i] = transmit(i);
    // Retry only unacknowledged frames
    for (int i = 0; i < TOTAL; i++)
        while (!ack[i]) ack[i] = transmit(i);
}

int main() {
    srand(time(0));
    int ch;
    cout << "1=Go-Back-N  2=Selective Repeat: ";
    cin >> ch;
    if (ch == 1) goBackN();
    else selectiveRepeat();
}
```

### Code-Level Explanation

| Block | What it does |
|---|---|
| `rand() % 10 < 7` | 70% chance of success — simulates lossy channel. |
| `goBackN()` — inner for loop | Sends frames `[base, base+WIN)`. Stops immediately on first loss. |
| `allAck` flag | If any frame in window failed, `allAck` stays false → retransmit entire window from `base`. |
| `base += WIN` | Window slides forward only when all frames in it are ACKed. |
| `selectiveRepeat()` — first loop | Single pass; records which frames were ACKed. |
| `while (!ack[i])` | Keeps retransmitting only that specific frame until it succeeds. |

---

## Practical 6 — IP Subnetting (CIDR)

### Concept

**Subnetting** divides a large IP address block into smaller sub-networks. **CIDR (Classless Inter-Domain Routing)** notation like `192.168.1.0/24` specifies how many leading bits form the **network portion**.

Key terms:
- **Subnet Mask:** Has 1s for network bits, 0s for host bits. `/24` → `255.255.255.0`
- **Network Address:** IP ANDed with mask — identifies the subnet.
- **Broadcast Address:** Network address ORed with inverted mask — last address in subnet.
- **Host Range:** All addresses between network+1 and broadcast-1.
- **Usable Hosts:** `2^(32-cidr) - 2` (subtract network and broadcast).

---

### Code

```cpp
#include <iostream>
using namespace std;

// Converts a 32-bit integer to dotted-decimal IP string
string toIP(unsigned int n) {
    return to_string((n>>24)&255) + "." +
           to_string((n>>16)&255) + "." +
           to_string((n>> 8)&255) + "." +
           to_string( n     &255);
}

int main() {
    string ip;
    int cidr, a, b, c, d;
    cout << "Enter IP and CIDR (e.g. 192.168.1.1 24): ";
    cin >> ip >> cidr;

    // Parse dotted-decimal into 4 octets
    sscanf(ip.c_str(), "%d.%d.%d.%d", &a, &b, &c, &d);

    // Pack 4 octets into a single 32-bit integer
    unsigned int ipInt    = (a<<24)|(b<<16)|(c<<8)|d;

    // Subnet mask: 32 leading 1-bits, shifted right by host bits
    unsigned int mask     = cidr ? (~0u << (32-cidr)) : 0;

    unsigned int network  = ipInt & mask;           // AND gives network address
    unsigned int broadcast = network | ~mask;       // OR with inverted mask gives broadcast

    cout << "Subnet Mask   : " << toIP(mask)        << "\n";
    cout << "Network Addr  : " << toIP(network)      << "\n";
    cout << "Broadcast Addr: " << toIP(broadcast)    << "\n";
    cout << "Host Range    : " << toIP(network+1) << " - " << toIP(broadcast-1) << "\n";
    cout << "Usable Hosts  : " << (1 << (32-cidr)) - 2 << "\n";
}
```

### Code-Level Explanation

| Expression | Meaning |
|---|---|
| `(n>>24)&255` | Right-shift 24 bits, mask lowest byte → extracts 1st octet. |
| `(a<<24)\|(b<<16)\|(c<<8)\|d` | Packs 4 octets into one 32-bit int for bitwise operations. |
| `~0u << (32-cidr)` | All-ones integer left-shifted → network bits are 1, host bits are 0. |
| `ipInt & mask` | Zeroes out host bits → network address. |
| `network \| ~mask` | Sets all host bits to 1 → broadcast address. |
| `(1 << (32-cidr)) - 2` | Total addresses in subnet minus 2 (network + broadcast). |

---

## Practical 7 — Dijkstra's Shortest Path Algorithm

### Concept

**Dijkstra's Algorithm** finds the shortest path from a single source node to all other nodes in a weighted graph with non-negative edge weights.

**How it works:**
1. Set distance to source = 0, all others = ∞.
2. Pick the unvisited node with the smallest known distance (`u`).
3. For each unvisited neighbour `v` of `u`: if `dist[u] + weight(u,v) < dist[v]`, update `dist[v]` (**relaxation**).
4. Mark `u` as visited (its distance is now final).
5. Repeat until all nodes are visited.

Used in: routing protocols (OSPF), GPS navigation, network topology.

---

### Code

```cpp
#include <iostream>
using namespace std;

const int INF = 1e9, MAXN = 100;
int g[MAXN][MAXN], dist[MAXN];
bool vis[MAXN];

int main() {
    int n, e;
    cin >> n >> e;

    // Initialize adjacency matrix
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            g[i][j] = (i == j) ? 0 : INF;

    // Read edges (undirected)
    for (int i = 0; i < e; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        g[u][v] = g[v][u] = w;
    }

    int src;
    cin >> src;

    // Initialize distances
    for (int i = 0; i < n; i++) dist[i] = INF, vis[i] = false;
    dist[src] = 0;

    // Main Dijkstra loop — runs n-1 times
    for (int i = 0; i < n - 1; i++) {
        // Find unvisited node with minimum distance
        int u = -1;
        for (int j = 0; j < n; j++)
            if (!vis[j] && (u == -1 || dist[j] < dist[u])) u = j;

        vis[u] = true;   // Lock in u's shortest distance

        // Relax neighbours of u
        for (int v = 0; v < n; v++)
            if (g[u][v] != INF && !vis[v] && dist[u] + g[u][v] < dist[v])
                dist[v] = dist[u] + g[u][v];
    }

    cout << "Shortest distances from node " << src << ":\n";
    for (int i = 0; i < n; i++)
        cout << "To " << i << ": "
             << (dist[i] == INF ? "unreachable" : to_string(dist[i])) << "\n";
}
```

### Code-Level Explanation

| Block | What it does |
|---|---|
| `g[i][j] = INF` | Represents no edge between i and j (large sentinel value). |
| `g[i][i] = 0` | Distance from a node to itself is 0. |
| Min-distance scan | Linear scan over all unvisited nodes to find the one with smallest `dist` — O(n²) overall. |
| `vis[u] = true` | Once processed, a node's distance is finalized (greedy property). |
| Relaxation condition | If going through `u` gives a shorter path to `v`, update `dist[v]`. |
| `dist[i] == INF` | Node `i` was never reached → unreachable from source. |

---

## Practical 8 — UDP File Transfer (Client-Server)

### Concept

**UDP (User Datagram Protocol)** is a connectionless transport layer protocol. Unlike TCP, it does not guarantee delivery, ordering, or duplicate protection — but it is fast and low-overhead, making it suitable for file transfer demos and real-time applications.

This practical implements a simple file transfer:
- **Client (sender):** Opens file, sends filename first, then file contents in chunks, then an `END` sentinel.
- **Server (receiver):** Listens for the filename, collects chunks until `END`, saves the file.

---

### Code

**Client (Sender)**
```python
import socket, sys

CHUNK = 1024
HOST, PORT = "127.0.0.1", 5001

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # UDP socket
file = sys.argv[1]   # Filename from command-line argument

# Send filename so receiver knows what to save as
sock.sendto(file.encode(), (HOST, PORT))

# Send file content in 1024-byte chunks
with open(file, "rb") as f:
    while chunk := f.read(CHUNK):     # Walrus operator: read and check non-empty
        sock.sendto(chunk, (HOST, PORT))

sock.sendto(b"END", (HOST, PORT))    # Sentinel to signal end of file
print(f"Sent: {file}")
sock.close()
```

**Server (Receiver)**
```python
import socket, os

PORT = 5001
SAVE = "./received"
os.makedirs(SAVE, exist_ok=True)   # Create output directory if needed

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.bind(("0.0.0.0", PORT))       # Listen on all interfaces
print(f"Listening on port {PORT}...")

# First packet contains the filename
data, addr = sock.recvfrom(4096)
filename = data.decode()
print(f"Receiving: {filename}")

# Collect chunks and write to file
with open(f"{SAVE}/{filename}", "wb") as f:
    while True:
        data, _ = sock.recvfrom(4096)
        if data == b"END":
            break
        f.write(data)

print(f"Saved to {SAVE}/{filename}")
sock.close()
```

### Code-Level Explanation

| Element | What it does |
|---|---|
| `SOCK_DGRAM` | Creates a UDP socket (vs `SOCK_STREAM` for TCP). |
| `sock.bind(("0.0.0.0", PORT))` | Listens on all network interfaces on the given port. |
| `file.encode()` | Converts filename string to bytes for UDP transmission. |
| `while chunk := f.read(CHUNK)` | Python walrus operator — reads and assigns; loop ends when empty (EOF). |
| `b"END"` sentinel | Simple application-level signal since UDP has no built-in EOF concept. |
| `sock.recvfrom(4096)` | Returns `(data, sender_address)` — up to 4096 bytes per call. |
| `os.makedirs(SAVE, exist_ok=True)` | Creates directory without error if it already exists. |

---

## Practical 9 — UDP File Transfer (Extended Version with Comments)

### Concept

Same concept as Practical 8 but written more explicitly with comments explaining each line. This version uses port `5005`, reads `sample.mp4`, and uses `b"EOF"` as the sentinel — demonstrating that the naming is arbitrary as long as sender and receiver agree.

Key UDP behaviour to note:
- Each `sendto()` call creates one datagram.
- Datagrams may arrive out of order or be lost (no built-in recovery here).
- For production use, a sequence number and ACK mechanism would be added on top.

---

### Code

**Server (Receiver)**
```python
import socket

SERVER_IP   = "0.0.0.0"   # Listen on all interfaces
SERVER_PORT = 5005
BUFFER_SIZE = 4096         # Max bytes per datagram

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.bind((SERVER_IP, SERVER_PORT))
print("Server listening on port", SERVER_PORT)

file = open("received_file", "wb")  # Open file for binary writing

while True:
    data, addr = sock.recvfrom(BUFFER_SIZE)  # Block until a datagram arrives

    if data == b"EOF":           # Application-level end signal
        print("File received successfully.")
        break

    file.write(data)             # Write raw bytes directly to file

file.close()
sock.close()
```

**Client (Sender)**
```python
import socket

SERVER_IP   = "192.168.1.10"   # IP of receiver machine
SERVER_PORT = 5005
BUFFER_SIZE = 4096
FILE_NAME   = "sample.mp4"

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

with open(FILE_NAME, "rb") as file:
    while True:
        chunk = file.read(BUFFER_SIZE)    # Read next 4KB chunk
        if not chunk:
            break                          # Reached end of file
        sock.sendto(chunk, (SERVER_IP, SERVER_PORT))

sock.sendto(b"EOF", (SERVER_IP, SERVER_PORT))   # Signal completion
print("File sent successfully.")
sock.close()
```

### Code-Level Explanation

| Element | Explanation |
|---|---|
| `socket.AF_INET` | IPv4 address family. |
| `socket.SOCK_DGRAM` | UDP — no connection, no guarantee of delivery. |
| `sock.recvfrom(BUFFER_SIZE)` | Receives one UDP packet at a time; blocks until data arrives. |
| `open("received_file", "wb")` | Binary write mode — handles any file type (images, video, binary). |
| `file.read(BUFFER_SIZE)` | Reads file in 4096-byte chunks matching the datagram size. |
| `b"EOF"` sentinel | Byte string used as stop signal; receiver checks for it after each packet. |

---

## Practical 10 — DNS Lookup (Forward & Reverse)

### Concept

**DNS (Domain Name System)** translates human-readable hostnames (like `google.com`) to IP addresses and vice versa.

- **Forward DNS:** hostname → IP address(es). Used by browsers and applications to connect to servers.
- **Reverse DNS:** IP address → hostname. Used for logging, spam filtering, and diagnostics.

Python's `socket` module provides built-in DNS functions:
- `socket.gethostbyname_ex(host)` → returns `(hostname, aliases, [IP list])`
- `socket.gethostbyaddr(ip)` → returns `(hostname, aliases, [IP list])` for reverse lookup

---

### Code

```python
import socket

def dns_lookup():
    inp = input("Enter IP address or hostname: ").strip()
    try:
        if inp[0].isdigit():
            # Input starts with digit → treat as IP → reverse DNS lookup
            hostname, _, ips = socket.gethostbyaddr(inp)
            print(f"Input      : {inp}")
            print(f"Hostname   : {hostname}")
            print(f"IP Address : {inp}")
        else:
            # Input is a hostname → forward DNS lookup
            _, _, ips = socket.gethostbyname_ex(inp)
            # Also do reverse lookup on first IP to confirm canonical hostname
            hostname_resolved = socket.gethostbyaddr(ips[0])[0] if ips else inp
            print(f"Input      : {inp}")
            print(f"Hostname   : {hostname_resolved}")
            print(f"All IPs    :")
            for ip in ips:
                print(f"  → {ip}")

    except socket.herror as e:
        print(f"Reverse DNS failed : {e}")   # IP → hostname failed (no PTR record)
    except socket.gaierror as e:
        print(f"DNS resolution failed : {e}") # hostname → IP failed (NXDOMAIN etc.)

dns_lookup()
```

### Code-Level Explanation

| Element | Explanation |
|---|---|
| `inp[0].isdigit()` | Quick check: if first character is a digit, assume it's an IP address. |
| `socket.gethostbyaddr(inp)` | Reverse DNS — sends PTR query; returns `(hostname, alias_list, address_list)`. |
| `socket.gethostbyname_ex(inp)` | Forward DNS — returns all IPs for the hostname (load-balanced servers may have many). |
| `ips[0]` for reverse | Gets canonical hostname from the first resolved IP for confirmation. |
| `socket.herror` | Error class for reverse DNS failures (no PTR record found). |
| `socket.gaierror` | Error class for forward DNS failures (bad hostname, no network, etc.). |

---

## Summary Table

| Practical | Topic | Language | Key Concept |
|---|---|---|---|
| 4 | Hamming Code | C++ | Error detection & single-bit correction using parity bits |
| 5 | Sliding Window | C++ | Go-Back-N vs Selective Repeat flow control |
| 6 | IP Subnetting | C++ | CIDR, subnet mask, network/broadcast address calculation |
| 7 | Dijkstra's Algorithm | C++ | Shortest path in weighted graphs using greedy relaxation |
| 8 | UDP File Transfer | Python | Connectionless file transfer with application-level framing |
| 9 | UDP File Transfer (Extended) | Python | Same as above with detailed comments and binary file support |
| 10 | DNS Lookup | Python | Forward and reverse DNS resolution using socket library |
