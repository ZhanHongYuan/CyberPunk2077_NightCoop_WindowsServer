# NightCoop Hub — Windows One-Click Deployment

<p align="center">
  **An easier way to host your own NightCoop Hub on Windows**
</p>

A portable Windows x64 deployment package for NightCoop Hub.

No need to install Rust, Visual Studio, Docker, or a development environment. Simply download, extract, configure, and launch your own Hub using the included batch scripts.

This project aims to make NightCoop Hub easier to deploy for Windows users, allowing more Cyberpunk 2077 NightCoop players to host their own Hubs and enjoy co-op sessions with friends.

---

## ⚠️ Disclaimer and Project Notice

**This project is an independently created, third-party Windows deployment package. It is NOT an official NightCoop project and is NOT affiliated with the NightCoop development team.**

- NightCoop is an unofficial fan-made modification. It is not affiliated with or endorsed by CD PROJEKT RED.
- Cyberpunk 2077 is a trademark of CD PROJEKT S.A.
- This project only provides a convenient Windows deployment package. It does not own NightCoop, Cyberpunk 2077, or any third-party components.
- Copyrights and licenses for NightCoop Hub and its dependencies remain with their respective authors.
- Compatibility with different NightCoop versions is not guaranteed. Please refer to the upstream NightCoop project for supported versions.

This project is intended for learning, research, and private co-op use with friends. Users are responsible for ensuring that their use complies with the applicable software, game, and mod terms.

**This project is not affiliated with, endorsed by, or sponsored by CD PROJEKT RED, CD PROJEKT S.A., or the official NightCoop team.**

## Related Projects and Official Documentation

| Project | Link |
|---|---|
| NightCoop Official Website | https://nightcoop.net/ |
| NightCoop Installation and Usage Guide | https://nightcoop.net/docs.html#install |
| NightCoop Hub Upstream Repository | https://github.com/Twenfys/nightcoop-hub |

If you are new to NightCoop, please read the official installation guide first to understand client installation, multiplayer setup, and version compatibility.

