### *Common commands*
**Stop all docker containers**
```
sudo docker stop $(sudo docker ps -aq)
```

**Hide docker container's port from the internet and proxy with nginx**
First create a docker network.
```
sudo docker network create <network-name>
```

Then run your container with the following command.
```
sudo docker run -d --env-file .env --network <network-name> --name <container name> <container id>
```

Now check which IP address your `network-name`, find the `Containers` object and copy the `IPv4Address` for your nginx configuration.
```
sudo docker network inspect <network-name>
[
    {
        "Name": "<network-name>",
        "Id": "1f7b8029bbc3c9c37a8e16e6999ee35525b996b85ea6b14893a7fe3f00f9032b",
        "Created": "2024-01-04T18:38:16.543929918Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "6d7a754ee84514b614bbec0a5f93bb706210ce3d087184caae8ddc2b31f81649": {
                "Name": "<contianer-name>",
                "EndpointID": "0807a0f72dc85744f16a07847df34becd24730f8bb5279d958618e22fdb2530a",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16", //Use this IPV4 in nginx
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

```

In `/etc/nginx/sites-available/<nginx-config>` edit the proxy-pass and add the IPv4 address you copied from the previous command. For me, my container is running on port 3001.
```
server {
    listen 80;
    location / {
        proxy_pass http://172.18.0.2:3001;
    }
}
```