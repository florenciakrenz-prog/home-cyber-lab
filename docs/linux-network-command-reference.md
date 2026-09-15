# Linux & Network Command Reference

> Practical command reference built from commands used during my Home Cyber Lab experiments.

This document is not intended to be a complete Linux cheat sheet. It contains commands I have actually used while configuring, troubleshooting, validating, and documenting my cybersecurity home lab.

---

## 1. Network Configuration & Interfaces

### `ip route`

```bash
ip route
```

Displays the system routing table.

I used it to identify:

- the default gateway;
- the active network interface;
- the local network range;
- the IP assigned to the Linux host.

Example information obtained in the lab:

```text
default via <gateway> dev wlp2s0
<local-network>/24 dev wlp2s0 src <local-ip>
```

Useful when establishing the basic network context before performing discovery.

---

### `ip addr`

```bash
ip addr
```

Displays network interfaces and their assigned addresses.

Useful for identifying:

- interface names;
- IPv4/IPv6 addresses;
- interface state;
- local addressing.

---

### `lspci -k`

```bash
lspci -k
```

Lists PCI devices and the kernel drivers associated with them.

During LAB-001 I used:

```bash
lspci -k | grep -A 4 -i network
```

This helped identify the Wi-Fi and Ethernet adapters and their respective drivers before investigating Wake-on-LAN support.

---

## 2. Connectivity Testing

### `ping`

```bash
ping <IP>
```

Tests whether a host responds to ICMP echo requests.

Example:

```bash
ping -c 4 <IP>
```

The `-c 4` option limits the test to four packets.

### Important lesson

A successful `ping` proves that the target responded to ICMP at that moment.

A failed discovery scan does **not automatically prove that a device is offline**.

During network discovery I observed a device responding to `ping` while another discovery method did not report it as active.

This reinforced an important investigation principle:

> Correlate multiple sources before reaching a conclusion.

---

## 3. Network Discovery with Nmap

### Host discovery

```bash
nmap -sn <network>/24
```

Performs host discovery without running a port scan.

Used to identify devices that may be active inside the authorized home network.

---

### Skip host discovery

```bash
nmap -Pn <IP>
```

Treats the target as online and skips the normal host-discovery phase.

Useful when a host does not respond as expected to discovery probes.

### Investigation note

Nmap results should be interpreted as evidence, not absolute truth.

Firewalls, operating-system behavior, wireless networking and probe handling can affect results.

---

## 4. Wake-on-LAN Investigation

### `ethtool`

```bash
sudo ethtool enp3s0
```

Displays capabilities and current configuration for the Ethernet interface.

During LAB-001 I used it to inspect Wake-on-LAN support.

Relevant fields included:

```text
Supports Wake-on:
Wake-on:
Link detected:
```

---

### Enable Magic Packet Wake-on-LAN

```bash
sudo ethtool -s enp3s0 wol g
```

Configures the interface to wake when it receives a Magic Packet.

Verification:

```bash
sudo ethtool enp3s0 | grep Wake-on
```

In the experiment the state changed from:

```text
Wake-on: d
```

to:

```text
Wake-on: g
```

This confirmed that Wake-on-LAN was enabled at the interface level.

It did **not** prove that the computer could successfully wake remotely because the final Ethernet test could not yet be performed.

---

## 5. HTTP Communication

### Start a temporary Python HTTP server

```bash
python3 -m http.server 8000
```

Starts a simple HTTP server on TCP port `8000`.

Used during LAB-002 to create real network communication between the Lenovo Linux host and the Motorola/Termux client.

---

### `curl`

```bash
curl http://<server-ip>:8000
```

Sends an HTTP request to the server.

The server recorded a successful request similar to:

```text
"GET / HTTP/1.1" 200
```

This provided evidence that the client reached the server successfully.

---

### Security lesson: working directory matters

Starting:

```bash
python3 -m http.server 8000
```

shares files from the **current working directory**.

During the experiment I initially launched the server from a directory that exposed more files than necessary.

I corrected this by creating an isolated directory and starting the server there.

The workflow became:

```text
Create → Observe → Detect → Correct → Retest
```

---

## 6. Ports and Services

### Check a web service with OpenSSL

```bash
openssl s_client -connect <IP>:<PORT>
```

Creates a TLS connection to a service and displays certificate and handshake information.

Useful when investigating whether a web interface is using TLS and what certificate it presents.

---

## 7. System Resources

### `free -h`

```bash
free -h
```

Displays RAM and swap usage in a human-readable format.

I used this while investigating performance and a system-resume problem on the Linux laptop.

Useful fields include:

- total memory;
- used memory;
- available memory;
- swap usage.

---

## 8. Logs and Troubleshooting

### `journalctl`

Linux systems using systemd store logs in the journal.

To inspect warnings from the previous boot:

```bash
journalctl -b -1 -p warning
```

Where:

