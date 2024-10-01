# Ansible role for nas media server install in docker

This repository contains a Docker Compose configuration for various NAS applications, including Pi-hole, qBittorrent, Radarr, Plex, Jackett, Sonarr, Watchtower, Requestrr, Tautulli, and Unmanic. This setup allows you to easily deploy these services on your home server.

## Description of services
Here's a brief explanation of each service included in your Docker Compose setup:

- `Pi-hole`: A network-wide ad blocker that filters out unwanted advertisements and trackers at the DNS level. It acts as a DNS server for your home network, blocking ads across all devices.

- `qBittorrent`: A lightweight, open-source BitTorrent client with a user-friendly Web UI. It allows you to download torrents while providing control over bandwidth, connections, and download schedules.

- `Radarr`: A movie collection manager that helps automate the process of downloading and organizing movies. It integrates with qBittorrent to download movies and monitors your collection for upgrades or missing files.

- `Plex`: A media server application that organizes and streams your movies, TV shows, music, and photos across various devices. Plex provides an elegant interface to browse your media library and stream it remotely.

- `Jackett`: A tool that acts as a proxy server, allowing Radarr and Sonarr to search for torrents from public and private torrent indexers. It simplifies torrent management by aggregating multiple indexers into one source.

- `Sonarr`: Similar to Radarr but for TV shows, Sonarr automates the downloading and organization of TV series. It helps maintain a clean TV library by automatically upgrading files when higher quality versions become available.

- `Watchtower`: An automatic update tool for Docker containers. Watchtower periodically checks for updates to the containers you're running and pulls newer images, ensuring you're always running the latest versions.

- `Requestrr`: A chatbot designed for requesting media (movies and TV shows) via platforms like Discord. It integrates with Radarr and Sonarr to allow users to request new media directly from a chat platform.

- `Tautulli`: A monitoring tool for Plex Media Server. Tautulli tracks activity, streaming statistics, and user behavior on your Plex server, providing insights and alerts based on usage patterns.

- `Unmanic`: A library management tool that automatically transcodes media files to more efficient formats. It can help reduce storage space used by your media library by converting files to modern codecs and formats.

## Prerequisites

- **Ansible**: Ensure Ansible is installed on your system to execute the ansible role.

## Getting Started

1. **Clone the repository**:

   ```bash
   git clone <repository_url>
   cd <repository_directory>
   ```

   Replace `<repository_url>` with the URL of your Git repository and `<repository_directory>` with the directory name where you cloned it.

2. **Customize the environment variables**:

   Before running the Docker Compose setup, you may need to customize the environment variables defined in the `docker-compose.yml` file. You can find these variables under the `environment` section of each service. Key variables to set include:

   - `role_nas_install_image_tag`: Docker image tag for each service.
   - `role_nas_install_tz`: Your local timezone (e.g., "Europe/Madrid").
   - `pihole_password`: The password for the Pi-hole web interface.
   - `role_nas_install_puid` and `role_nas_install_pgid`: User ID and Group ID for file permissions.

   Inventory:
   - `ansible_become_pass`: User password of the system (with sudoers permissions).
    - `ansible_user`: User of the system.

3. **Deploy the services**:

   Run the following command to start all services defined in the `docker-compose.yml` file:

   ```bash
   ansible-playbook -i inventories/local/hosts.ini playbook.yml
   ```

## Services Overview

### 1. Pi-hole
- **Image**: `pihole/pihole`
- **Ports**: 
  - TCP: `53` (DNS)
  - UDP: `53` (DNS)
  - Web interface: `8082`
- **Environment Variables**:
  - `TZ`: Time zone (e.g., "Europe/Madrid").
  - `WEBPASSWORD`: Password for the web interface.

### 2. qBittorrent
- **Image**: `lscr.io/linuxserver/qbittorrent`
- **Ports**: 
  - Web UI: `8080`
  - Torrenting: `6881`
- **Environment Variables**:
  - `PUID`: User ID.
  - `PGID`: Group ID.
  - `TZ`: Time zone.

### 3. Radarr
- **Image**: `lscr.io/linuxserver/radarr`
- **Ports**: 
  - Web UI: `7878`
- **Environment Variables**:
  - `PUID`: User ID.
  - `PGID`: Group ID.
  - `TZ`: Time zone.

### 4. Plex
- **Image**: `lscr.io/linuxserver/plex`
- **Network Mode**: Host
- **Environment Variables**:
  - `PUID`: User ID.
  - `PGID`: Group ID.
  - `TZ`: Time zone.
  - `VERSION`: Docker.

### 5. Jackett
- **Image**: `lscr.io/linuxserver/jackett`
- **Ports**: 
  - Web UI: `9117`
- **Environment Variables**:
  - `PUID`: User ID.
  - `PGID`: Group ID.
  - `TZ`: Time zone.

### 6. Sonarr
- **Image**: `lscr.io/linuxserver/sonarr`
- **Ports**: 
  - Web UI: `8989`
- **Environment Variables**:
  - `PUID`: User ID.
  - `PGID`: Group ID.
  - `TZ`: Time zone.

### 7. Watchtower
- **Image**: `containrrr/watchtower`
- **Purpose**: Automatically updates running Docker containers.

### 8. Requestrr
- **Image**: `thomst08/requestrr`
- **Ports**: 
  - Web UI: `4545`

### 9. Tautulli
- **Image**: `lscr.io/linuxserver/tautulli`
- **Ports**: 
  - Web UI: `8181`
- **Environment Variables**:
  - `PUID`: User ID.
  - `PGID`: Group ID.
  - `TZ`: Time zone.

### 10. Unmanic
- **Image**: `josh5/unmanic`
- **Ports**: 
  - Web UI: `8888`
- **Environment Variables**:
  - `PUID`: User ID.
  - `PGID`: Group ID.

## Stopping the Services

To stop all running containers, use:

```bash
docker-compose down
```

This command will stop and remove all containers defined in the `docker-compose.yml`.

## Conclusion

This Docker Compose setup provides a convenient way to deploy and manage various NAS services on your home server. Feel free to customize the configuration according to your needs, and consult the documentation for each application for further configuration options.
