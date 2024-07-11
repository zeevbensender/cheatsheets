# DOCKER GOOGLE DRIVE CLIENT
## Based on ```rclone```
Deploying a Google Drive client in a Docker container and mapping it to an external USB HDD for file synchronization is a fantastic way to manage your files efficiently. One of the most widely used tools for interacting with Google Drive is `rclone`. This step-by-step guide will walk you through setting up rclone in a Docker container and mapping your external USB HDD for synchronization.

### Prerequisites:
1. A machine with Docker installed.
2. An external USB HDD connected to your machine.
3. A Google Drive account.

### Step 1: Install Docker

If you don't have Docker installed, follow these instructions:

**For Ubuntu:**
```sh
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install docker-ce
```

**For macOS and Windows:**
Download and install Docker Desktop from [Docker's official website](https://www.docker.com/products/docker-desktop).

### Step 2: Create a Container Image with Rclone

We will need to create a Dockerfile to build our container image with rclone installed.

Create a directory for your Docker setup:
```sh
mkdir rclone-docker
cd rclone-docker
```

Create a `Dockerfile` with the following contents:
```dockerfile
FROM alpine:latest

RUN apk update && apk add --no-cache \
    rclone \
    fuse \
&& mkdir /root/.config

CMD ["sleep", "infinity"]
```

Build the Docker image:
```sh
docker build -t rclone-image .
```

### Step 3: Configure Rclone

Run a temporary container to generate the rclone configuration:
```sh
docker run -it --rm \
    -v $(pwd)/rclone:/root/.config/rclone \
    rclone-image rclone config
```

Follow the prompts to set up your Google Drive remote. Typically, you will:
1. Choose `n` for a new remote.
2. Name your new remote (e.g., `gdrive`).
3. Select type as `drive`.
4. Follow the instructions to authenticate with your Google account.

Make sure your configuration file is saved in `$(pwd)/rclone/rclone.conf`.

### Step 4: Run Rclone in a Persistent Container

Now, let’s set up and run the rclone container for continuous synchronization.

Determine your USB drive’s mount point. This will depend on your OS.

**For Linux:**
Attach the USB drive and find its mount point using:
```sh
lsblk
```
Assuming the mount point is `/media/usb`.

**For macOS:**
Typically mounted under `/Volumes`.

Create and run the Docker container:
```sh
docker run -d \
    --name rclone-sync \
    --device /dev/fuse \
    --cap-add SYS_ADMIN \
    --security-opt apparmor=unconfined \
    -v $(pwd)/rclone:/root/.config/rclone \
    -v /media/usb:/mnt/usb \
    rclone-image sh -c "rclone mount gdrive: /mnt/usb"
```

This command does the following:
1. Runs in detached mode (`-d`).
2. Names the container `rclone-sync`.
3. Adds necessary capabilities for FUSE.
4. Mounts the local rclone configuration.
5. Maps the USB drive to `/mnt/usb` in the container.
6. Uses the command `rclone mount gdrive: /mnt/usb` to mount your Google Drive.

### Step 5: Automate Synchronization (Optional)

For continuous synchronization, you can periodically run rclone sync using cron or similar tools. Here’s how to run it manually inside the container:
```sh
docker exec -it rclone-sync sh -c "rclone sync gdrive:/ /mnt/usb"
```

To automate, you can create a cron job on the host machine:
```sh
(crontab -l ; echo "0 * * * * docker exec rclone-sync sh -c 'rclone sync gdrive:/ /mnt/usb'") | crontab -
```

This job runs every hour (`0 * * * *`).

### Conclusion

You’ve now set up a Docker container with rclone connecting to Google Drive and mapped it to an external USB HDD for file synchronization. This setup allows for easy management of your Google Drive files directly from your local storage, providing a robust and scalable solution for file synchronization.

## Using ```docker compose``` instead of ```docker run``` 
### Step-by-Step Guide to Create Docker Compose File

1. **Create Docker Compose File**

Make sure you are inside your project directory (e.g., `rclone-docker`) and create a new file named `docker-compose.yml`:

```yaml
version: '3.8'

services:
  rclone-sync:
    image: rclone-image
    build: .
    container_name: rclone-sync
    devices:
      - /dev/fuse:/dev/fuse
    cap_add:
      - SYS_ADMIN
    security_opt:
      - apparmor:unconfined
    volumes:
      - ./rclone:/root/.config/rclone
      - /media/usb:/mnt/usb
    command: ["rclone", "mount", "gdrive:", "/mnt/usb"]
    restart: unless-stopped
```

2. **Build and Run with Docker Compose**

Navigate to your project directory and use Docker Compose to build and run the service:

```sh
docker-compose up -d
```

### Explanation of docker-compose.yml

- **version: '3.8'**: Specifies the Docker Compose file version.
- **services**: Defines the services.
  - **rclone-sync**: Name of the service.
    - **image: rclone-image**: Specifies the name of the image.
    - **build: .**: Specifies to build the Dockerfile in the current directory.
    - **container_name: rclone-sync**: Names the container.
    - **devices**: Adds the FUSE device.
    - **cap_add**: Adds SYS_ADMIN capability.
    - **security_opt**: Disables AppArmor for this container.
    - **volumes**: Maps local files and directories to the container.
      - `./rclone:/root/.config/rclone`: Mounts the configuration directory.
      - `/media/usb:/mnt/usb`: Maps the USB mount.
    - **command**: Overrides the default command to mount Google Drive to the USB mount point.
    - **restart**: Ensures the container restarts unless explicitly stopped.

### Step 3: Verify and Automate Synchronization

Optionally, create the synchronization script `sync.sh`:

```sh
#!/bin/sh
docker exec rclone-sync rclone sync gdrive:/ /mnt/usb
```

Make the script executable:
```sh
chmod +x sync.sh
```

Set up the cron job:
```sh
(crontab -l ; echo "0 * * * * /path/to/rclone-docker/sync.sh") | crontab -
```

This cron job will run every hour. Adjust the timing to fit your needs.
