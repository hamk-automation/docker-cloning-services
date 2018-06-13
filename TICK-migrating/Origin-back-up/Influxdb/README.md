# Migrate existing influx-data

Copy influx-data directory to same level with this Dockerfile, then

Run this command
```
docker build -t influxdb .
```
to build an image of influxdb with existing data.

Create a container to mount influx-data into Docker a volume name influx_backup
```
docker run -dit -p 8086:8086 --name=influx-backup -v influx_backup:/var/lib/influxdb influxdb
```

## Volumerize and migrate to google drive storage
#### Authorizing
Run this command first time before any migration
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
#### Migrating
Run this command to start a volumerize container
```
docker run -d \
   --name volumerizeI \
   -v volumerize_cache:/volumerize-cache \
   -v volumerize_credentials:/credentials \
   -v influx_backup:/source:ro \
   -e "VOLUMERIZE_SOURCE=/source" \
   -e "VOLUMERIZE_TARGET=gdocs://pdathuynh@gmail.com/kontti-backup/migrate-$host_name/influxdb" \
   blacklabelops/volumerize
```
Start an initial full backup:
```
docker exec volumerizeI backupFull
```

## Clean up
```
docker stop volumerizeI
docker rm volumerizeI
docker image rm influxdb
```
