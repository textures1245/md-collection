[[Important Note Contents]] #Linux

![[Deployment Infrastructure.excalidraw]]

## Linux Command

| Command               | Description                                                              |
| --------------------- | ------------------------------------------------------------------------ |
| `whoami`              | Display the current user's username                                      |
| `hostname`            | Display the system's hostname                                            |
| `pwd`                 | Display the current working directory                                    |
| `ls`                  | List files and directories in the current directory                      |
| `cd`                  | Change the current working directory                                     |
| `mkdir`               | Create a new directory                                                   |
| `rm`                  | Remove files or directories                                              |
| `mv`                  | Move or rename files and directories                                     |
| `cp`                  | Copy files and directories                                               |
| `chmod`               | Change file permissions                                                  |
| `chown`               | Change file ownership                                                    |
| `grep`                | Search for patterns in files                                             |
| `find`                | Search for files in a directory hierarchy                                |
| `sudo`                | Execute a command as a superuser or root                                 |
| `apt`/`yum`           | Package management tools for Debian/Ubuntu and CentOS/RHEL, respectively |
| `systemctl`           | Control and manage system services and daemons                           |
| `journalctl`          | View and manage systemd logs                                             |
| `ip`/`ifconfig`       | Configure and display network interfaces                                 |
| `netstat`/`ss`        | Display network connections and statistics                               |
| `ping`                | Test network connectivity                                                |
| `ssh`                 | Secure Shell client for remote access                                    |
| `scp`                 | Secure Copy for transferring files over SSH                              |
| `rsync`               | Utility for efficient file transfers and synchronization                 |
| `tar`                 | Create and extract archive files                                         |
| `gzip`/`gunzip`       | Compress and uncompress files                                            |
| `top`                 | Display real-time system processes and resource usage                    |
| `ps`                  | List running processes                                                   |
| `kill`                | Terminate a process                                                      |
| `vim`/`nano`          | Text editors for editing files                                           |
| `cat`                 | Concatenate and display file contents                                    |
| `tail`/`head`         | Display the last or first lines of a file                                |
| `service`/`systemctl` | Start, stop, and manage system services                                  |
|                       |                                                                          |
## Basis Docker Commands

```shell
# Connect to the remote server via SSH
ssh user@remote_server_ip

# Check Docker version
docker --version

# Pull a Docker image
docker pull image_name

# List all Docker images
docker images

# List running containers
docker ps

# Run a Docker container
docker run -d -p host_port:container_port image_name

# Stop a running container
docker stop container_id

# Start a stopped container
docker start container_id

# View logs of a container
docker logs container_id

# Execute a command inside a running container
docker exec -it container_id /bin/bash

# Copy files/directories to a container
docker cp /path/to/file container_id:/path/in/container

# Copy files/directories from a container
docker cp container_id:/path/in/container /path/to/file

# Commit changes to a new image
docker commit container_id new_image_name

# Push an image to a registry
docker push image_name

# Remove a container
docker rm container_id

# Remove an image
docker rmi image_id
```

```bash
# Check Linux distribution
cat /etc/os-release

# Update package lists
sudo apt update (Ubuntu/Debian)
sudo yum update (CentOS/RHEL)

# Install packages
sudo apt install package_name (Ubuntu/Debian)
sudo yum install package_name (CentOS/RHEL)

# Check system resources (CPU, memory, disk)
top
df -h
free -m

# Check open ports
sudo ss -tulpn

# Configure firewall rules
sudo ufw allow port_number (Ubuntu)
sudo firewall-cmd --add-port=port_number/protocol (CentOS/RHEL)

# Start/stop/restart service
sudo systemctl start/stop/restart service_name

# Check service status
sudo systemctl status service_name

# Edit configuration files
nano /path/to/file
vi /path/to/file

# Copy files/directories
cp source_path destination_path
scp user@host:/path/to/file /path/to/destination (copy from remote)
scp /path/to/file user@host:/path/to/destination (copy to remote)

# Create/delete directories
mkdir directory_name
rmdir directory_name

# View log files
tail -n 20 /path/to/log_file
```