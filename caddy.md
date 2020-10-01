# Caddy

More information: https://caddyserver.com/

A reverse proxy is used to create a unified front and as a TLS boundary.

| URL        | Use                                  | Target        | Port |
| ---------- | ------------------------------------ | ------------- | ---- |
| /          | Website                              | Webserver     | 3000 |
| /getconfig | Arduino Config from Node             | Pirate-Bridge | 9876 |
| /ctrl      | POST Control of Arduino through Node | Pirate-Bridge | 9876 |
| /stream    | SSE-Stream from Node                 | Pirate-Bridge | 9876 |
| /janusopt  | Janus WebRTC Stream                  | Janus-Gateway | 8088 |



## Caddyfile

Configfile: To route 

```
{    
admin :34567 # expose admin port
}
wappler.me  # domain under which to serve

reverse_proxy /* localhost:3000  # Website
reverse_proxy /getconfig* raspberrypi:9876 # Pirate-Bridge
reverse_proxy /ctrl raspberrypi:9876 # Pirate-Bridge
reverse_proxy /stream { # Pirate-Bridge
	flush_interval -1
	to raspberrypi:9876
	}
route /janusopt/* { 
    uri strip_prefix /janusopt
    reverse_proxy raspberrypi:8088 # Janus-Gateway control port
}

tls "<path_to_cert>.crt" "<path_to_public_key>.key"  # enable tls 

```
