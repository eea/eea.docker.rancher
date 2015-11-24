# EEA Rancher server

This repo includes the docker-compose orchestration for deployment of a [Rancher server](https://github.com/rancher/rancher/) in [a single node setup](http://docs.rancher.com/rancher/installing-rancher/installing-server/).

It expect a data-only-container to be present on the host with name rancher-data as described in the [rancher upgrade guide](http://docs.rancher.com/rancher/upgrading/).

## How to install it

You must have Docker and Docker compose installed on your host.

It expects a data-only-container to be present on the host with name rancher-data as described in the [rancher upgrade guide](http://docs.rancher.com/rancher/upgrading/).

```
# git clone https://github.com/eea/eea.docker.rancher
# cd eea.docker.rancher
# docker-compose up -d
```

Go to http://<yourhost>:8080/ to view the Rancher server UI.

## Upgrades

First of all make sure on the first upgrade you have a data-only-container where the rancher mysql data is stored. See the [rancher official upgrade documentation](http://docs.rancher.com/rancher/upgrading/).

For supsequent upgrades you just need to pull the new image and bump up the version in the docker-compose.yml file and restart, in the example below we use v0.46.0.

```
# docker pull rancher/server:v0.46.0
# vi docker-compose.yml # bump-up the version and save
# docker-compose up -d
```

## Troubleshooting

See [official page](http://docs.rancher.com/rancher/faqs/troubleshooting/)
