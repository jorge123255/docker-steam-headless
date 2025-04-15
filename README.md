# Headless Steam Service

![](./images/banner.jpg)

Remote Game Streaming Server.

Play your games either in the browser with audio or via Steam Link or Moonlight. Play from another Steam Client with Steam Remote Play.

Easily deploy a Steam Docker instance in seconds.

## Features:
- Steam Client configured for running on Linux with Proton
- Moonlight compatible server for easy remote desktop streaming
- Easy installation of EmeDeck, Heroic and Lutris via Flatpak
- Full video/audio noVNC web access to a Xfce4 Desktop
- NVIDIA, AMD and Intel GPU support
- Full controller support
- Support for Flatpak and Appimage installation
- Root access
- Based on Debian Bookworm

---
## Notes:

### ADDITIONAL SOFTWARE:
If you wish to install additional applications, you can generate a script inside the `~/init.d` directory ending with ".sh".
This will be executed on the container startup.

Also, you can install applications using the WebUI under **Applications > System > Software**. There you can install other game launchers like Lutris, Heroic or EmuDeck.

### STORAGE PATHS:
Everything that you wish to save in this container should be stored in the home directory or a docker container mount that you have specified. 
All files that are store outside your home directory are not persistent and will be wiped if there is an update of the container or you change something in the template.

### GAMES LIBRARY:
It is recommended that you mount your games library to `/mnt/games` and configure Steam to add that path.

### AUTO START APPLICATIONS:
In this container, Steam is configured to automatically start. If you wish to add additional services to automatically start, 
add them under **Applications > Settings > Session and Startup** in the WebUI.

### NETWORK MODE:
If you want to use the container as a Steam Remote Play (previously "In Home Streaming") host device you should create a custom network and assign this container it's own IP, if you don't do this the traffic will be routed through the internet since Steam thinks you are on a different network.

### USING HOST X SERVER:
If your host is already running X, you can just use that. To do this, be sure to configure:
  - DISPLAY=:0    
    **(Variable)** - *Configures the sceen to use the primary display. Set this to whatever your host is using*
  - MODE=secondary    
    **(Variable)** - *Configures the container to not start an X server of its own*
  - HOST_DBUS=true    
    **(Variable)** - *Optional - Configures the container to use the host dbus process*
  - /run/dbus:/run/dbus:ro    
    **(Mount)**  - *Optional - Configures the container to use the host dbus process*


---
## Installation:
- [Docker Compose](./docs/docker-compose.md)
- [Unraid](./docs/unraid.md)
- [Ubuntu Server](./docs/ubuntu-server.md)


---
## Running locally:

For a development environment, I have created a script in the devops directory.


---
## TODO:
- Remove SSH
- Require user to enter password for sudo
- Document how to run this container:
    - Other server OS
    - TrueNAS Scale 

# Steam Headless for Unraid

This repository contains the configuration for running Steam Headless on Unraid.

## Installation Instructions

1. In Unraid, go to the "Apps" tab
2. Search for "steam-headless"
3. Click "Install" or "Actions > Install" from the search results

## GPU Support

### NVIDIA
1. Install the [Nvidia-Driver Plugin](https://forums.unraid.net/topic/98978-plugin-nvidia-driver/) by ich777
2. In the steam-headless container template:
   - Toggle to "Advanced View"
   - Add `--runtime=nvidia` to the "Extra Parameters" field
   - If you have multiple NVIDIA GPUs, set the correct GPU UUID in the `NVIDIA_VISIBLE_DEVICES` variable

### AMD
1. Install the [Radeon-Top Plugin](https://forums.unraid.net/topic/92865-support-ich777-amd-vendor-reset-coraltpu-hpsahba/) by ich777

## Controller Support
1. Install the "uinput" plugin from the Apps tab
2. Ensure the container's "Network Type" is set to "host"

## Volume Mappings
- `/mnt/user/steam-headless/home` → `/home/default`
- `/mnt/user/steam-headless/games` → `/mnt/games`

## Environment Variables
- `TZ`: Timezone (default: America/New_York)
- `USER_PASSWORD`: Password for the default user (change this!)
- `WEB_UI_MODE`: Set to "vnc" for web access
- `PORT_NOVNC_WEB`: Web UI port (default: 8083)
- `ENABLE_STEAM`: Set to "true" to enable Steam
- `ENABLE_SUNSHINE`: Set to "true" for Moonlight streaming support
