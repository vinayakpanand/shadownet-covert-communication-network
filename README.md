# shadownet-covert-communication-network
A hands-on cybersecurity project simulating a covert  communications network using Docker, WireGuard VPN,  and automated backup/restore infrastructure.

**ShadowNet — Covert Communications Network**

ShadowNet is a hands-on cybersecurity project where I built and simulated a fully functional covert communications network from scratch on my local machine using Docker containers. The project involved spinning up three isolated server nodes — a relay node, a chat node, and a backup node — connected through a private Docker network called "shadownet". All communication between the nodes was secured using WireGuard VPN, a modern encryption protocol that uses ChaCha20 encryption to ensure that all traffic passing between nodes is completely unreadable to any outside observer. Once the encrypted tunnel was established and verified, I set up a real-time encrypted chat channel between the nodes using Netcat, demonstrating how secure covert communication works at the network level.
Beyond just setting up the network, I also built a complete infrastructure resilience system. I wrote bash scripts to automatically back up all WireGuard keys and configurations, compress them into encrypted archives, and securely transfer them to the backup node using SCP over SSH. To test the resilience of the system, I then simulated a complete network takedown by forcefully destroying Node 1 and Node 2 — wiping all keys, configs, and network settings. Finally, I ran the restore script which automatically pushed all backups back to freshly created nodes and brought the entire network back online within minutes, with the encrypted chat fully functional again.
This project gave me deep hands-on experience with Docker and containerization, WireGuard VPN and key management, Linux system administration, bash scripting and automation, secure file transfer using SCP and SSH, and disaster recovery planning — all skills that are directly applicable to real-world cybersecurity and DevOps roles.

This is a 100% educational project built on local Docker containers. No real network infrastructure was deployed.

**Architecture**

The network consists of three nodes running inside isolated Docker containers, all connected through a private bridge network called shadownet. Node 1 and Node 2 communicate through an encrypted WireGuard tunnel, while Node 3 sits separately as the backup server.

<img width="647" height="508" alt="image" src="https://github.com/user-attachments/assets/ca87aaa0-f9cb-49e1-b1cf-a757cbff7577" />


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

---------------------------------------------------------------------------------------------------------
<img width="880" height="348" alt="1" src="https://github.com/user-attachments/assets/6becc929-d5d3-43c9-aabd-fcdd24196c13" />

**Built 3 isolated server nodes using Docker**

Instead of buying expensive hardware, I simulated 
an entire server infrastructure on my laptop using 
Docker containers.

Each node runs Ubuntu 22.04 and acts as an 
independent server — just like real-world deployments.

Node 1 → Relay Server

Node 2 → Chat Server  

Node 3 → Backup Server

This is containerization in action — the same 
technology used by Netflix, Google, and Amazon 
to run their infrastructure.

<img width="410" height="177" alt="2" src="https://github.com/user-attachments/assets/81f294c8-deac-4b7f-b208-eeca844f120b" />

**Created a private isolated network**

Real covert networks don't use the public internet 
for internal communication.

I created "shadownet" — a private Docker bridge 
network that isolates all 3 nodes from the outside 
world.

Think of it like building a private LAN inside 
your laptop. All nodes can talk to each other, 
but nobody outside can see them.

This is network segmentation — a core principle 
in enterprise cybersecurity.

<img width="860" height="495" alt="4" src="https://github.com/user-attachments/assets/54f0e4d8-0a76-436b-b33a-69af1a8b21c8" />

**Encrypted all traffic using WireGuard VPN**

This is where it gets serious.

I set up WireGuard — a modern, blazing-fast VPN 
protocol used by cybersecurity professionals worldwide.

Every single packet traveling between nodes is 
now fully encrypted using military-grade 
cryptography (ChaCha20 + Poly1305).

Even if someone intercepts the traffic, they 
cannot read it without the private keys.

The keys are hidden for security — as they 
should be in any real deployment.

<img width="634" height="362" alt="5" src="https://github.com/user-attachments/assets/584bc32e-6d05-4aad-b115-53c6f19a48a6" />

**Verified the encrypted tunnel
**
A network is useless if it doesn't work reliably.

I tested the WireGuard tunnel by sending ICMP 
ping packets from Node 2 to Node 1 through 
the encrypted tunnel.

Result:
-4 packets sent

-4 packets received  

-0% packet loss

-Average latency: 2.1ms

The tunnel is rock solid. All traffic is flowing 
securely between nodes — completely encrypted 
end to end.

<img width="1919" height="384" alt="6" src="https://github.com/user-attachments/assets/cad2dd06-7a8c-44cb-8efd-6f2c3a2c3f9b" />

**Live encrypted communication between nodes**

This is the moment everything came together.

Using Netcat (a networking tool used by security 
researchers worldwide), I established a real-time 
encrypted chat channel between Node 1 and Node 2.

Left window → Node 1 (Relay) sending messages

Right window → Node 2 (Chat) receiving messages

Every message travels through the WireGuard 
encrypted tunnel — completely private and 
unreadable to any outside observer.

This is the foundation of how secure messaging 
apps like Signal work at the network level.

<img width="962" height="1017" alt="13" src="https://github.com/user-attachments/assets/cea45a01-bdcf-4cb9-b10f-43f4645a3eb1" />

** Automated backup of entire infrastructure**

**What happens if your servers get wiped?**

I wrote a bash script that automatically:

-Collects all WireGuard keys and configs

-Bundles them into a compressed archive

-Securely transfers everything to the backup server (Node 3) using SCP

**Both node backups are now safely stored 
on the isolated backup server:**

node1_backup.tar.gz — 489 bytes

node2_backup.tar.gz — 471 bytes

This is infrastructure-as-code and disaster 
recovery planning in practice.

<img width="905" height="383" alt="15" src="https://github.com/user-attachments/assets/4eae3f15-3720-4bf3-82bf-fc020ea2c283" />

**Simulated a full network takedown**

The real test of any infrastructure is — 
what happens when everything goes down?

I simulated a complete network seizure by 
forcefully destroying Node 1 and Node 2:

docker rm -f node1

docker rm -f node2

All WireGuard configs, encryption keys, and 
network settings — completely wiped.

Only Node 3 (backup server) survived.

This is exactly what happens in real-world 
scenarios — server failures, ransomware attacks, 
or infrastructure seizures. 

The question is: can you recover? 

<img width="961" height="1021" alt="18" src="https://github.com/user-attachments/assets/7bb1e906-e8c7-40dc-b43c-093d66d12824" />

**Full restore — back online in seconds**

From complete destruction to fully operational 
in under 2 minutes — purely from backup!

The restore script automatically:
-Pushed configs back to new Node 1 & Node 2

-Restored all WireGuard encryption keys

-Re-established the encrypted tunnel

-Brought the chat server back online

As you can see — both nodes are chatting again 
through the encrypted tunnel, completely restored!

This is why backups and disaster recovery 
planning are not optional in cybersecurity — 
they are essential.

Tools used: Docker • WireGuard • Linux • 
Bash Scripting • SCP • OpenSSH

#cybersecurity #docker #wireguard #linux 
#infosec #networking #bash #devops 
#ethicalhacking #learnincybersecurity
