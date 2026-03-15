# Cisco CML Breakout Automation

## Overview

The goal of this project is to automate access to running lab nodes in the Cisco Modeling Labs (CML) environment using the external Terminator terminal emulator on Ubuntu Linux.

The solution automatically opens terminal sessions to all nodes in a running lab, eliminating the need to manually configure individual Telnet connections for each node.

Two scripts — **`breakout`** and **`nodes`** — parse the lab configuration file (`labs.yaml`) and establish connections to all nodes in a single Terminator window, where each node session is opened in its own tab.

The scripts were developed and tested on **Ubuntu Linux (Intel CPU)** using the Terminator terminal emulator.

---

# Prerequisites

* Linux system running **Ubuntu**
* **Terminator** terminal emulator installed
* Access to **Cisco Modeling Labs**
* Breakout Tool for Linux (`breakout-linux-amd64`)

---

# Installation

### 1. Download the Breakout Tool

Download **`breakout-linux-amd64`** from the CML web interface:

```
Tools → Breakout Tool → Download
```

### 2. Make the file executable

```bash
chmod +x breakout-linux-amd64
```

### 3. Create a working directory

```bash
mkdir -p ~/cml/breakout
mv breakout-linux-amd64 ~/cml/breakout/
cd ~/cml/breakout
```

---

# Configuration

Generate the Breakout Tool configuration:

```bash
./breakout-linux-amd64 config
```

This command creates a **`config.yaml`** file.
**Note:** this is **not** the lab configuration file!

Edit the `config.yaml` file according to your CML environment settings:

```
console_start_port: 9000
controller: https://XXX.XXX.XXX.XXX   # IP address of your CML controller
extra_lf: false
lab_config_name: labs.yaml
listen_address: 127.0.0.1             # local loopback address
populate_all: false
ui_server_port: 8080
username: your_cml_username           # your CML username
password: your_cml_password           # your CML password
verify_tls: false                     # disable TLS verification
vnc_start_port: 5900
```

---

# Installing the Scripts

Download the two script files **`breakout`** and **`nodes`**, then move them to `/usr/bin`:

```bash
sudo mv breakout nodes /usr/bin
```

---

# Using the Scripts

1. Run the command:

```
breakout
```

This command returns a list of all nodes running in the CML lab.
**Do not close this session with `Ctrl+C`!**

2. Open a new terminal tab:

```
Ctrl + Shift + T
```

3. In the new tab, run:

```
nodes
```

Multiple tabs will be created automatically based on the list of nodes.

4. Optionally, rename the tabs according to the node labels by **double-clicking the tab name** in the terminal.

---

# How It Works

1. The **Breakout Tool** connects the local machine to the CML controller and the running lab.
2. The scripts parse the **`labs.yaml`** configuration file.
3. **Terminator** launches multiple tabs in a single window.
4. Each node is opened in its own tab with an active Telnet session.

This provides quick access to all lab nodes without manual connection setup.

---

# Use Case

This automation is particularly useful when working with **large network topologies** (for example labs with **10+ nodes**), where manual connection setup would otherwise be time-consuming.
