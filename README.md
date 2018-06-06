# DNCA Path Trace

Path trace traces a path through the network from host A to host B. You can also provide TCP/UDP port information and the controller will look at points in the network where there is multiple paths and let you know which of the possible paths will be used for this flow. In addition you can get device and interface statistics along the active path. With the DNA Center Platform APIs, you can put path trace to work wherever you want.

## Getting started

These instructions will get you a copy of the Python code for DNAC path trace up and running on your local machine for development and testing purposes.

## Requirements

- Python 2.7.10 or higher
- Python 3.6.3 or higher
- "git" command line tools
- Homebrew (Mac OS X)

## Mac OS X Installation

```
git installation - https://git-scm.com/download/mac
```
```
homebrew installation - ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```
```
Python 3.6 installation - https://www.python.org/downloads/release/python-364/
Python 2.7 installation - https://www.python.org/downloads/release/python-2714/
Python pip installation
curl -o get-pip.py https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
```
Command Line Developer Tools Installation. After running command, complete installation using the GUI.
```          
xcode-select --install
```

## Windows Installation
```
git installation - https://git-scm.com/download/win
Python 3.6 installation - https://www.python.org/downloads/release/python-364/
Python 2.7 installation - https://www.python.org/downloads/release/python-2714/
Be sure to check box for "Add Python to PATH" during the installer
```

## 'GIT' this code

All of the code and examples for this lesson is located in the 'add me here' directory. Clone and access it with the following commands:

```
git clone 'add me here'
cd
```
Use pip to install the necessary requirements
```
pip install -r requirements.txt
```

### Code features

- An easy-to-use single Python script for monitoring network health, identifying issue root causes, and remediating issues
- Run a path trace from source to destination to quickly get key performance statistics for each device along the network path
- Troubleshoots issues along the network path

## DNAC API reference

If you look at the Python code for our script, you will see the API calls used.

- `https://{}/api/system/v1/auth/token` Gets and encapsulates user identity and role information as a single value that RBAC-governed APIs use to make access-control decisions.
- `https://{}/api/v1/network-device` Gets the list of first 500 network devices sorted lexicographically based on host name. It can be filtered using management IP address, mac address, hostname and location name.
- `https://{}/api/v1/interface` Gets every interface on every network device. Whilst you can get a list of all interfaces via an API call, it is often more useful to get a subset of them. For example those that are attached to a specific network-device.
- `https://{}/api/v1//host` You can use the host API to get the name of a host, the ID of the VLAN that the host uses, the IP address of the host, the MAC address of the host, the IP address of the network device to which host is connected, and more.

## Running path trace Python scripts

The script `path_trace.py` requires two arguments to see how this works run the `path_trace_prep.py` script. In this script we use `Argparse` is a built in Python module which makes easy to write user-friendly command-line interfaces. The program defines what arguments it requires. Argparse will figure out how to parse those out of sys.argv. We will use Argparse to input our host source and destination IP addresses.

```
python path_trace.py -h
usage: path_trace.py [-h] source_ip destination_ip

positional arguments:
  source_ip       Source IP Address
  destination_ip  Destination IP Address

optional arguments:
  -h, --help      show this help message and exit
```

The source and destination IP address of the hosts. The Source and destination can also be addresses of interfaces on network-devices as well.

- Source: Enter the IP address from which you want the trace to start
- Destination: Enter the IP address, host name, username, or application name at which you want the trace to end

:warning: Path traces from the a router's loopback interface or a wireless controller's management interface are not supported

## Example Path trace script

