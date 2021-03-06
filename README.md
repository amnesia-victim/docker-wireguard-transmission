# docker-wireguard-transmission
Docker image for running Transmission over a WireGuard connection, based on Alpine Linux

## Changelog
**2020-06-01:** First release, most notable change since initial upload is the addition of two environemnt variables *INTERFACE* and *KILLSWITCH*. Read more about these in the usage section below.

## Usage
* **KILLSWITCH** let's you bind Transmission to only use the assigned VPN IP. If you don't want to use it you only need to leave the environment variable empty.
* **INTERFACE** let's you customize which WireGuard config file that wg-quick will load. Default: wg0 which loads /etc/wireguard/wg0.conf via the choosen mount volume. (See: *-v /path/to/wg-conf-dir*)

### docker run
```
docker run --name wireguard-transmission \
--privileged \
-e "USERNAME=transmission" \
-e "PASSWORD=transmission" \
-e "INTERFACE=wg0" \
-e "KILLSWITCH=wg0" \
-p 51820:51820/udp \
-p 9091:9091 \
-v /path/to/wg-conf-dir:/etc/wireguard \
-v /path/to/transmission-conf-dir:/etc/transmission-daemon \
-v /path/to/transmission-complete-dir:/transmission/complete \
-v /path/to/transmission-incomplete-dir:/transmission/incomplete \
-v /path/to/transmission-watch-dir:/transmission/watch \
sebdanielsson/wireguard-transmission
```

### docker-compose.yml
```
version: '3.3'
services:
    wireguard-transmission:
        container_name: wireguard-transmission
        privileged: true
        environment:
            - USERNAME=transmission
            - PASSWORD=transmission
            - INTERFACE=wg0
            - KILLSWITCH=wg0
        ports:
            - '51820:51820/udp'
            - '9091:9091'
        volumes:
            - '/path/to/wg-conf-dir:/etc/wireguard'
            - '/path/to/transmission-conf-dir:/etc/transmission-daemon'
            - '/path/to/transmission-complete-dir:/transmission/complete'
            - '/path/to/transmission-incomplete-dir:/transmission/incomplete'
            - '/path/to/transmission-watch-dir:/transmission/watch'
        image: sebdanielsson/wireguard-transmission
```

## To-Do
- [x] Configure Transmission kill switch dynamically on container creation
- [ ] Add multi arch support once Docker Hub automated builds supports it properly

## Contribute
All contributions are appreciated

## License
MIT
