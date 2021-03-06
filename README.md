## OpenTelemetry Playbook
This repo provides guideline for building and testing OpenTelemetry. 

## Build & Run

### Build and run from the OpenTelemetry Collector Contrib repo
1. Clone the [opentelemetry-collector-contrib](https://github.com/open-telemetry/opentelemetry-collector-contrib) repo.
1. Get inside the main directory: `cd opentelemetry-collector-contrib`
1. To build the collector run: `make otelcontribcol`
1. Add a config file in the main repo: `test_config.yaml`
1. Run the collector with your config file: `bin/otelcontribcol_darwin_amd64 --config test_config.yaml`
1. Before running unit test for a component, move inside the folder, e.g.: `cd processor/metricsgenerationprocessor/`
1. To run unit test: `go test -v ./... -cover -coverprofile mytest.out`
1. To see coverage of unit test: `go tool cover -html=mytest.out`

### Build docker image from the OpenTelemetry Collector Contrib repo
1. Clone the [opentelemetry-collector-contrib](https://github.com/open-telemetry/opentelemetry-collector-contrib) repo.
1. Get inside the main directory: `cd opentelemetry-collector-contrib`
1. To build the collector run: `make otelcontribcol`
1. To build the docker image run: `make docker-otelcontribcol`
1. Change the docker image tag using: `docker tag otelcontribcol:latest rhossai2/otel-contrib:v0.0.0` where `rhossai2` is your docker-hub user name.
1. Push the image to docker-hub running: `docker push rhossai2/otel-contrib:v0.0.0`

Command for otel-collector-contrib image:
```yaml
- command:
    - "/otelcontribcol"
    - "--config=/conf/adot-collector-config.yaml"
    - "--log-level=DEBUG"
```

### Build and run from the OpenTelemetry Collector repo
1. Clone the [opentelemetry-collector](https://github.com/open-telemetry/opentelemetry-collector) repo.
1. Get inside the main directory: `cd opentelemetry-collector`
1. To build the collector run: `make otelcol`
1. Add a config file in the main repo: `test_config.yaml`
1. Run the collector with your config file: `./bin/otelcol_darwin_amd64 —config test_config.yaml`
1. Before running unit test for a component, move inside the folder, e.g.: `cd processor/metricsgenerationprocessor/`
1. To run unit test: `go test -v ./... -cover -coverprofile mytest.out`
1. To see coverage of unit test: `go tool cover -html=mytest.out`

### Build docker image from ADOT Collector repo

Command for running ADOT Collector:
```yaml
- command:
          - "/awscollector"
          - "--config=/conf/adot-collector-config.yaml"
          - "--log-level=DEBUG"
```

### Add new component (receiver/processor) in the OpenTelemetry Collector Contrib repo

We need to make changes in two different files to enable a components (receiver/processor). 
1. Update the `go.mod` file.
1. Update default component file.