```
python path_trace.py 10.10.22.98 10.10.22.114
Running Troubleshooting Script for
      Source IP:      10.10.22.98
      Destination IP: 10.10.22.114

Source Host Details:
-------------------------
Host Name: Unavailable
Network Type: wired
Connected Network Device: 10.10.22.66
Connected Interface Name: TenGigabitEthernet1/0/1
VLAN: 1
Host IP: 10.10.22.98
Host MAC: c8:4c:75:68:b2:c0
Host Sub Type: UNKNOWN

Destination Host Details:
-------------------------
Host Name: Unavailable
Network Type: wired
Connected Network Device: 10.10.22.70
Connected Interface Name: TenGigabitEthernet1/0/24
VLAN: 1
Host IP: 10.10.22.114
Host MAC: 00:1e:13:a5:b9:40
Host Sub Type: UNKNOWN

Source Host Network Connection Details:
---------------------------------------------
Device Hostname: cat_9k_1.abc.inc
Management IP: 10.10.22.66
Device Location: None
Device Type: Cisco Catalyst 9300 Switch
Platform Id: C9300-24UX
Device Role: ACCESS
Serial Number: FCW2136L0AK
Software Version: 16.6.1
Up Time: 144 days, 21:42:05.16
Reachability Status: Reachable
Error Code: None
Error Description: None

Attached Interface:
--------------------
Port Name: TenGigabitEthernet1/0/1
Interface Type: Physical
Admin Status: UP
Operational Status: up
Media Type: 100/1000/2.5G/5G/10GBaseTX
Speed: 1000000
Duplex Setting: FullDuplex
Port Mode: access
Interface VLAN: 1
Voice VLAN: None

Destination Host Network Connection Details:
---------------------------------------------
Device Hostname: cat_9k_2.abc.inc
Management IP: 10.10.22.70
Device Location: None
Device Type: Cisco Catalyst 9300 Switch
Platform Id: C9300-24UX
Device Role: ACCESS
Serial Number: FCW2140L039
Software Version: 16.6.1
Up Time: 144 days, 21:39:55.40
Reachability Status: Reachable
Error Code: None
Error Description: None

Attached Interface:
--------------------
Port Name: TenGigabitEthernet1/0/24
Interface Type: Physical
Admin Status: UP
Operational Status: up
Media Type: 100/1000/2.5G/5G/10GBaseTX
Speed: 1000000
Duplex Setting: FullDuplex
Port Mode: access
Interface VLAN: 1
Voice VLAN: None

Running Flow Analysis from 10.10.22.98 to 10.10.22.114
-------------------------------------------------------
Flow analysis not complete yet, waiting 5 seconds
Number of Hops from Source to Destination: 5
()
Flow Details:
****************************************
Hop 1: Network Device cat_9k_1.abc.inc
Device IP: 10.10.22.66
Device Role: ACCESS
()
Ingress Interface
--------------------
Port Name: TenGigabitEthernet1/0/1
Interface Type: Physical
Admin Status: UP
Operational Status: up
Media Type: 100/1000/2.5G/5G/10GBaseTX
Speed: 1000000
Duplex Setting: FullDuplex
Port Mode: access
Interface VLAN: 1
Voice VLAN: None

Egress Interface
--------------------
Port Name: TenGigabitEthernet1/1/1
Interface Type: Physical
Admin Status: UP
Operational Status: up
Media Type: unknown
Speed: 10000000
Duplex Setting: FullDuplex
Port Mode: routed
Interface VLAN: None
Voice VLAN: None

****************************************
Hop 2: Network Device cs3850.abc.inc
Device IP: 10.10.22.69
Device Role: DISTRIBUTION
()
Ingress Interface
--------------------
Port Name: TenGigabitEthernet1/1/2
Interface Type: Physical
Admin Status: UP
Operational Status: up
Media Type: SFP-10GBase-CX1
Speed: 10000000
Duplex Setting: FullDuplex
Port Mode: routed
Interface VLAN: None
Voice VLAN: None

Egress Interface
--------------------
Port Name: TenGigabitEthernet1/1/3
Interface Type: Physical
Admin Status: UP
Operational Status: up
Media Type: SFP-10GBase-CX1
Speed: 10000000
Duplex Setting: FullDuplex
Port Mode: routed
Interface VLAN: None
Voice VLAN: None

****************************************
Hop 3: Network Device cat_9k_2.abc.inc
Device IP: 10.10.22.70
Device Role: ACCESS
()
Ingress Interface
--------------------
Port Name: TenGigabitEthernet1/1/1
Interface Type: Physical
Admin Status: UP
Operational Status: up
Media Type: unknown
Speed: 10000000
Duplex Setting: FullDuplex
Port Mode: routed
Interface VLAN: None
Voice VLAN: None

Egress Interface
--------------------
Port Name: TenGigabitEthernet1/0/24
Interface Type: Physical
Admin Status: UP
Operational Status: up
Media Type: 100/1000/2.5G/5G/10GBaseTX
Speed: 1000000
Duplex Setting: FullDuplex
Port Mode: access
Interface VLAN: 1
Voice VLAN: None
```

## Acknowledgements

- Hank Preston -  :e-mail: hapresto@cisco.com
- Adam Radford - :e-mail: aradford@cisco.com