- `-b -1` = previous boot;
- `-p warning` = warning priority and above.

This was useful after the laptop failed to resume normally from suspension.

---

### Inspect kernel logs within a time window

```bash
journalctl -b -1 -k --since "HH:MM" --until "HH:MM"
```

Where:

- `-b -1` selects the previous boot;
- `-k` restricts output to kernel messages;
- `--since` and `--until` define the investigation window.

This is much more useful than reading thousands of unrelated log entries.

### Investigation lesson

A warning occurring near an incident is **evidence of correlation**, not automatically proof of causation.

---

## 9. Text Processing

These commands are especially useful when working with logs, command output and host lists.

### `grep`

```bash
grep "pattern" file
```

Searches for matching text.

Example:

```bash
sudo ethtool enp3s0 | grep Wake-on
```

---

### `awk`

```bash
awk '{print $1}' file
```

Processes structured text by columns.

Useful when extracting IP addresses or specific fields from command output.

---

### `sort`

```bash
sort file
```

Sorts lines.

To remove duplicates:

```bash
sort -u file
```

---

### `comm`

```bash
comm file1 file2
```

Compares two sorted files.

Useful when comparing a saved network baseline against a later observation.

---

### `tr`

```bash
tr '<old>' '<new>'
```

Translates or removes characters from text streams.

Useful in small shell-processing pipelines.

---

### `mktemp`

```bash
mktemp
```

Creates a temporary file safely.

Useful in scripts that need intermediate data without relying on a fixed temporary filename.

---

### `sleep`

```bash
sleep 5
```

Pauses execution for a specified number of seconds.

Useful in scripts when waiting between observations or network checks.

---

## 10. Files and Directories

### Change directory

```bash
cd <directory>
```

Example:

```bash
cd ~/home-cyber-lab
```

---

### List files

```bash
ls
```

Detailed listing:

```bash
ls -la
```

---

### Create a directory

```bash
mkdir <directory>
```

Create parent directories when needed:

```bash
mkdir -p <path>
```

---

### Create or edit a text file

```bash
nano <file>
```

Nano keyboard shortcuts:

```text
Ctrl + O    Save
Ctrl + X    Exit
```

---

## 11. Git Workflow

### Check repository status

```bash
git status
```

One of the most important commands before committing.

It shows:

- modified files;
- staged files;
- untracked files;
- current branch.

---

### Stage a specific file

```bash
git add <file>
```

I prefer staging specific files instead of blindly using:

```bash
git add .
```

when unfinished experiments are present in the repository.

This reduces the risk of committing unrelated or sensitive files.

---

### Create a commit

```bash
git commit -m "Commit message"
```

---

### Push to GitHub

```bash
git push
```

Uploads local commits to the configured remote repository.

---

### Inspect remotes

```bash
git remote -v
```

Shows the remote repository addresses configured for Git.

---

## 12. SSH and GitHub

### Test GitHub SSH authentication

```bash
ssh -T git@github.com
```

Used to verify that the computer can authenticate with GitHub using the configured SSH key.

### Security rule

Private SSH keys must never be committed or published.

Examples of sensitive files:

```text
*.key
*.pem
.env
secrets/
private/
```

These patterns are excluded in the repository `.gitignore`.

---

## 13. Splunk

### Start Splunk

In my lab I created an alias for the local Splunk installation:

```bash
splunk-start
```

The alias runs the appropriate Splunk start command for this environment.

---

### Basic Splunk search

```spl
index=_internal
```

Searches Splunk's internal events.

---

### Filter errors

```spl
index=_internal log_level=ERROR
```

Useful for focusing on error-level internal events.

---

### Count events by component

```spl
index=_internal
| stats count by component
```

Aggregates events and helps identify which Splunk components are producing activity.

---

## 14. Evidence Handling

Technical screenshots are stored only when they demonstrate something meaningful.

Good evidence includes:

- configuration before/after;
- successful communication;
- relevant log entries;
- errors that affected an experiment;
- validation after a correction.

Before publishing evidence:

- hide credentials;
- hide tokens;
- hide private keys;
- blur or sanitize network identifiers when appropriate;
- remove unrelated personal information.

---

## 15. Investigation Workflow

The most important lesson from these commands is not memorizing their syntax.

It is knowing **why and when to use them**.

My current workflow is:

```text
Observe
   ↓
Form a hypothesis
   ↓
Validate
   ↓
Correlate evidence
   ↓
Document
```

A command produces data.

The analyst's job is to turn that data into evidence and determine what can — and cannot — be concluded from it.

---

## Related Experiments

- **LAB-001:** Wake-on-LAN — Motorola → Lenovo Linux
- **LAB-002:** HTTP Communication — Termux → Python HTTP Server
- **LAB-003:** Network Baseline & Device Discovery — in progress

---

*This reference evolves together with the Home Cyber Lab. New commands are added when they are actually used and understood during an experiment.*
