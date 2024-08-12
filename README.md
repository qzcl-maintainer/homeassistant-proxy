# homeassistant-proxy
Author: maintainer@qzconsultancy.com
Last Update: 11-Aug-2024
### Purpose: Create a Home Assistant Proxy running on NGINX and using SSL for securing your HA Installation

### Prerequisites
1. Home Assistant must be running and working.
2. Docker must be installed and running.
3. Generate your ssl certificates using certbot and place in the `.ssl` directory.

### Instructions
1. Start your docker container running on port 80 first
```
cd /opt
git clone https://github.com/qzcl-maintainer/homeassistant-proxy.git
cd homeassistant-proxy
docker compose -f compose-http.yaml -d
```

2. Connect to your Docker container
```docker container exec -it homeassistant-proxy-nginx-1 bash```

3. Generate your dhparams file
```
cd /etc/nginx/ssl
sudo openssl dhparam -out dhparams.pem 2048
exit
```

4. Bring the instance down
```docker compose down```

5. Edit the `default.conf` and change the following parameters to match your installation
- server_name
- ssl_certificate file name
- ssl_certificate_key file name
- proxy_pass IPv4 Address and Port

The `proxy_pass` value should be your Home Assistant IP and Port.

6. Bring your Proxy online with SSL Configuration.
```docker compose up -d```

### References:
1. [Community Guide Reference 1](https://community.home-assistant.io/t/reverse-proxy-using-nginx/196954)
2. [Community Guide Reference 2](https://community.home-assistant.io/t/home-assistant-with-nginx-reverse-proxy/628138/3)
