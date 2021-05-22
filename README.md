## OpenTelemetry Playbook
This repo provides guideline for building and testing OpenTelemetry. 

## Build & Run

### Build OpenTelemetry Collector Contrib
1. Clone the [opentelemetry-collector-contrib](https://github.com/open-telemetry/opentelemetry-collector-contrib)
1. Get inside the main directory: `cd opentelemetry-collector-contrib`
1. To build the collector run: `make otelcontribcol`
1. Add a config file in the main repo: `test_config.yaml`
1. Run the collector with your config file: `bin/otelcontribcol_darwin_amd64 --config test_config.yaml`