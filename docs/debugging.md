## Heapster Debugging FAQ:

This is a collection of common issues faced by users and ways to debug them.

Depending on the deployment setup, the issue could be either with heapster, cadvisor, kubernetes, or the monitoring backend.

### Heapster Core

Heapster exports a '/validate' endpoint that will provide some information about its current state.

If the '/validate' endpoint does not provide enough information, additional logging can be enabled by setting an extra flag. This requires restarting heapster though.
Add `--vmodule=*=4` flag to heapster. When using the docker image or when running in kubernetes, pass an extra environment variable `FLAGS="--vmodule=*=4`. 
If you are running heapster on kubernetes, the environment variable needs to be added to the `env` section in [heapster-controller.yaml](../deploy/kube-config/heapster-controller.yaml)

### InfluxDB & Grafana

Ensure Influxdb is up and reachable. Heapster attempts to create a database by default, which will fail eventually after a fixed number of retries.
If the Grafana queries are stuck or slow, it is due to InfluxDB being unresponsive. Consider providing InfluxDB more compute resources (CPU and Memory).
The default database on Influxdb is 'k8s'. 
A `list series` query on 'k8s' database should list all the series being pushed by heapster. If you do not see any series, take a look at heapster logs.