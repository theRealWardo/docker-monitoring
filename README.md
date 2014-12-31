### Docker Monitoring Playground

This is my collection of random attempts at monitoring Docker containers using
various technologies. If you want to check out these stacks, you'll need to
have installed [Fig](http://www.fig.sh/).

##### cAdvisor -> InfluxDB -> Grafana

`cadvisor-influxdb-grafana` contains a `fig.yml` file that brings up 3 containers.

* *[cAdvisor](https://github.com/google/cadvisor)* for gathering host and container metrics
* *[InfluxDB](https://github.com/influxdb/influxdb)* - for storing whatever
cAdvisor reports
* *[Grafana](https://github.com/grafana/grafana)* for looking at what is in InfluxDB

A simple `fig up -d` in that folder should bring some containers up.

You can checkout what cAdvisor is seeing by going to `yourhost:8080`. Then look
at what InfluxDB is collecting by going to `yourhost:8083` and log in with the
user name `root` and password `root`. Finally, to try graphing in Grafana you'll
want to run `fig logs grafana` which will print the password to `yourhost:8088`.
Note that Grafana dashboards will not persist.
