---
title: Homelab Setup
date: 2024-04-25T22:45:00+05:30
---

## My homelab setup

I've been interested in trying out self-hosting and set up my own 'home-lab' for some time now. A few months back, I finally decided to experiment with a VM from [Hetzner](https://www.hetzner.com/cloud/).

I created the VM from the hetzner console and enabled ssh for access. To improve security a bit, I disabled password based ssh login and root user login. 
Additionally, I enabled fail2ban which automatically blocks IP addresses trying to brute force for a specific time. 
Anyone familiar with internet accessible VMs will understand the pain in dealing with relentless barrage of requests on ssh and other ports.

Next, I wanted  to setup a way to host and access apps without directly exposing ports on my box, while access should be limited from my devices.

I thought about setting up wireguard VPN but accessing apps via IP did not sound too appealing. 
Thats when I came accross tailscale. It uses wireguard and is straightforward to setup, for free tier usage they also provide a dns name per device.
It does require you to setup an account with them and routing requests via their network. Its built on top of sorcery of NAT traversal. I would refer to their [blog](https://tailscale.com/blog/how-nat-traversal-works) to understand it more deeply.


##### docker-compose example for tailscale setup
```
services:
  tailscale:
    image: tailscale/tailscale:latest
    container_name: tailscaled
    restart: unless-stopped
    network_mode: host
    cap_add:
      - net_admin
      - net_raw
    volumes:
      - ./data:/var/lib/tailscale
      - /dev/net/tun:/dev/net/tun
    environment:
      TS_STATE_DIR:
      TS_HOSTNAME:
      TS_AUTHKEY:
```


Once I had a basic idea of how I will access the box for setting up the applications, I wanted to use containers. 
My day job involves alot of work involving kubernetes and containers, so I am pretty familiar with them. Although I hadn't used Caddy previously, the ease of setup
and easy configuration appealed me.

I launched caddy in container and its own network.


##### caddy setup example
```
services:
  caddy:
    image: caddy:latest
    restart: on-failure
    ports:
      - "127.0.0.1:80:80"
    networks:
      - internal
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config
volumes:
  caddy_data:
    external: true
  caddy_config:
networks:
  internal:
    name: caddy-network
```

For adding new applications I update the caddyfile with new route and launch them using docker in the caddy network.
I can access these apps easily from any of my devices by connecting to tailscale via the box's dns name.


##### example for caddyfile
```
<dns>.ts.net:80 {
    handle_path /app1/* {
        reverse_proxy app1-server:8080
    }
    handle_path /app2/* {
        reverse_proxy app2-server:8081
    }
}
```

##### docker compose example for an application
```
services:
  app1-server:
    image: app1-server:latest
    container_name: app1-server
    networks:
      - internal
    volumes:
      - ./config:/home/app1/config
networks:
  internal:
    name: caddy-network
```

I can also use the node as a proxy server by setting it as a tailscale exit node. 
Im currently running an rss reader,a bookmark manager, llamafile for my personal LLM, an uptime monitor and a remote vscode server on it


There are a few things that I am yet to try out running on the VM including hosting my Valheim server, trying ssl with tailscale and caddy. Also, tailscale recently announced open access to their ssh setup, so want to see if its an improvement over openssh

Hope this post was helpful in providing some information and one of the approaches for self hosting. 
