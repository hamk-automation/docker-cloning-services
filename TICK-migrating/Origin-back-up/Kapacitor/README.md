# Migrate existing kapacitor-data

Copy kapacitor-data directory to same level with this Dockerfile, then

Run this command
```
docker build -t kapacitor .
```
to build an image of kapacitor with existing data.

Create a container to mount kapacitor-data into Docker a volume name kapacitor_backup
```
docker run -dit -p 9092:9092 --name=kapacitor-backup -v kapacitor_backup:/var/lib/kapacitor kapacitor
```

## Volumerize and migrate to google drive storage
#### Authorizing
Run this command one time before any migration, ignore if done already
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
Run this command to start a volumerize container (in case influx has multiple volumes, see [this](https://github.com/blacklabelops/volumerize))
```
docker run -d \
   --name volumerizeK \
   -v volumerize_cache:/volumerize-cache \
   -v volumerize_credentials:/credentials \
   -v kapacitor_backup:/source:ro \
   -e "VOLUMERIZE_SOURCE=/source" \
   -e "VOLUMERIZE_TARGET=gdocs://pdathuynh@gmail.com/kontti-backup/migrate-$host_name/kapacitor" \
   blacklabelops/volumerize
```
Start an initial full backup:
```
docker exec volumerizeK backupFull
```

## Clean up
```
docker stop volumerizeK
docker rm volumerizeK
docker image rm kapacitor
```