---

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Requirements](#requirements)
- [Download and Installation](#download-and-installation)
- [First Launch](#first-launch)
- [Server Configuration](#server-configuration)
- [Connecting Game Clients](#connecting-game-clients)
- [Key Management](#key-management)
- [Network and Firewall Configuration](#network-and-firewall-configuration)
- [Logs and Troubleshooting](#logs-and-troubleshooting)
- [Common Issues](#common-issues)
- [Security Recommendations](#security-recommendations)
- [License and Credits](#license-and-credits)

---

## Introduction

NightCoop Hub is the Hub server component used by the NightCoop multiplayer system.

The upstream Hub project provides source code and deployment instructions. Users can deploy it using Docker or by building the source themselves.

This project provides a portable Windows deployment package based on the upstream Hub, combining a prebuilt executable with convenient management scripts.

It allows ordinary Windows users to deploy their own Hub without setting up a development or compilation environment.

With this package, you can:

- Run a Hub on your Windows PC.
- Deploy a Hub on Windows Server or a Windows VPS.
- Share your Hub connection information with friends.
- Customize supported server settings such as the Hub name, port, and client limit.
- Run independent Hubs for different groups of players.

**Important: This package deploys NightCoop Hub, not a Cyberpunk 2077 game server.** The game world is still provided by the host player through the NightCoop multiplayer system.

## Features

- **One-click Windows launch:** Start your Hub using a batch script.
- **No manual compilation:** Includes a prebuilt Windows x64 executable.
- **Independent signing key:** Each deployment instance uses its own signing key.
- **Configuration management:** Adjust supported server settings through the configuration file.
- **Log recording:** Store runtime logs for troubleshooting.
- **Key backup:** Includes a dedicated key backup script.
- **Firewall setup:** Includes a script for opening the default UDP port.

## Requirements

| Item | Requirement |
|---|---|
| Operating System | Windows 10 / Windows 11 / Windows Server |
| Architecture | x64 |
| Network | A network reachable by game clients |
| Permissions | Administrator privileges for Windows Firewall configuration |
| Development Environment | No Rust, Visual Studio, or Docker required |

For cloud deployments, you must also open the required UDP ports in your cloud provider's security group.

## Download and Installation

### 1. Download

Visit the GitHub Releases page of this project and download the latest Windows portable package.

Extract the archive into a dedicated directory, for example:

```text
D:\NightCoop-Hub\
```

Avoid placing the program in Windows system directories, temporary folders, or other protected locations.

### 2. Directory Structure

The extracted directory should look similar to this:

```text
NightCoop-Hub/
│
├─ nightcoop_hub.exe
│
├─ Start-Server.bat
├─ Open-Firewall-Port.bat
├─ Backup-Key.bat
│
├─ config.json
│
├─ data/
│   └─ (local signing key and data)
│
└─ logs/
    └─ (runtime logs)
```

Actual filenames may vary depending on the published release.

Do not delete, move, or modify program files unless you know what they are used for.

## First Launch

### Step 1: Configure Windows Firewall

Run:

```text
Open-Firewall-Port.bat
```

Allow administrator privileges when prompted by Windows.

This script adds a Windows Firewall inbound rule for the default Hub UDP port.

If you are using a cloud server, you must also open the same UDP port in your cloud provider's security group.

### Step 2: Start the Hub

Run:

```text
Start-Server.bat
```

During the first launch, the package initializes the required local data and signing key according to its configuration.

Once started successfully, the console will display the Hub runtime information.

Pay attention to:

- Hub name
- Listening port
- Hub fingerprint
- Server startup status

Save your Hub address and fingerprint. Other players will need them to connect.

### Step 3: Keep the Hub Running

Keep the console window open while the Hub is running.

Closing the window or terminating the process may take the Hub offline and prevent other players from connecting.

To stop the Hub, use:

```text
Ctrl + C
```

in the console and allow the program to exit normally.

---

## Server Configuration

Open the following file in the program directory:

```text
config.json
```

Modify the supported settings according to the options and comments provided in the file.

Common configuration items include:

| Setting | Description |
|---|---|
| Hub Name | The name displayed to players |
| Listening Port | The UDP port used by the Hub |
| Shards / Threads | Server shard or thread configuration |
| Maximum Clients | Maximum number of allowed clients |
| Region | Server region identifier |

It is recommended to stop the Hub before changing the configuration.

After making changes, restart the server using the launch script.

**Important: Always refer to the actual `config.json` included in your release. Do not add unsupported parameters.**

If you change the port or thread configuration, make sure to update your Windows Firewall and cloud security group rules accordingly.

---

## Connecting Game Clients

### 1. Install the NightCoop Client

All players joining the same multiplayer session need to install the NightCoop client mod and its required dependencies.

Make sure that:

- Your Cyberpunk 2077 game version is supported by your NightCoop build.
- All players in the same session use compatible NightCoop builds.
- The client mod is installed in the correct game directory.
- The Co-op option appears in the game menu.

For installation instructions, see the official guide:

https://nightcoop.net/docs.html#install

### 2. Obtain the Hub Connection Information

The Hub administrator needs to provide:

1. The Hub address.
2. The Hub signing-key fingerprint.

Example using a domain name:

```text
hub.example.org#K3QF-9X2M-TV7A-0D4R
```

Example using an IP address:

```text
203.0.113.7#K3QF-9X2M-TV7A-0D4R
```

Example using a non-default port:

```text
hub.example.org:47900#K3QF-9X2M-TV7A-0D4R
```

These are examples only. Replace them with your own server address and fingerprint.

### 3. Add a Custom Hub

In the game, open:

```text
Co-op → Server: ... → Add an own server
```

Enter or paste the address and fingerprint provided by the Hub administrator, then save.

After a successful connection, the game will display the server status and verification information.

### 4. Invite Other Players

After the host creates a multiplayer session, use NightCoop's Copy invite feature to copy the invitation information.

The invite typically contains:

- The session code.
- The Hub address.
- The Hub fingerprint.

Other players can paste the invite into Join by code to join the session.

For more details, see the official guide:

https://nightcoop.net/docs.html#install

---

## Key Management

Each Hub should have its own independent signing key.

This package is designed so that each user maintains a separate key in their own deployment directory instead of sharing another user's server identity.

### Key Location

The package uses a dedicated data directory:

```text
data/
```

This directory stores the local signing key and related data.

Do not delete, overwrite, or move an existing key unless you understand the consequences.

### Back Up Your Key

Run:

```text
Backup-Key.bat
```

Follow the instructions provided by the script.

It is recommended to store the backup in another secure location, such as an encrypted USB drive or controlled private storage.

### Why Is Key Backup Important?

The Hub signing key is associated with the server's identity.

If the original key is deleted or lost, generating a new key may change the Hub identity.

Clients may then need to verify the server again and update the saved fingerprint.

Therefore:

- Do not regenerate the key unnecessarily.
- Never upload your private signing key to GitHub.
- Never share your private key in public chats or public file-sharing services.
- Do not reuse the same private key across unrelated Hub deployments.

**The Hub fingerprint can be shared with players. The private signing key must never be shared.**

---

## Network and Firewall Configuration

### Default Port

NightCoop Hub uses the following default UDP port:

```text
47800 / UDP
```

Make sure that UDP traffic is allowed in both locations:

**Windows Firewall**

Run the included script:

```text
Open-Firewall-Port.bat
```

**Cloud Server Security Group**

Add an inbound rule in your cloud provider's console:

| Protocol | Port | Direction |
|---|---|---|
| UDP | 47800 | Inbound |

### Shards / Threads Ports

According to the NightCoop official guide, additional thread-related ports may also be required:

```text
47810+
```

If these ports are closed, creating a multiplayer session may take longer. The official documentation mentions an additional delay of approximately 5–6 seconds.

Multiplayer may still work, but server administrators should check the actual thread configuration and listening ports, then open the required additional UDP ports.

Official documentation:

https://nightcoop.net/docs.html#install

### Hosting on a Home PC

If you host the Hub on a home computer:

- Your computer must remain powered on.
- The Hub process must keep running.
- Your router may require UDP port forwarding.
- If your ISP uses CGNAT, normal port forwarding may not provide public access.

### Hosting on a Cloud Server

For Windows cloud server deployments, the following are recommended:

- Use Windows Server x64.
- Use a static public IP or a stable domain name.
- Open only the required UDP ports.
- Back up the signing key regularly.
- Monitor logs and disk usage.

---

## Logs and Troubleshooting

Log directory:

```text
logs/
```

If the Hub fails to start or players cannot connect, check the following:

1. Look for error messages in the console.
2. Check whether logs are generated in the `logs` directory.
3. Make sure the configuration file has not been modified incorrectly.
4. Check whether another program is using the UDP listening port.
5. Make sure Windows Firewall allows the required traffic.
6. Make sure the cloud security group allows the required UDP traffic.
7. Verify that the client is using the correct Hub address and fingerprint.

### Common Issues

| Error or Symptom | Possible Cause | Suggested Solution |
|---|---|---|
| Hub fails to start | Invalid configuration or missing files | Check the configuration and program directory |
| Server does not answer | Hub is offline or UDP port is blocked | Check the Hub process and firewall |
| Server is not found | Domain name cannot be resolved | Check the domain or use the public IP |
| Server failed the key check | Fingerprint does not match the server key | Ask the administrator for the correct address and fingerprint |
| Session creation takes longer | Thread-related UDP ports are closed | Check ports 47810+ |
| Hub identity changes after restart | Original key was lost or replaced | Restore the original key backup |

### Client Logs

If the problem occurs inside the game client, you can also check the NightCoop and RED4ext logs.

The official documentation lists these locations:

```text
Cyberpunk 2077\red4ext\logs
```

And:

```text
Cyberpunk 2077\red4ext\plugins\NightCoop\logs
```

When reporting a client issue, record the time, what you were doing, and whether you were hosting or joining. Follow the official guide for submitting logs.

---

## Security Recommendations

Please follow these security practices:

1. **Never share your private signing key.** It is an important part of your Hub identity.
2. **Never commit private keys to GitHub.** This includes public repositories and uncontrolled backup repositories.
3. **Never publicly distribute a complete server directory containing private keys.**
4. **Do not publish personal public IP addresses, access credentials, or sensitive logs in public repositories.**
5. **Do not open unnecessary network ports.**
6. **Back up your signing key and configuration regularly.**
7. **If you suspect that your key has been compromised, follow the key-management procedures supported by the upstream project.**

---

## License and Credits

This project is a third-party Windows deployment wrapper for NightCoop Hub.

NightCoop Hub upstream repository:

https://github.com/Twenfys/nightcoop-hub

NightCoop official website:

https://nightcoop.net/

This project does not claim ownership of the upstream project or any third-party dependencies. The use, copying, and redistribution of each component must comply with its respective license.

Before redistributing this package, please review the licenses and redistribution requirements of the upstream software and its dependencies.

Special thanks to the NightCoop developers and the authors of all related open-source dependencies.

---

<p align="center">

**Enjoy Night City with your friends!**

</p>
