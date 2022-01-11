# biorob_group12_bomi

Meta-repository containing both the python scripts (used for 'target reaching', 'mouse control' and 'unity mechanism control') and the nodejs server used for interfacing the scripts with Unity3D.

## Running locally

## Running using docker and docker compose

You will need to install `docker-compose` if it's not present on your system already (see [install docker-compose](https://docs.docker.com/compose/install))

Once you have `docker-compose` installed you will just have to run the compose file present in this repo with

```bash
.../biorob_group12_bomi$ docker-compose up
```

This will launch the dockerfiles