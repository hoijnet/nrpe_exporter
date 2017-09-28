# NRPE exporter

The NRPE exporter allows for the exposing of nrpe server command's metrics, namely duration, status, and success.

## Building and running

### Local Build

    go build nrpe_exporter.go
    ./nrpe_exporter --nrpe-server-address=<nrpe-server-address>

Visiting [http://localhost:9275/export?command=check_load](http://localhost:9275/export?command=check_load)
will return metrics for the command 'check_load' agaisnt a locally running nrpe-server.

### Building with Docker

TODO

## Configuration

The nrpe_exporter requires little to no configuration.

The few options available such as logging level and the port to run on are configured via command line flags.

Run ./nrpe_exporter -h to view all available flags.

Note: The NRPE server you're connecting to must be configured with SSL disabled as this exporter does not support SSL.

## Prometheus Configuration

The NRPE exporter needs to be passed a nrpe server command as a parameter, this can be
done with relabelling.

Example config:
```yml
global:
  scrape_interval: 10s
scrape_configs:
  - job_name: nrpe
    metrics_path: /export
    params:
      command: [check_load]
    static_configs:
      - targets:
        - 127.0.0.1:9275
```