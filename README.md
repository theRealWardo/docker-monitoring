# Docker Monitoring Playground

This is my collection of random attempts at monitoring Docker containers using various open
source technologies. Neither closed source nor paid services (like [DataDog](https://www.datadoghq.com))
will be discussed/covered in this playground. If there is a piece of software you think might be
super awesome at monitoring Docker containers please
[file an issue](https://github.com/theRealWardo/docker-monitoring/issues), +1 an existing issue
or send a PR!

If you want to check out these stacks, you'll need to have installed [Fig](http://www.fig.sh/). To run any
of them, just go into the directory with `fig.yml` of the stack you want to try and run `fig up -d`.

### cAdvisor &#65515; InfluxDB &#65513; Grafana

`cadvisor-influxdb-grafana` contains a `fig.yml` file that brings up 3 containers.

* [cAdvisor](https://github.com/google/cadvisor) - for gathering host and container metrics
* [InfluxDB](https://github.com/influxdb/influxdb) - for storing whatever cAdvisor reports
* [Grafana](https://github.com/grafana/grafana) - for looking at what is in InfluxDB

You can checkout what cAdvisor is seeing by going to `yourhost:8080`. Then look
at what InfluxDB is collecting by going to `yourhost:8083` and log in with the
user name `root` and password `root`. Finally, to try graphing in Grafana you'll
want to run `fig logs grafana` which will print the password to `yourhost:8088`.
Note that Grafana dashboards will not persist.

### Dockerana

`dockerana` contains a `fig.yml` file that brings up 7 containers.

* [Nginx](https://github.com/dockerana/dockerana/tree/master/components/nginx) - as a frontend for Graphite.
* [Graphite](https://github.com/dockerana/dockerana/tree/master/components/graphite) - for graphs and stuff.
* [Carbon Cache](https://github.com/dockerana/dockerana/tree/master/components/carbon) - for storing time series and stuff.
* [StatsD](https://github.com/dockerana/dockerana/tree/master/components/statsd) - for collecting metrics and forwarding to Carbon.
* [Grafana](https://github.com/dockerana/dockerana/tree/master/components/grafana) - for graphing and building dashboards.
* [Elasticsearch](https://github.com/dockerana/dockerana/tree/master/components/elasticsearch) - for storing dashboards.
* [ingestsyslog](https://github.com/dockerana/dockerana/blob/master/Dockerfile) - a container to report host machine syslog events.

This is a pretty large stack with a lot going on, but basically it seems that each machine should run something like
`ingestsyslog` which is monitoring the host machine and reporting the number of running processes and more to StatsD.
I could only see loadavg and processes reported but the code seems to indicate more is supposed to be happening.
I didn't really dig in to try and get more to work (yet). Anyway, you can look at the data in Graphite at `yourhost:8080`
or go to the Grafana dashboard at `yourhost:8081`. You can add more containers that report data with
`docker run -d --link dockerana_statsd_1:statsd dockerana/dockerana` or similar.
