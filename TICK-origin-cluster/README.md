# On origin cluster

On origin machine or cluster with existing TICK stack

###### Ports

Change ports in docker-compose-tick.yml or tear down existing TICK stack

###### Images version

Change each image tags into latest stable tags:
   - [Chronograf](https://hub.docker.com/r/library/chronograf/tags/)
   - [Kapacitor](https://hub.docker.com/r/library/kapacitor/tags/)
   - [Telegraf](https://hub.docker.com/r/library/telegraf/tags/)
   - [Influxdb](https://hub.docker.com/r/library/influxdb/tags/)

###### [Avoid data lost upon restart (for more than one manager)](https://gist.github.com/cdelaitre/85949d8b697359a319e30a678e23d8bd)


