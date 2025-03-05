# This repository contains the documentation and details for an application I have developed. It is hosted in a private repository and is intended solely for personal use and reference. The code and resources are not currently available for public access

This repository provides a system for fire detection using a camera stream and a Flask web application. It includes multiple components:

![Camera Stream](/Doc/UML1.png)
![Camera Stream](/Doc/Skärmbild.png)

- **Raspberry Pi Camera Setup**: This part of the system uses a Raspberry Pi to capture a video feed.
- **Fire Detection**: The fire detection model is used to identify fires in the video stream.
- **Dashboard**: A Flask web application that shows the video feed from multiple Raspberry Pi devices, providing real-time fire detection updates.
- **Docker Setup**: The system is containerized using Docker for easy deployment across various platforms.

## Setup Instructions

### Prerequisites

1. **Docker**: Make sure Docker is installed on your system for building and running containers.
2. **Ansible**: This is required for automating the setup on your Raspberry Pi devices.

## 📌 What You Need  

### Steps to Deploy

- **Raspberry Pi and Webcam**  
  - Set up your Pi OS using the Pi Imager tool → [Download here](https://www.raspberrypi.com/software/)  
  - Create a username: `pi`  
  - Set a password of your choice  
  - (Optional) Set a **static IP** for the Pi  

- **Install Ansible on Your System**  

  ```bash
   sudo nano /etc/apt/sources.list
   Add the following line to the file:
   deb http://http.kali.org/kali kali-rolling main non-free contrib
   ```

   Then, run:

   ```bash
   sudo apt-get update
   sudo apt-get install ansible
   ```

   Run the Setup Script inside the `ansible` directory after adding the host, username and password to the `inventory.ini` file:

   ```bash
   ansible-playbook -i inventory.ini setup_pi.yml -vvv
   ```

  - you can delete the repository when the setup is done and all your caneras are hosting correctly, because you will runn the app using docker compose as i will show you in the next section
  - or you can keep it for future use :) .

- **Configure Telegram bot**

  - [Watch this video](https://www.youtube.com/watch?v=l5YDtSLGhqk)
  
   ```bash
   BOT_TOKEN = "your token here" 
   CHAT_ID = "your chat id here"
   ```

### Docker Compose setup

- Create a folder and inside the folder create a `docker-compose.yml` and a `cameras.json`.
- in the `docker-compose.yml` file add the following code, and replace token and chat id with your own:

```yaml
  version: '3.8'

  services:
    fire-app:
      container_name: fire-detection
      image:
      restart: unless-stopped
      environment:
        - BOT_TOKEN="you token here"
        - CHAT_ID="chat id here"
        - MDOEL_SENSITIVITY=0.6 #default is 0.6 you can adjust it between (0.0 to 1.0)
      volumes:
        - ./cameras.json:/app/server/cameras.json
      ports:
        - "5001:5001"
```

- in the `cameras.json` file add the following code, and replace the links with your own, and use the names you want and add as much cameras as you want to monitor and dont forget to add ',' at the end of each line except the last one:

```json
{
    "room1 or name of your choice": "link to that camera",
    "room2 or name of your choice": "link to that camera"
}
```

- You need to have Docker and Docker Compose installed on your system, or Docker Desktop if you're using macOS or Windows.
- [Docker Compose](https://docs.docker.com/compose/install/)

- Run the following command to start the containers:

```bash
docker compose up -d
```

### Troubleshooting

- If you encounter any issues with Docker or Ansible, make sure you have the latest versions installed.
- For connectivity issues with Raspberry Pi, verify the SSH credentials and network connection.

### License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
