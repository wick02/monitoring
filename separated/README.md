# Monitoring Demo

Get a monitoring station up and available. Seperated out into their own docker compose services that can be scaled up with the --scale feature. Allow anyone to have a monitoring setup locally and configured in a manner that an operations team can handle.

## Pre reqs:

* Docker
* Docker compose 
* Promtail (Using brew on mac in this demo)

## How to run:

Clone the repo and follow these steps

0. Create networks for docker

Seperating each service so it's independent from another. There are other ways to handle the networking, this is done so killing a "docker compose up" doesn't kill the other services because they are attaching to the other networks 

```
docker network create loki
docker network create mimir
brew install promtail
```

1. Start Logs backend in a terminal

Not necessary to include the `scale` parts but wanted to show how to do so 

```
cd loki 
docker compose up --scale write=1 --scale read=1 
```

2. Start promtail stream in a terminal

Configured out of the box to grab prometheus logs due to the name of the container & creates a positions.yaml file.

```
cd promtail
promtail --config.file=promtail.yaml 
```

3. Start Metrics backend in a terminal

Necessary to have multiple replicas of each, so the scale is needed

```
cd mimir
docker compose up --scale ingester=3 --scale distributor=2 
```

4. Start Metrics feed prometheus in a terminal 

Only feeds prometheus metrics currently, will be adding in cadvisor & sample application that publishes a scrap point for it

```
cd prometheus
docker compose up 
```

5. Start Dashboarding/Query Frontend in a terminal

```
cd grafana
docker compose up
```

