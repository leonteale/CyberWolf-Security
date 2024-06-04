---
description: >-
  Caldera™ is an adversary emulation platform designed to easily run autonomous
  breach-and-attack simulation exercises. Caldera is built on the MITRE ATT&CK™
  framework and is an active research project
---

# Mitre Caldera

{% embed url="https://caldera.readthedocs.io/en/latest/Installing-Caldera.html" %}

### Docker Deployment[](https://caldera.readthedocs.io/en/latest/Installing-Caldera.html#docker-deployment)

Caldera can be installed and run in a Docker container.

Start by cloning the Caldera repository recursively, passing the desired version/release in x.x.x format:

```bash
git clone https://github.com/mitre/caldera.git --recursive --branch 5.0.0
```

Next, build the docker image, changing the image tag as desired.

```bash
cd caldera
docker build --build-arg WIN_BUILD=true . -t caldera:server
```

Alternatively, you can use the `docker-compose.yml` file by running:

```bash
docker-compose build
```

Finally, run the docker Caldera server, changing port forwarding as required. More information on Caldera’s configuration is [available here](https://caldera.readthedocs.io/en/latest/Server-Configuration.html#configuration-file).

```bash
docker run -p 7010:7010 -p 7011:7011/udp -p 7012:7012 -p 8888:8888 caldera:server
```

To gracefully terminate your docker container, do the following:

```bash
# Find the container ID for your docker container running Caldera
docker ps

# Send interrupt signal, e.g. "docker kill --signal=SIGINT 5b9220dd9c0f"
docker kill --signal=SIGINT [container ID]
```
