<h1 align="center"> NetPractice </h1>

<p align="center"> <img src="https://media.giphy.com/media/UZQCbV4OW1mXdHJNPS/giphy.gif" alt="NetPractice gif" width="300" /> </p>

## Table of contents

- [Description](#description)
- [How the project works](#how-the-project-works)
- [Networking concepts](#networking-concepts)
- [Instructions](#instructions)
- [Repository structure and submission](#repository-structure-and-submission)
- [Evaluation](#evaluation)
- [Resources](#resources)

## Description

**NetPractice** is a hands-on introduction to computer networking. Instead of writing code, the goal is to **diagnose and repair simulated small-scale networks** so that every device can communicate as required.

Each of the **10 levels** presents a network diagram that does not work. The diagram contains hosts, routers and switches, and each device has network interfaces and routing tables with editable fields. One or more goals are shown at the top of the page (for example, *"host A needs to communicate with host C"*). The challenge is to adjust the configuration, namely **IP addresses, subnet masks, default gateways and routes**, until all goals are reached.

The networks are fictitious and run entirely in a training interface in the browser. They are not connected to any real network.

What this project teaches:

- How to read and write IPv4 addresses and masks, including in binary
- How to split a network into subnets and compute network, broadcast and host ranges
- How hosts decide between talking directly or going through a gateway
- How routers forward packets using routing tables
- How to read logs to understand exactly where and why a packet is dropped

## How the project works

1. A **training interface** (a local web page) displays a level with a faulty network.
2. You edit the unshaded fields: interface IPs, masks and routing tables.
3. **Check again** simulates packets between the devices named in the goals and reports the result.
4. The **logs** at the bottom of the page show each hop a packet takes, and where it stops if something is wrong (missing gateway, invalid IP, no matching route, and so on).
5. When a level is solved, **Get my config** exports the configuration, and **Next level** moves on.

## Networking concepts

A short summary of the ideas used throughout the levels.

### IPv4 and binary

An IPv4 address is a 32-bit number written as four bytes (octets), each from 0 to 255. Each bit of a byte has a weight that is a power of two:

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|:---:|:--:|:--:|:--:|:-:|:-:|:-:|:-:|

For example, `192.168.1.130` is `11000000.10101000.00000001.10000010`.

### Subnet masks and CIDR

A **subnet mask** is a run of 1s followed by 0s. The 1s mark the **network** part of the address, and the 0s mark the **host** part. CIDR notation (`/26`) simply counts the 1s.

| CIDR | Mask | Block size | Usable hosts |
|:----:|:----:|:----------:|:------------:|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

**Finding the network of an IP** (example: `192.168.1.130/26`):

1. Block size = 256 − 192 = **64**, so networks start at 0, 64, 128, 192.
2. 130 falls in the block starting at **128**, so the network address is `192.168.1.128`.
3. The broadcast address is the last of the block: `192.168.1.191`.
4. Usable host addresses: `192.168.1.129` to `192.168.1.190`.

The network address and the broadcast address can never be assigned to a device.

### Default gateways and routing

- Two devices in the **same network** talk directly.
- To reach a **different network**, a host sends the packet to its **default gateway**, which must be an address of a router **inside the host's own network**.
- A **routing table** maps destination networks to a next hop. The route `0.0.0.0/0` is the **default route**, used when nothing more specific matches.
- A router always prefers the **most specific** matching route (the longest mask).

### Routers and switches

- A **router** connects different networks and forwards packets between them using its routing table.
- A **switch** connects devices **within the same network** and does not route.

### OSI model

Addressing and routing live at the network layer (**layer 3**), while switching works at the data link layer (**layer 2**). The OSI model is a useful way to reason about which device is responsible for what.

### Rules of thumb for the levels

1. Both ends of a link must be in the **same network** with the **same mask**.
2. Different links must use **different, non-overlapping networks**.
3. No interface can use a **network** or **broadcast** address.
4. No two interfaces in the same network can share an **IP**.
5. Every host needs a **gateway** pointing to the router interface in its own network.
6. Routers need **routes** for every network they are not directly connected to.

## Instructions

### Requirements

- A Unix-like shell
- A web browser
- `python3` (used by the fallback method and by `run.sh` to serve the pages)

### Running the training interface

1. Download the archive attached to the project page and extract it into any folder.
2. From inside that folder, run:

   ```sh
   ./run.sh
   ```

   This starts a local web server and opens your default browser on the NetPractice page.

3. If `run.sh` does not work, start the server manually:

   ```sh
   python3 -m http.server 49242
   ```

   Then open `http://localhost:49242` in your browser. The port number can be changed.

> A local web server is required because of technical and security restrictions in web browsers; opening the HTML files directly will not work.

### Practicing

- **Training tab**: enter the login `mariaalm` and click **Start!**. This loads the personal configuration used for the submission.
- **Evaluation tab**: leave the login empty to generate a random configuration, exactly like in a defense. This is the best way to practice.

### Exporting configurations

1. Solve a level until the status shows success.
2. Click **Get my config** and save the downloaded file.
3. Only then click **Next level**.
4. Repeat for all 10 levels.

Entering your login in the interface is essential: the exported files depend on it.

## Repository structure and submission

The **10 exported configuration files (one per level)** must be placed at the **root of the repository**, next to this README, and none of them may be empty.

```
.
├── README.md
└── <10 configuration files exported with "Get my config", one per level>
```

Only the content of the repository is evaluated during the defense.

## Evaluation

During the defense, three random levels must be solved correctly in a limited time (15 minutes), with no internet or external resources. A simple calculator such as `bc` is tolerated, but it is the limit. For each level, the reasoning behind every value must be explained.

## Resources

### Video playlist

- ▶️ [Networking video playlist used to study for this project](https://www.youtube.com/watch?v=5WfiTHiU4x8&list=PLIhvC56v63IKrRHh3gvZZBAGvsvOhwrRF)
