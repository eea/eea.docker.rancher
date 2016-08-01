# EEA Rancher server

This repo includes the docker-compose orchestration for deployment of a [Rancher server](https://github.com/rancher/rancher/) in [a single node setup](http://docs.rancher.com/rancher/installing-rancher/installing-server/).

It uses an external mysql service with named volumes.

## How to install it

You must have Docker and Docker compose installed on your host.

Before starting the server you must add the secret key file named "server-eea.key" under the bind-mounted directory ./ngnix/tls. This adds SSL termination in nginx in front of Rancher [as descirbed in docs](http://docs.rancher.com/rancher/installing-rancher/installing-server/basic-ssl-config/).

```
# git clone https://github.com/eea/eea.docker.rancher
# cd eea.docker.rancher
```

Configure the secrets

```
# cp dbsecrets.env-dist dbsecrets.env
# vi dbsecrets.env
# cp mysqlsecrets.env-dist mysqlsecrets.env
# vi mysqlsecrets.env
```

Start the mysql server and setup the rancher cattle DB as described in [rancher with external database](http://docs.rancher.com/rancher/latest/en/installing-rancher/installing-server/#using-an-external-database). Close the mysql service.

Than finally run the full stack:

```
# docker-compose up -d
```

Go to http://yourhost/ to view the Rancher server UI.

## Where is the data?

Rancher uses mysql to store all Rancher metadata and settings. In the docker-compose file you can see that we store the data in a named volume.

We also have a mysql-backup service which will automatically do mysql dumps at certain intervals specified via environment variables. See the [original docker image deitch/mysql-backup/](https://hub.docker.com/r/deitch/mysql-backup/) for more info. The dumps are stored under ./mysql-backup.

## Upgrades

For supsequent upgrades you just need to pull the new image and bump up the version in the docker-compose.yml file and restart, in the example below we use v1.1.0.

```
# docker pull rancher/server:v1.1.0
# vi docker-compose.yml # bump-up the version and save
# docker-compose up -d
```

See the [rancher official upgrade documentation](http://docs.rancher.com/rancher/upgrading/).

## Troubleshooting

See more on [official page](http://docs.rancher.com/rancher/faqs/troubleshooting/)
