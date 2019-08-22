# Grafana XXL

[![DockerHub pulls](https://img.shields.io/docker/pulls/monitoringartist/grafana-xxl.svg?style=plastic&label=DockerHub%20Pulls)](https://img.shields.io/docker/pulls/monitoringartist/grafana-xxl.svg) [![GitHub stars](https://img.shields.io/github/stars/monitoringartist/grafana-xxl.svg?style=plastic&label=GitHub%20Stars)](https://github.com/monitoringartist/grafana-xxl) [![DockerHub stars](https://img.shields.io/docker/stars/monitoringartist/grafana-xxl.svg?style=plastic&label=DockerHub%20Stars)](https://img.shields.io/docker/pulls/monitoringartist/grafana-xxl.svg) [![Commercial support ready](https://img.shields.io/badge/Commercial%20support-ready-brightgreen.svg)](http://www.monitoringartist.com 'DevOps / Docker / Kubernetes / AWS ECS / Google GCP / Zabbix / Zenoss / Terraform / Monitoring')

Dockerized Grafana with all preinstalled plugins from https://grafana.net/plugins.

![Grafana XXL datasources and plugins](https://raw.githubusercontent.com/monitoringartist/grafana-xxl/master/doc/grafana-xxl-datasources-plugins.png)

Please donate to author, so he can continue to publish another awesome projects
for free:

[![Paypal donate button](http://jangaraj.com/img/github-donate-button02.png)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=8LB6J222WRUZ4)

# Running your Grafana XXL Docker image

Start your image binding the external port 3000:

    docker run -d --name=grafana-xxl -p 3000:3000 monitoringartist/grafana-xxl:latest

Try it out, default admin user is admin/admin.

## Grafana XXL with persistent storage (recommended)

    # create /var/lib/grafana as persistent volume storage
    docker run -d -v /var/lib/grafana --name grafana-xxl-storage busybox:latest

    # start grafana-xxl
    docker run \
      -d \
      -p 3000:3000 \
      --name grafana-xxl \
      --volumes-from grafana-xxl-storage \
      monitoringartist/grafana-xxl:latest

## Running specific version of Grafana XXL

    # specify right tag, e.g. 2.6,3.1,dev (latest nigthly build) - see Docker Hub for available tags
    docker run \
      -d \
      -p 3000:3000 \
      --name grafana-xxl \
      monitoringartist/grafana-xxl:dev

## Building Grafana XXL for ARM

You need ARM-version Grafana's deb (for example from [here](https://github.com/fg2it/grafana-on-raspberry/releases)).
Also you need ARM-version of gosu (from [here](https://github.com/tianon/gosu/releases))

    # armhf
    docker build \
      --tag grafana-xxl \
      --build-arg GRAFANA_DEB_URL=https://github.com/fg2it/grafana-on-raspberry/releases/download/v5.0.4/grafana_5.0.4_armhf.deb \
      --build-arg GOSU_BIN_URL=https://github.com/tianon/gosu/releases/download/1.10/gosu-armhf .

    # arm64
    docker build \
      --tag grafana-xxl \
      --build-arg GRAFANA_DEB_URL=https://github.com/fg2it/grafana-on-raspberry/releases/download/v5.0.4/grafana_5.0.4_arm64.deb \
      --build-arg GOSU_BIN_URL=https://github.com/tianon/gosu/releases/download/1.10/gosu-arm64 .

## Building Grafana XXL Enterprise version

Building the enterprise version of Grafana with its premium plugins requires you to be logged into the site before the download location is given in the API (which is grafana.com/api, not grafana.net/api, confusingly). So, the grafana.com cookie must be passed into the build process.

To begin, create a local environment variable with the grafana.com website cookie to keep the `docker build` command clean

    export GRAFANA_COM_COOKIE="connect.sid=s%3AkT0eX...1j0HrHIE; consent=%7B%22analytics%22%3Atrue%7D; driftt_aid=5e4e-...-7525; DFTT_END_USER_PREV_BOOTSTRAPPED=true; config=%7B%22org%22%3A%22whateverenterprise%22%2C%22displayType%22%3A%22list%22%7D; io=bS-kE...68"

Then, build the image with the `--build-arg` switch, including your cookie

    docker build --build-arg GRAFANA_COM_COOKIE=${GRAFANA_COM_COOKIE} .

This will iterate through all premium plugins listed in the `grafana.net/api/plugins` list and install them. *Note* there are still some premium plugins that do not provide a download url (like "grafana-timestream-datasource"), but they fail gracefully.

## Configuring your Grafana container

All options defined in conf/grafana.ini can be overriden using environment
variables by using the syntax GF_<SectionName>_<KeyName>. For example:

    docker run \
      -d \
      -p 3000:3000 \
      --name=grafana-xxl \
      -e "GF_SERVER_ROOT_URL=http://grafana.server.name" \
      -e "GF_SECURITY_ADMIN_PASSWORD=secret" \
      monitoringartist/grafana-xxl:latest

More information in the grafana configuration documentation: http://docs.grafana.org/installation/configuration/

## Configuring AWS credentials for CloudWatch support (only Grafana 3.+)

    docker run \
      -d \
      -p 3000:3000 \
      --name=grafana-xxl \
      -e "GF_AWS_PROFILES=default" \
      -e "GF_AWS_default_ACCESS_KEY_ID=YOUR_ACCESS_KEY" \
      -e "GF_AWS_default_SECRET_ACCESS_KEY=YOUR_SECRET_KEY" \
      -e "GF_AWS_default_REGION=eu-west-1" \
      monitoringartist/grafana-xxl:latest

You may also specify multiple profiles to `GF_AWS_PROFILES` (e.g.
`GF_AWS_PROFILES=default another`).

Supported variables:

- `GF_AWS_PROFILES`: list of AWS profiles for Cloudwatch datasource
- `GF_AWS_${profile}_ACCESS_KEY_ID`: AWS access key ID (required).
- `GF_AWS_${profile}_SECRET_ACCESS_KEY`: AWS secret access  key (required).
- `GF_AWS_${profile}_REGION`: AWS region (optional).

## Auto upgrade plugins

Container tries to upgrade all installed plugins in the container automatically before Grafana start. If you want to disable this behaviour, please use environment variable `-e UPGRADEALL=false`.

## Supported monitoring services

- [Hawkular](http://www.hawkular.org/docs/components/metrics/grafana_integration.html)
- [Raintank](http://raintank.io/docs/litmus/raintank-datasource/)

# Integrations

* [Puppet for dockerized grafana-xxl](https://github.com/monitoringartist/grafana-xxl/blob/master/puppet.md)
* [Ansible for dockerized grafana-xxl](https://github.com/monitoringartist/grafana-xxl/blob/master/ansible.md)
* [docker-compose for dockerized grafana-xxl](https://github.com/monitoringartist/grafana-xxl/blob/master/docker-compose.yml)

# Author

[Devops Monitoring Expert](http://www.jangaraj.com 'DevOps / Docker / Kubernetes / AWS ECS / Google GCP / Zabbix / Zenoss / Terraform / Monitoring'),
who loves monitoring systems and cutting/bleeding edge technologies: Docker,
Kubernetes, ECS, AWS, Google GCP, Terraform, Lambda, Zabbix, Grafana, Elasticsearch,
Kibana, Prometheus, Sysdig, ...

Summary:
* 1000+ [GitHub](https://github.com/monitoringartist/) stars
* 6000+ [Grafana dashboard](https://grafana.net/monitoringartist) downloads
* 800 000+ [Docker image](https://hub.docker.com/u/monitoringartist/) pulls

Professional devops / monitoring / consulting services:

[![Monitoring Artist](http://monitoringartist.com/img/github-monitoring-artist-logo.jpg)](http://www.monitoringartist.com 'DevOps / Docker / Kubernetes / AWS ECS / Google GCP / Zabbix / Zenoss / Terraform / Monitoring')
