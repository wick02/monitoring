# Monitoring Demo

Get a monitoring station up and available. Seperated out into their own docker compose services that can be scaled up with the --scale feature

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

```
cd loki 
docker compose up --scale write=1 --scale read=1 # not necessary but I want everyone to know these are indended to scale
```

2. Start promtail stream in a terminal
```
cd promtail
./promtail --config.file=promtail.yaml # configured out of the box to grab two containers based on name, nginx & sampleapi
```

3. Start Metrics backend in a terminal
```
cd mimir
docker compose up --scale ingester=3 --scale distributor=2 # necessary to have multiple replicas of each
```

4. Start Metrics feed prometheus in a terminal 
```
cd prometheus
docker compose up # Only feeds prometheus metrics currently, will be adding in cadvisor & sample application that publishes a scrap point for it
```

5. Start Frontend in a terminal
```
cd grafana
docker compose up
```

## Services:

* Loki:       http://localhost:3100
* Promtail
* Mimir:      http://localhost:9009
* Prometheus: http://localhost:9090
* Grafana:    http://localhost:3000

## What do we have?

* Logs will be captured from the promtail config file to loki
* Metrics are captured through prometheus and sent to mimir
* Grafana has both data sources defined when brought up, you can start adjusting your prometheus config and promtail config to add in your other 
* All services are scalable because they are defined using gRPC communcation to talk with each other.
* There are load balancers in front of both backends so you can get to them on your browser
* When you want to deploy to a cloud service, you already have a built in scaling by component for both Mimir and Loki 

## Recommendation once everything is up

Checkout grafana explore at http://localhost:3100/explore
