# Nodejs

## Install Instructions

```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```

use correct version number: 14 is current as of 2020-08-28

```
sudo apt install -y nodejs
```



## Server

```
sudo npm install -g serve
```

### HTTP Server

```
sudo npm install -g http-server
```

https://www.npmjs.com/package/http-server




## Server with HTTPS / Create Self Signed Certificates

sudo openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem

serve --ssl-cert ./cert.pem --ssl-key ./key.pem


### HTTPS

https://nodejs.org/en/knowledge/HTTP/servers/how-to-create-a-HTTPS-server/