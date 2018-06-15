# Backing up data on new cluster

Automatic backing up using Docker volume is not recommended. This should be done manually if possible since automatic backing up require tearing down or updating existing stack automatically, which is not yet supported by Docker.

## Example

```
docker run -d \
    --name volumerize \
    -v influx-test-volume:/source/influx-data:ro \
    -v backup_volume:/backup \
    -v cache_volume:/volumerize-cache \
    -e "VOLUMERIZE_JOBBER_TIME=0 0 3 * * *" \
    -e "TZ=Europe/Finland" \
    -e "VOLUMERIZE_FULL_IF_OLDER_THAN=7D" \
    -e "VOLUMERIZE_SOURCE=/source" \
    -e "VOLUMERIZE_TARGET=file:///backup" \
    blacklabelops/volumerize
```
Remove or edit flag parameters for desired output.

### Backingup multiple volumes

If there are two or more volumes to be back up, simply add another -v flag, but the target directories should all be under one "source" directory.
```
   -v influx-volume-1:/source/influx-1:ro \
   -v influx-volume-2:/source/influx-2:ro \
   ...
```

### Periodic backups

>The default cron setting for this container is: 0 0 4 * * *. That's four a clock in the morning UTC. You can set your own schedule with the environment variable VOLUMERIZE_JOBBER_TIME.
> 
>You can set the time zone with the environment variable TZ.
> 
>The syntax is different from cron because I use Jobber as a cron tool: [Jobber Time Strings](https://dshearer.github.io/jobber/doc/v1.1/#/time-strings).

### Enforcing Full Backups Periodically

>The default behavior is that the initial backup is a full backup. Afterwards, Volumerize will perform incremental backups. You can enforce another full backup periodically by specifying the environment variable VOLUMERIZE_FULL_IF_OLDER_THAN.
> 
>The format is a number followed by one of the characters s, m, h, D, W, M, or Y. >(indicating seconds, minutes, hours, days, weeks, months, or years)
> 
>Examples:
> 
>  - After three Days: 3D
>  - After one month: 1M
>  - After 55 minutes: 55m



