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

# https://github.com/pi-hole/docker-pi-hole/blob/master/README.md

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    # For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "80:80/tcp"
    environment:
      TZ: 'America/Chicago'
      # WEBPASSWORD: 'set a secure password here or it will be random'
    # Volumes store your data between container upgrades
    volumes:
      - './etc-pihole/:/etc/pihole/'
      - './etc-dnsmasq.d/:/etc/dnsmasq.d/'
      # run `touch ./var-log/pihole.log` first unless you like errors
      # - './var-log/pihole.log:/var/log/pihole.log'
    # Recommended but not required (DHCP needs NET_ADMIN)
    #   https://github.com/pi-hole/docker-pi-hole#note-on-capabilities
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
    
```

### Resources Used

Here is a [link](https://github.com/ElastiCourse/pihole/blob/master/Pi-Hole%20on%20Docker%20script) to an installation guide that I used.


## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/lbertaux1/Docker.Project/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/lbertaux1/Docker.Project/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
