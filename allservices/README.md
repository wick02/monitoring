# Monitoring Demo

Get a monitoring station up and available. Seperated out into their own docker compose services that can be scaled up with the --scale feature. Allow anyone to have a monitoring setup locally and configured in a manner that an operations team can handle.

## Pre reqs:

* Docker

## How to run:

Clone the repo and follow these steps

0. Create networks for docker

```
docker network create allservices 
```

1. Start up the application

```
docker compose up --scale mimir=3 
```
