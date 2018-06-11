# On origin cluster

On origin machine or cluster with existing TICK stack

###### Ports and mounting points

Change ports in docker-compose-tick.yml or tear down existing TICK stack.

Select desired mounting points to share data with existing stack.

###### Images version

Change each image tags into latest stable tags:
   - [Chronograf](https://hub.docker.com/r/library/chronograf/tags/)
   - [Kapacitor](https://hub.docker.com/r/library/kapacitor/tags/)
   - [Telegraf](https://hub.docker.com/r/library/telegraf/tags/)
   - [Influxdb](https://hub.docker.com/r/library/influxdb/tags/)

###### [Avoid data lost upon restart (for more than one manager)](https://gist.github.com/cdelaitre/85949d8b697359a319e30a678e23d8bd)

###### Config files

Before deploying, check config files:
   - [kapacitor.conf](https://github.com/influxdata/sandbox/blob/master/kapacitor/config/kapacitor.conf)
   - [influxdb.conf](https://github.com/influxdata/sandbox/blob/master/influxdb/config/influxdb.conf)
   - [telegraf.conf](https://github.com/influxdata/sandbox/blob/master/telegraf/telegraf.conf)
