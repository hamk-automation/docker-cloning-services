# Migrate existing chronograf-data

Copy chronograf-data directory to same level with this Dockerfile, then

Run this command
```
docker build -t chronograf .
```
to build an image of chronograf with existing data.

Create a container to mount chronograf-data into Docker a volume name chronograf_backup
```
docker run -dit -p 9092:9092 --name=chronograf-backup -v chronograf_backup:/var/lib/chronograf chronograf
```

#### Volumerize and migrate to google drive storage
##### Authorizing
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
##### Migrating
Run this command to start a volumerize container (in case influx has multiple volumes, see [this](https://github.com/blacklabelops/volumerize))
```
docker run -d \
   --name volumerizeC \
   -v volumerize_cache:/volumerize-cache \
   -v volumerize_credentials:/credentials \
   -v chronograf_backup:/source:ro \
   -e "VOLUMERIZE_SOURCE=/source" \
   -e "VOLUMERIZE_TARGET=gdocs://pdathuynh@gmail.com/kontti-backup/migrate-$host_name/chronograf" \
   blacklabelops/volumerize
```
Start an initial full backup:
```
docker exec volumerizeC backupFull
```

Clean up
```
docker stop volumerizeC
docker rm volumerizeC
docker image rm chronograf
```
