# Jenkins slave with Alpine Linux for Docker [![](https://images.microbadger.com/badges/image/thedrhax/jenkins-slave-alpine.svg)](https://hub.docker.com/r/thedrhax/jenkins-slave-alpine)

This image extends [alpine:edge](https://hub.docker.com/r/_/docker) with Jenkins Swarm module.

## Example

The command below will start a slave named "test" that will try to connect to Jenkins located at http://jenkins:8080/ using `jenkins` as login and password. You can change these settings by overriding variables listed below.

```
docker run -it --rm --name slave -e JENKINS_SLAVE_NAME="test" thedrhax/jenkins-slave-alpine
```

### Setting up master Jenkins

* Install the [Swarm Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Swarm+Plugin).
* Make sure that port 50000 of master will be accessible for this slave.
* [optional] Create separate account and allow it to create slaves or just use your account.

### Slave configuration variables

* `-e JENKINS_MASTER_USERNAME=jenkins` — username for logging into Jenkins
* `-e JENKINS_MASTER_PASSWORD=jenkins` — password for user specified above
* `-e JENKINS_MASTER_URL=http://jenkins:8080/` — URL of Jenkins

* `-e JENKINS_SLAVE_ROOT=/root/jenkins-slave`
* `-e JENKINS_SLAVE_MODE=exclusive` — `exclusive` or `normal` are allowed. [[more info]](https://wiki.jenkins-ci.org/display/JENKINS/Swarm+Plugin)
* `-e JENKINS_SLAVE_NAME=swarm-$RANDOM` — name of slave displayed in Jenkins
* `-e JENKINS_SLAVE_WORKERS=1` — number of simultaneously running tasks
* `-e JENKINS_SLAVE_LABELS` — slave labels which can be used in Jenkins

### Alpine configuration

* `-e REQUIRED_PACKAGES=""` — package list to be installed when container is started

## Projects based on this image

* [thedrhax/jenkins-slave-docker](https://github.com/thedrhax-dockerfiles/jenkins-slave-docker)

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](./LICENSE) file for details.
