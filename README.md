# Home Assistant Proxy with NGINX and Docker

**Author:** maintainer@qzconsultancy.com  
**Last Update:** 11-Aug-2024

## Overview
This guide provides step-by-step instructions to set up a reverse proxy for your [Home Assistant](https://www.home-assistant.io) installation using NGINX and Docker. The proxy is configured to secure your Home Assistant instance with SSL certificates.

## Prerequisites
Before proceeding, ensure that the following are in place:
1. **Home Assistant** is running and fully operational.
2. **Docker** is installed and running on your system.
3. SSL certificates have been generated using `certbot` and stored in the `.ssl` directory.

## Environment
- **Home Assistant Version:** 2024.8.1
- **Hardware:** Raspberry Pi 4

## Setup Instructions

### 1. Clone the Repository
First, clone the repository containing the configuration files.
```bash
cd /opt
sudo git clone https://github.com/qzcl-maintainer/homeassistant-proxy.git
sudo chmod 777 /opt/homeassistant-proxy
cd homeassistant-proxy
mkdir ssl
```

### 2. Start the HTTP Proxy (Without SSL)
Launch the proxy using the HTTP configuration to verify that it works correctly.

```bash
docker compose -f compose-http.yml up --detach
```

### 3. Access the NGINX Container
Connect to the running NGINX Docker container to generate Diffie-Hellman parameters.

```bash
docker container exec -it homeassistant-proxy-nginx-1 bash
```

### 4. Generate Diffie-Hellman Parameters
Generate the dhparams.pem file, which is used for SSL/TLS key exchange.

```bash
cd /etc/nginx/ssl
openssl dhparam -out dhparams.pem 2048
exit
```

### 5. Shut Down the Proxy
After verifying the HTTP proxy setup, bring down the container to apply SSL configurations.

```bash
docker compose down
```

### 6. Edit the NGINX Configuration
Modify the `default.conf` file to match your Home Assistant installation:

- **server_name:** Your server's domain name.
- **ssl_certificate:** The file path to your SSL certificate.
- **ssl_certificate_key:** The file path to your SSL certificate key.
- **proxy_pass:** The IP address and port of your Home Assistant instance.

### 7. Start the HTTPS Proxy (With SSL)
Finally, bring the proxy online with SSL enabled.

```bash
docker compose up --detach
```

### 8. Confirm Access to Your New Proxy
Navigate to `https://your_server_name:8124`. You should see the Home Assistant Dashboard.



### Troubleshooting
If you encounter any issues during setup, refer to the community guides:
1. [Community Guide Reference 1](https://community.home-assistant.io/t/reverse-proxy-using-nginx/196954)
2. [Community Guide Reference 2](https://community.home-assistant.io/t/home-assistant-with-nginx-reverse-proxy/628138/3)
