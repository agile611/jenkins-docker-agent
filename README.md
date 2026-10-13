# Jenkins SSH build agents

This repository contains Docker images for Jenkins build agents that accept SSH connections. Each image starts `sshd` on port 22; configure the Jenkins SSH build-agent connection separately.

## Images

| Dockerfile | Contents | Suggested Docker Hub tag |
| --- | --- | --- |
| `DockerfileAgentAlpine324` | Alpine 3.24, OpenJDK 25, Git, Bash, Maven 3.9.16, OpenSSH | `alpine324` |
| `DockerfileAgentAnsible` | Debian 13, OpenJDK 25, Git, Bash, OpenSSH, Python 3, Ansible, sudo | `ansible` |
| `DockerfileAgentDebian13` | Debian 13, OpenJDK 25, Git, Bash, OpenSSH | `debian13` |
| `DockerfileAgentLight` | Alpine 3.24, OpenJDK 25, Git, Bash, OpenSSH | `light` |
| `DockerfileAgentTerraform` | Debian 13, OpenJDK 25, Git, Bash, OpenSSH, Terraform | `terraform` |
| `DockerfileAgentUbuntu2604` | Ubuntu 26.04, OpenJDK 25, Git, Bash, OpenSSH, Maven 3.9.16 | `ubuntu2604` |

`DockerfileAlpine37` is a legacy image based on Alpine 3.7 and is not included in the current build and publish commands below.

## Build an image

Run commands from the repository root. For example, build the Debian 13 agent locally:

```sh
docker build -f DockerfileAgentDebian13 -t jenkins-agent:debian13 .
```

Change the Dockerfile and tag to build another variant.

## Build and publish all current agents

Create a `jenkins-agent` repository under your Docker Hub account, then log in. Use a Docker Hub access token when prompted for a password.

```sh
export DOCKERHUB_USERNAME=your-docker-hub-username
docker login --username "$DOCKERHUB_USERNAME"

images=(
  "DockerfileAgentAlpine324:alpine324"
  "DockerfileAgentAnsible:ansible"
  "DockerfileAgentDebian13:debian13"
  "DockerfileAgentLight:light"
  "DockerfileAgentTerraform:terraform"
  "DockerfileAgentUbuntu2604:ubuntu2604"
)

for image in "${images[@]}"; do
  dockerfile="${image%%:*}"
  tag="${image#*:}"
  image_name="${DOCKERHUB_USERNAME}/jenkins-agent:${tag}"

  docker build --pull -f "$dockerfile" -t "$image_name" . || exit 1
  docker push "$image_name" || exit 1
done
```

Published images will be available as `your-docker-hub-username/jenkins-agent:<tag>`, for example `your-docker-hub-username/jenkins-agent:debian13`.

## Security note

The current Dockerfiles set the `jenkins` user's password to `jenkins`. Change that credential before using an image outside a trusted environment, and restrict access to SSH port 22.

## License

This project is licensed under the GNU General Public License v3.0. See [LICENSE](./LICENSE) for details.