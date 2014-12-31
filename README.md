# Docker Monitoring Playground

This is my collection of random attempts at monitoring Docker containers using various open
source technologies. Neither closed source nor paid services (like [DataDog](https://www.datadoghq.com))
will be discussed/covered in this playground. If there is a piece of software you think might be
super awesome at monitoring Docker containers please
[file an issue](https://github.com/theRealWardo/docker-monitoring/issues), +1 an existing issue
or send a PR!

If you want to check out these stacks, you'll need to have installed [Fig](http://www.fig.sh/).

### cAdvisor &#65515; InfluxDB &#65513; Grafana

`cadvisor-influxdb-grafana` contains a `fig.yml` file that brings up 3 containers.

* [cAdvisor](https://github.com/google/cadvisor) - for gathering host and container metrics
* [InfluxDB](https://github.com/influxdb/influxdb) - for storing whatever cAdvisor reports
* [Grafana](https://github.com/grafana/grafana) - for looking at what is in InfluxDB

A simple `fig up -d` in that folder should bring some containers up.

You can checkout what cAdvisor is seeing by going to `yourhost:8080`. Then look
at what InfluxDB is collecting by going to `yourhost:8083` and log in with the
user name `root` and password `root`. Finally, to try graphing in Grafana you'll
want to run `fig logs grafana` which will print the password to `yourhost:8088`.
Note that Grafana dashboards will not persist.
