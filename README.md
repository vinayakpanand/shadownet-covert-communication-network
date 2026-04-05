# shadownet-covert-communication-network
A hands-on cybersecurity project simulating a covert  communications network using Docker, WireGuard VPN,  and automated backup/restore infrastructure.

**ShadowNet — Covert Communications Network**

ShadowNet is a hands-on cybersecurity project where I built and simulated a fully functional covert communications network from scratch on my local machine using Docker containers. The project involved spinning up three isolated server nodes — a relay node, a chat node, and a backup node — connected through a private Docker network called "shadownet". All communication between the nodes was secured using WireGuard VPN, a modern encryption protocol that uses ChaCha20 encryption to ensure that all traffic passing between nodes is completely unreadable to any outside observer. Once the encrypted tunnel was established and verified, I set up a real-time encrypted chat channel between the nodes using Netcat, demonstrating how secure covert communication works at the network level.
Beyond just setting up the network, I also built a complete infrastructure resilience system. I wrote bash scripts to automatically back up all WireGuard keys and configurations, compress them into encrypted archives, and securely transfer them to the backup node using SCP over SSH. To test the resilience of the system, I then simulated a complete network takedown by forcefully destroying Node 1 and Node 2 — wiping all keys, configs, and network settings. Finally, I ran the restore script which automatically pushed all backups back to freshly created nodes and brought the entire network back online within minutes, with the encrypted chat fully functional again.
This project gave me deep hands-on experience with Docker and containerization, WireGuard VPN and key management, Linux system administration, bash scripting and automation, secure file transfer using SCP and SSH, and disaster recovery planning — all skills that are directly applicable to real-world cybersecurity and DevOps roles.

This is a 100% educational project built on local Docker containers. No real network infrastructure was deployed.

**Architecture**

The network consists of three nodes running inside isolated Docker containers, all connected through a private bridge network called shadownet. Node 1 and Node 2 communicate through an encrypted WireGuard tunnel, while Node 3 sits separately as the backup server.

┌─────────────────────────────────────────────────────┐
│              Docker Network (shadownet)             │
│                                                     │
│   ┌─────────────┐    🔐 WireGuard    ┌────────────┐ │
│   │   NODE 1    │◄──────Tunnel──────►│   NODE 2    │ │
│   │relay-node   │                    │  chat-node  │ │
│   │ 10.0.0.1    │                    │  10.0.0.2   │ │
│   └─────────────┘                    └─────────────┘ │
│          │                                 │         │
│          └──────────────┬──────────────────┘         │
│                         │                            │
│                ┌────────▼────────┐                   │
│                │     NODE 3      │                   │
│                │  backup-node    │                   │
│                │   10.0.0.3      │                   │
│                └─────────────────┘                   │
└───────────────────────────────────────────────────────┘

**Node Breakdown**

Node 1 — Relay Node (10.0.0.1)
Node 1 acts as the primary relay server in the network. It is the initiating end of the WireGuard encrypted tunnel and is responsible for routing encrypted traffic to Node 2. In a real-world covert network, a relay node acts as the entry point — it receives incoming connections and forwards them securely to the appropriate destination. In this project, Node 1 holds its own WireGuard private key and Node 2's public key, which together establish the encrypted tunnel. It also runs the backup script that collects all critical configuration files and keys, compresses them, and securely transfers them to Node 3 for safe storage. Node 1 is the first node to be destroyed during the takedown simulation and the first to be restored from backup.

Node 2 — Chat Node (10.0.0.2)
Node 2 acts as the communications server — the receiving end of the encrypted tunnel. It listens on port 5000 for incoming messages from Node 1 using Netcat (ncat), enabling real-time two-way encrypted communication between the nodes. All messages exchanged between Node 1 and Node 2 travel through the WireGuard tunnel, meaning the traffic is fully encrypted end-to-end. In a real-world scenario, this node would host the actual communication application — a chat server, a file drop server, or any other covert messaging service. Like Node 1, it also runs a backup script that saves its WireGuard keys and configs to Node 3.

Node 3 — Backup Node (10.0.0.3)
Node 3 is the most critical node in the entire network from a resilience perspective. It acts as the isolated backup server — its sole job is to safely store encrypted backup archives from Node 1 and Node 2. It runs an OpenSSH server so that other nodes can securely transfer files to it using SCP. During the takedown simulation, Node 3 is the only node that survives — and it is from Node 3 that the entire network is restored. It holds the restore script that automatically pushes backup archives back to freshly created Node 1 and Node 2 containers, bringing the full network back online. The separation of the backup server from the operational nodes is a real-world best practice — if your operational infrastructure goes down, your backups must be on a completely separate, isolated system.

**Features**

Containerized Infrastructure — entire network simulated inside Docker containers on a single machine
WireGuard VPN Encryption — all inter-node traffic encrypted using ChaCha20 + Poly1305
Real-time Encrypted Chat — live two-way messaging between nodes through the encrypted tunnel
Automated Backup System — bash scripts collect, compress, and transfer all critical configs and keys to the backup node
Network Takedown Simulation — complete wipeout of Node 1 and Node 2 including all keys and configs
Full Disaster Recovery — one-script restore that brings the entire network back from backup within minutes
Key Management — WireGuard public/private key pairs generated and managed manually for each node
Secure File Transfer — SCP over SSH used for all backup transfers between nodes

**Future Improvements**

 Add Tor routing layer for additional anonymization
 Implement automatic scheduled backups using cron jobs
 Encrypt backup archives using GPG before storing
 Add a web-based monitoring dashboard for node health
 Add Node 4 as a secondary relay for redundancy
 Automate entire setup using docker-compose
 Add intrusion detection alerts between nodes
