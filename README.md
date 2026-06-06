# Local-private-ai-stack
This is a self hosted local AI with open webui interface perfect for use locally and also can be used for familly everything and users group workspace can be controlled by admin console i use Tailscale for port forwarding insted of vpn
# Local Private AI Stack: Ollama + Open WebUI + Tailscale

A complete guide to deploying a 100% free, private, and locally-hosted ChatGPT alternative on Ubuntu Linux, configured with secure remote access to connect users across the globe without open router ports.

---

## 📈 System Architecture & Project Scope

This project demonstrates how to turn a standard consumer laptop into a completely secure, local AI server. It bypasses commercial API restrictions, guarantees absolute data privacy, and scales across local and virtual private networks.

* **Core Engine:** Ollama (Inference Runner)
* **User Interface:** Open WebUI (ChatGPT-style responsive frontend via Docker)
* **Global Access Layer:** Tailscale Mesh VPN (Secure peer-to-peer overlay network)
* **Target Hardware Used:** Lenovo ThinkPad X1 Carbon 7th Gen (Intel i5 8th Gen, 16GB RAM, Ubuntu Linux)

---

## 🛠️ Step-by-Step Deployment Guide

### Phase 1: Core AI Inference Engine Installation (Ollama)
First, bootstrap the background server that compiles and processes the weights of our Large Language Models (LLMs).

1. Install Ollama via the official automated Linux architecture script:
   ```bash
   --curl -fsSL [https://ollama.com/install.sh](https://ollama.com/install.sh) | sh

  Verify that the systemd daemon successfully initiated the service in the background:

   -- sudo systemctl status ollama
   Assign the active user account into the Docker group permissions array to avoid utilizing elevated root privileges (sudo) for daily containers execution:
--sudo usermod -aG docker $USER

  Crucial Step: Reload the Ubuntu session groups cache to activate permissions instantly without requiring a full machine reboot:
  --newgrp docker

Deploy the production branch of Open WebUI, utilizing host networking configurations for low-latency communication with the local Ollama socket layer:

--docker run -d --network=host -v open-webui:/app/backend/data -e OLLAMA_BASE_URL=[http://127.0.0.1:11434](http://127.0.0.1:11434) --name open-webui --restart always ghcr.io/open-webui/open-webui:main

 Phase 3:
 Web Setup & Initial Model AcquisitionOpen your web browser and navigate to http://localhost:8080.Register the initial master administrative account.Select the Workspace icon ($\mathbf{\boxplus}$) in the far-left sidebar panel.Navigate to the Models upper tab view and query/pull the targeted weights:llama3.2 (3 Billion parameters): Highly recommended for integrated CPU computing limits due to its balanced speed-to-intelligence output matrix. 


Global Access Layer Configuration (Connecting Across Borders)
To allow remote users (e.g., family or colleagues located overseas) to securely query the system without exposing vulnerable home ports to public WAN sniffing, we leverage a WireGuard-backed Mesh Network via Tailscale.

[Remote Client - Anywhere]
             │
      (Tailscale Tunnel)
             │
             ▼
 [Local Ubuntu Server - yourlocation] ──► (Open WebUI:8080) ──► (Ollama:11434)


 Install the Tailscale agent onto the local Ubuntu server node: --curl -fsSL [https://tailscale.com/install.sh](https://tailscale.com/install.sh) | sh

 Authenticate the node to register it securely onto your personal dashboard.

Install the Tailscale client on the remote user's device (Windows/macOS/Linux).

Log into the centralized Tailscale admin console, select the Ubuntu machine matrix node, and click "Share this machine" to generate an invitation payload for the remote user.

Capture your machine's dedicated backend static IPv4 allocation directly via terminal: --tailscale ip -4

The remote user can now access the local private AI interface from anywhere across the globe by opening their web browser and targeting: -- http://<YOUR_TAILSCALE_IP>:8080


Administrator Access Control Configurations
To ensure unauthorized users do not exhaust local processing hardware queues, Open WebUI operates under a strict principle of least privilege:

Role Promotion: Newly registered remote accounts enter a Pending state. The administrator must navigate to Admin Panel > Users to elevate their role authorization state to User.

Model Permissions & Access Matrix: Downloaded model schemas default to isolated access blocks. To share weights globally across authenticated users, navigate to Workspace > Models > Edit (Pencil Icon) > Visibility and toggle the attribute flag from Private to Public.

Project curated locally using open-source packages. 100% offline-capable, tracker-free, and enterprise privacy compliant.









 
