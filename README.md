## MicroMDM (Docker)

Simple docker container for [MicroMDM][1].  MicroMDM is simple to run, as it is contained within a single binary, but I still thought there was a need to have a small docker container that had a bit of intelligence to it.

There are at least 2 folders you would want to map into the container (or use a data container for).  You can easily migrate from a dedicated instance to a dockerized version by pointing the container to your pre-existing folders.

For the `/certs` folder:
This folder will contain your own TLS cert and key file if you opt to use one.  The `TLS_CERT` and `TLS_KEY` environment variable should match the name of the file(s) that you places in the `/certs` folder.

# How to build your image
It's time to build our image. We're going to name the image `micromdm`. To do this, issue the command:
```
docker build -t micromdm .
```

# How to tag and push the image
Finally, we're going to tag our new image and then push it to Docker Hub. First tag the image with :latest using the command:
```
docker image tag micromdm vinayaroradocker/micromdm:latest
```

Now that the image is tagged, we can push it to Docker Hub with:
```
docker image push vinayaroradocker/micromdm:latest
```

You can see the logs of the running container by using `docker logs` for example:
```
docker logs micromdm
```

### Supported tags

- `latest`
  - Latest release
- `pre-release`
  - Latest pre-release
- `1.8.0`
- `1.7.1`
- `1.7.0-alpha`
- `1.6.0`
- `1.5.0`
- `1.4.0`
- `1.3.1`
- `1.3.0`
- `1.2.0`








### Example Usage

```
docker run -d --restart always --name micromdm \
  -e SERVER_URL=https://micromdm.acme.com \
  -e API_KEY=abcdef1234567890 \
  -e TLS_CERT=micromdm.acme.com.crt \
  -e TLS_KEY=micromdm.acme.com.key \
  -e TLS=true \
  -e COMMAND_WEBHOOK_URL=https://your-webhook-server-url \
  -v /root/certs:/certs \
  -v /root/micromdm:/config \
  -v /root/mdmrepo:/repo \
  -p 80:80 \
  -p 443:443 \
  vinayaroradocker/micromdm
```

### Environment Variables

Variable | Description
--- | ---
API_KEY | Define your API key (Optional)
DEBUG | Set to `true` to enable `-http_debug`
SERVER_URL | Public HTTPS url of your server
COMMAND_WEBHOOK_URL | URL to send command responses (Optional)
TLS | Set to `true` to enable HTTPS (Defaults to False)
TLS_CERT | TLS certificate file name (within mapped /certs directory)
TLS_KEY |TLS private key file name (within mapped /certs directory)
NO_COMMAND_HISTORY | disables saving of command history (Boolean)
USE_DYNAMIC_CHALLENGE | require dynamic SCEP challenges (Boolean)
GEN_DYNAMIC_CHALLENGE | generate dynamic SCEP challenges in enrollment profile (built-in only) (Boolean)


### Mapped Volumes

Path | Description
--- | ---
/certs | Folder containing TLS certificates (Optional)
/config | Folder containing micromdm configuration
/repo | Folder for http file repo

### Ports

80, 443, 8080

Ports 80/443 used if TLS enabled.  Otherwise serves on port 8080.

[1]: https://micromdm.io
