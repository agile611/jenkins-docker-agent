# Agile611 Jenkins SSH build agents

This repository contains Docker images for Jenkins agents that connect over SSH. Each image runs `sshd` on port 22 and creates a `jenkins` user. 

The Jenkins SSH build-agent connection is configured separately in Jenkins; this repository only provides the runtime environment.

> [!NOTE]
> Before using these images in Jenkins, make sure the following is already configured:
> - the Docker plugin is installed and enabled in Jenkins
> - the SSH Agent plugin is installed and enabled in Jenkins
> - a Docker Cloud is already configured in Jenkins, since these agent images are meant to be launched from that cloud

The image set is focused on Java-based build workloads, with additional variants for common tools such as Maven, Ansible, Terraform, and lightweight Alpine base images.

## Available images

| Dockerfile | Base image | Included tools | Suggested Docker Hub tag |
| --- | --- | --- | --- |
| `DockerfileAgentAlpine320Java21` | Alpine 3.20 | OpenJDK 21, Git, Bash, OpenSSH | `alpine320-java21` |
| `DockerfileAgentAlpine324Java25` | Alpine 3.24.2 | OpenJDK 25, Git, Bash, OpenSSH | `alpine324-java25` |
| `DockerfileAgentAlpine324Java25Maven3916` | Alpine 3.24.2 | OpenJDK 25, Git, Bash, OpenSSH, Maven 3.9.16 | `alpine324-java25-maven3916` |
| `DockerfileAgentAlpine324Java25NodeJS` | Alpine 3.24.2 | OpenJDK 25, Git, Bash, OpenSSH, Node.js, npm | `alpine324-java25-nodejs` |
| `DockerfileAgentAnsible` | Debian 13 | OpenJDK 25, Git, Bash, OpenSSH, Python 3, Ansible, sudo | `ansible` |
| `DockerfileAgentDebian13Java25` | Debian 13 | OpenJDK 25, Git, Bash, OpenSSH | `debian13-java25` |
| `DockerfileAgentTerraform` | Debian 13 | OpenJDK 25, Git, Bash, OpenSSH, Terraform | `terraform` |
| `DockerfileAgentUbuntu2604Java25Mvn3916` | Ubuntu 26.04 | OpenJDK 25, Git, Bash, OpenSSH, Maven 3.9.16 | `ubuntu2604-java25-maven3916` |

Each image is designed to be connected to Jenkins as an SSH build agent. 

They all expose port 22 and start the SSH daemon by default.

## Build a local image

Run the build from the repository root. For example, to build the Debian 13 Java 25 agent locally:

```sh
docker build -f DockerfileAgentDebian13Java25 -t jenkins-agent:debian13-java25 .
```

Use the Dockerfile that matches your workload, and change the tag to match your preferred naming convention.

## Build and publish all current agents

Create a `jenkins-agent` repository under your Docker Hub account, then log in. Use a Docker Hub access token when prompted for a password.

```sh
export DOCKERHUB_USERNAME=your-docker-hub-username
docker login --username "$DOCKERHUB_USERNAME"

images=(
  "DockerfileAgentAlpine320Java21:alpine320-java21"
  "DockerfileAgentAlpine324Java25:alpine324-java25"
  "DockerfileAgentAlpine324Java25Maven3916:alpine324-java25-maven3916"
  "DockerfileAgentAlpine324Java25NodeJS:alpine324-java25-nodejs"
  "DockerfileAgentAnsible:ansible"
  "DockerfileAgentDebian13Java25:debian13-java25"
  "DockerfileAgentTerraform:terraform"
  "DockerfileAgentUbuntu2604Java25Mvn3916:ubuntu2604-java25-maven3916"
)

for image in "${images[@]}"; do
  dockerfile="${image%%:*}"
  tag="${image#*:}"
  image_name="${DOCKERHUB_USERNAME}/jenkins-agent:${tag}"

  docker build --pull -f "$dockerfile" -t "$image_name" . || exit 1
  docker push "$image_name" || exit 1
done
```

Published images will be available as `your-docker-hub-username/jenkins-agent:<tag>`, for example `your-docker-hub-username/jenkins-agent:debian13-java25`.

## Security note

The Dockerfiles set the default password for the `jenkins` user to `jenkins`. This is fine for a trusted environment and local testing, but before using an image outside a private network, change the password and restrict access to SSH port 22.

## License and Support

[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC_BY--NC_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/) Published by [Agile611](http://www.agile611.com/) under the **Creative Commons Attribution-NonCommercial 4.0 International** license.

README written by [Guillem Hernández Sola](https://www.linkedin.com/in/guillemhs/).

## Contact

- 🌐 [agile611.com](http://www.agile611.com/)
- 📍 Carrer Laureà Miró 309, 08950 Esplugues de Llobregat (Barcelona)
