# Deploy TICK stack with retrieved data on a new cluster

#### Retrieve data on new host
 - Authentication:
     Run this one time, ignore if done already
```
docker run -it --rm \
    -v volumerize_cache:/volumerize-cache \
    -v volumerize_credentials:/credentials \
    -e "VOLUMERIZE_SOURCE=/source" \
    -e "VOLUMERIZE_TARGET=gdocs://pdathuynh@gmail.com/kontti-backup/auth" \
    -e "GOOGLE_DRIVE_ID=xxxxxx-xxxxxxx.apps.googleusercontent.com" \
    -e "GOOGLE_DRIVE_SECRET=xxxxxxxxxx" \
    blacklabelops/volumerize backup
```
  
  - Retrieving data from Google drive
      - For influxdb (in case influx has multiple volumes, see [this](https://github.com/blacklabelops/volumerize))
```
docker run --rm \
    -v influx-data:/source \
    -v volumerize_credentials:/credentials \
    -e "VOLUMERIZE_SOURCE=/source" \
    -e "VOLUMERIZE_TARGET=gdocs://pdathuynh@gmail.com/kontti-backup/migrate-$origin_host_name/influxdb" \
    blacklabelops/volumerize restore
```

      - For kapacitor
```
docker run --rm \
    -v kapacitor-data:/source \
    -v volumerize_credentials:/credentials \
    -e "VOLUMERIZE_SOURCE=/source" \
    -e "VOLUMERIZE_TARGET=gdocs://pdathuynh@gmail.com/kontti-backup/migrate-$origin_host_name/kapacitor" \
    blacklabelops/volumerize restore
```

      - For chronograf
```
docker run --rm \
    -v chronograf-data:/source \
    -v volumerize_credentials:/credentials \
    -e "VOLUMERIZE_SOURCE=/source" \
    -e "VOLUMERIZE_TARGET=gdocs://pdathuynh@gmail.com/kontti-backup/migrate-$origin_host_name/chronograf" \
    blacklabelops/volumerize restore
```

#### Prepairing config files
Create this directory for additional TICK scripts: /home/kapacitor
Prepair config files from host machine into these directories:
 - etc/telegraf/
 - etc/influxdb/
 - etc/kapacitor/

and compare them with these:
   - [kapacitor.conf](https://github.com/influxdata/sandbox/blob/master/kapacitor/config/kapacitor.conf)
   - [influxdb.conf](https://github.com/influxdata/sandbox/blob/master/influxdb/config/influxdb.conf)
   - [telegraf.conf](https://github.com/influxdata/sandbox/blob/master/telegraf/telegraf.conf)

#### Deploy TICK stack under Docker
 - Initialize docker swarm mode
```
docker swarm init
```
 - Deploy the stack
```
docker stack deploy -c docker-compose-tick.yml tick
```
 - Connect Chronograf to Influxdb and Kapacitor at <NODE_LAN_IP>:8888


