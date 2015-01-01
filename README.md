![logo](http://allthingsd.com/files/2013/10/tor-logo.png)

# Tor and Privoxy

Tor and Privoxy (web proxy configured to route through tor) docker container

# What is Tor?

Tor is free software and an open network that helps you defend against traffic
analysis, a form of network surveillance that threatens personal freedom and
privacy, confidential business activities and relationships, and state security.

# What is Privoxy?

Privoxy is a non-caching web proxy with advanced filtering capabilities for
enhancing privacy, modifying web page data and HTTP headers, controlling access,
and removing ads and other obnoxious Internet junk.

---

# How to use this image

*NOTE*: this image is setup by default to be a relay only (not an exit node)

## Exposing the port

    sudo docker run --rm -p 1080:1080 -p 8118:8118 -d dperson/torproxy

*NOTE*: it will take a while for tor to bootstrap...

Then you can hit tor directly at `http://hostname:1080` or privoxy web proxy at
`http://host-ip:8080` with your browser.


## Complex configuration

    sudo docker run -it --rm dperson/torproxy -h
    Usage: torproxy.sh [-opt] [command]
    Options (fields in '[]' are optional, '<>' are required):
        -h          This help
        -t ""       Configure timezone
                    possible arg: "[timezone]" - zoneinfo timezone for container

        The 'command' (if provided and valid) will be run instead of torproxy

ENVIROMENT VARIABLES (only available with `docker run`)

 * `TIMEZONE` - As above, set a zoneinfo timezone, IE `EST5EDT`

## Examples

Any of the commands can be run at creation with `docker run` or later with
`docker exec torproxy.sh` (as of version 1.3 of docker).

### Start torproxy with a specified zoneinfo timezone:

    sudo docker run --rm -p 1080:1080 -p 8118:8118 -d dperson/torproxy -t EST5EDT

OR

    sudo docker run --rm -p 1080:1080 -p 8118:8118 -e TIMEZONE=EST5EDT -d \
                dperson/torproxy

## Test the proxy:

    curl -x http://<ipv4_address>:8118 http://jsonip.com/

---

If you wish to adapt the default configuration, use something like the following
to copy it from a running container:

    sudo docker cp torproxy:/etc/tor/torrc /some/torrc

Then mount it to a new container like:

    sudo docker run --rm -p 1080:1080 -p 8118:8118 \
                -v /some/torrc:/etc/tor/torrc:ro -d dperson/torproxy

# User Feedback

## Issues

If you have any problems with or questions about this image, please contact me
through a [GitHub issue](https://github.com/dperson/torproxy/issues).