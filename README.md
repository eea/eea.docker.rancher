# EEA Rancher server

This repo includes the docker-compose orchestration for deployment of a [Rancher server](https://github.com/rancher/rancher/) in [a single node setup](http://docs.rancher.com/rancher/installing-rancher/installing-server/).

It expect a data-only-container to be present on the host with name rancher-data as described in the [rancher upgrade guide](http://docs.rancher.com/rancher/upgrading/).

## How to install it

You must have Docker and Docker compose installed on your host.

Before starting the server you must add the secret key file named "server-eea.key" under the bind-mounted directory ./ngnix/tls. This adds SSL termination in nginx in front of Rancher [as descirbed in docs](http://docs.rancher.com/rancher/installing-rancher/installing-server/basic-ssl-config/).

```
# git clone https://github.com/eea/eea.docker.rancher
# cd eea.docker.rancher
# docker-compose up -d
```

Go to http://yourhost/ to view the Rancher server UI.

## Where is the data?

Rancher uses mysql to store all Rancher metadata and settings. In the docker-compose file you can see that we use a data-only-container (busybox) to store the mysql data.

We also have a mysql-backup service which will automatically do mysql dumps at certain intervals specified via environment variables. See the [original docker image deitch/mysql-backup/](https://hub.docker.com/r/deitch/mysql-backup/) for more info. The dumps are stored under ./mysql-backup.

## Upgrades

First of all make sure on the first upgrade you have a data-only-container where the rancher mysql data is stored. See the [rancher official upgrade documentation](http://docs.rancher.com/rancher/upgrading/).

For supsequent upgrades you just need to pull the new image and bump up the version in the docker-compose.yml file and restart, in the example below we use v0.46.0.

```
# docker pull rancher/server:v0.46.0
# vi docker-compose.yml # bump-up the version and save
# docker-compose up -d
```

## Troubleshooting

### Where can I find the detailed logs of the rancher server container?

Running docker logs on the Rancher server container will provide a set of the basic logs. To get more detailed logs, you can exec into the rancher server container and look at the log files.

Exec into the server container
```
$ docker exec -it <server_container_id> bash
```

Navigate to the location of the cattle logs
```
$ cd /var/lib/cattle/logs/
$ cat cattle-debug.log
```
Inside this folder, there will be cattle-debug.log and cattle-error.log. If you have been using Rancher server for many days, you will find a log file for each day as we create a new file for each day.

See more on [official page](http://docs.rancher.com/rancher/faqs/troubleshooting/)
