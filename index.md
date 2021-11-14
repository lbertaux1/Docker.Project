## PiHole Installation Using Docker on Ubuntu

### Lab 2, Option C - PiHole

### Step 1: Disable Ubuntu DNS Service

We need to free Port 53 so that we can use our own DNS service to connect to PiHole. Execute the two following commands to do that.

```
sudo systemctl stop systemd-resolved.service 
sudo systemctl disable systemd-resolved.service
```

Then, remove the symbolic link for resolv.conf to avoid future overrides.

```
sudo rm /etc/resolv.conf
```

Next, we must temporarily change the nameserver because we still need to have access to the internet even after disabling DNS. Execute the following command.

```
sudo nano /etc/resolv.conf
```

Change the nameserver in the file to 1.1.1.1. Save the file and exit nano.

You can ping a website to ensure that your DNS resolution is now working again.

```
ping google.com
```

### Step 2: Configure and Run PiHole Container on Docker

Download the PiHole files for Docker by running the following command.

```
wget https://raw.githubusercontent.com/pi-hole/docker-pi-hole/master/docker_run.sh
```

Now, we must give executable permission for the shell script.

```
sudo chmod +x docker_run.sh 
```

Now, we are ready to run the shell script. The first time I ran it,  I got an error saying that the request timed out. I ran it a second time and was successful.

```
sudo ./docker_run.sh
```

You are now ready to access the PiHole Web Interface. Open your browser of choice and go to localhost/admin.

### Docker-compose.yml code block

```
version: "3"

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "80:80/tcp"
    environment:
      TZ: 'America/Chicago'
    WEBPASSWORD: <redacted>
    volumes:
      - './etc-pihole/:/etc/pihole/'
      - './etc-dnsmasq.d/:/etc/dnsmasq.d/'
      # run `touch ./var-log/pihole.log` first unless you like errors
      # - './var-log/pihole.log:/var/log/pihole.log'
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
    
```

### Resources Used

Here is a [link](https://github.com/ElastiCourse/pihole/blob/master/Pi-Hole%20on%20Docker%20script) to an installation guide that I used.

### Screenshot of PiHole interface

Here is a [link](https://drive.google.com/file/d/1l074pybglFmPDn2zpKf6lHy7DHcqYLXY/view?usp=sharing) to a screenshot of my PiHole interface.


