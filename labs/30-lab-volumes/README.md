# Lab 3 - Volumes

## Use volumes

### Tips

- Use `docker container inspect ...`

### Existing volumes

1. run `docker container run --name couchdb -d -p 5984:5984 couchdb:2.1`
2. Check existing volumes
docker volume ls
DRIVER    VOLUME NAME
local     1b63aa21ada41f7a52d4c33cdfb20ea7be8f321848c48943ec9d91a819f85d80

   1. Why there is already a volume ?
   couchDb créer un volume automatiquement

3. Identify the volume that is used by `couchdb`
        "Mounts": [
            {
                "Type": "volume",
                "Name": "1b63aa21ada41f7a52d4c33cdfb20ea7be8f321848c48943ec9d91a819f85d80",
                "Source": "/var/lib/docker/volumes/1b63aa21ada41f7a52d4c33cdfb20ea7be8f321848c48943ec9d91a819f85d80/_data",
                "Destination": "/opt/couchdb/data",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }

4. Mount the identified volume to busybox 
docker container run -it --rm --volumes-from=couchdb busybox

5. Check files inside `/opt/couchdb/data`
/ # ls
bin    dev    etc    home   lib    lib64  opt    proc   root   sys    tmp    usr    var
/ # ls opt/couchdb/data/
_dbs.couch         _nodes.couch       _replicator.couch  _users.couch
/ # 

6. Stop couchdb

docker container kill couchdb

7. Delete the volume
docker volume rm 1b63aa21ada41f7a52d4c33cdfb20ea7be8f321848c48943ec9d91a819f85d80d

8. Check that the volume has been deleted

docker volume ls
DRIVER    VOLUME NAME


### Create a named volume

1. Create a volume named : `couchdb_vol`
docker volume create --name couchdb_vol

2. Run `couchedb` with the created volume
docker container run -dt -v couchdb_vol couchdb:2.1 

3. Inspect the container and look at `Mounts` that `couchdb_vol` is used

        "Mounts": [
            {
                "Type": "volume",
                "Name": "c4883d4052ccdb4f5d3576a628ccfce957a8aa32480cb5187c11049f12a4b05d",
                "Source": "/var/lib/docker/volumes/c4883d4052ccdb4f5d3576a628ccfce957a8aa32480cb5187c11049f12a4b05d/_data",
                "Destination": "couchdb_vol",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            },


### Mount directory

1. Mount `/var/lib/docker` from host to `/dck` into a busybox container 
docker container run -dt -v ~/../../var/lib/docker:/dck busybox:latest 

2. Check at `ls /dck/volumes/couchdb_vol/_data` inside the container to verify that `couchdb_vol` is available

docker container exec -it festive_hofstadter sh
/ # ls
bin    dck    dev    etc    home   lib    lib64  proc   root   sys    tmp    usr    var
/ # ls dck/volumes/couchdb_vol/_data/
/ # ls dck/volumes/
5dbe7d4410bb83f54a093711aca74afb7605af614679461299ae1a6bee1d364c  
backingFsBlockDev                                                 
c4883d4052ccdb4f5d3576a628ccfce957a8aa32480cb5187c11049f12a4b05d  metadata.db
couchdb_vol
/ # 

### SideCar Pattern

1. Create a directory named `sidecar` with `mkdir`
2. Run Busybox
   1. Command: `sh -c 'while true; do date >> /dck/date.log; sleep 1; done'`
   2. Volume to mount: `$(pwd)/sidecar:/dck`
   3. Name: `gen_date`
   4. State: detached
   docker run -d --name gen_date -v "$(pwd)/sidecar:/dck" busybox sh -c 'while true; do date >> /dck/date.log; sleep 1; done'

3. Check the content of `sidecar/date.log` with `cat`
(base) tolo@tolo-Nitro-AN515-52:~/Dev/Cours/Docker/ESGI-4IABD-janvier-2025$ cat sidecar/date.log 
Thu Aug 28 08:24:40 UTC 2025
Thu Aug 28 08:24:41 UTC 2025
Thu Aug 28 08:24:42 UTC 2025
Thu Aug 28 08:24:43 UTC 2025
Thu Aug 28 08:24:45 UTC 2025
Thu Aug 28 08:24:46 UTC 2025
Thu Aug 28 08:24:47 UTC 2025
Thu Aug 28 08:24:48 UTC 2025
Thu Aug 28 08:24:49 UTC 2025
Thu Aug 28 08:24:50 UTC 2025
Thu Aug 28 08:24:51 UTC 2025
Thu Aug 28 08:24:52 UTC 2025
Thu Aug 28 08:24:53 UTC 2025
Thu Aug 28 08:24:54 UTC 2025
Thu Aug 28 08:24:55 UTC 2025
Thu Aug 28 08:24:56 UTC 2025
Thu Aug 28 08:24:57 UTC 2025
Thu Aug 28 08:24:58 UTC 2025
Thu Aug 28 08:24:59 UTC 2025
Thu Aug 28 08:25:00 UTC 2025
Thu Aug 28 08:25:01 UTC 2025
Thu Aug 28 08:25:02 UTC 2025
Thu Aug 28 08:25:03 UTC 2025
Thu Aug 28 08:25:04 UTC 2025
Thu Aug 28 08:25:05 UTC 2025
Thu Aug 28 08:25:06 UTC 2025
Thu Aug 28 08:25:07 UTC 2025
Thu Aug 28 08:25:08 UTC 2025
Thu Aug 28 08:25:09 UTC 2025
Thu Aug 28 08:25:10 UTC 2025
Thu Aug 28 08:25:11 UTC 2025
Thu Aug 28 08:25:12 UTC 2025
Thu Aug 28 08:25:13 UTC 2025
Thu Aug 28 08:25:14 UTC 2025
Thu Aug 28 08:25:15 UTC 2025
Thu Aug 28 08:25:16 UTC 2025
Thu Aug 28 08:25:17 UTC 2025
Thu Aug 28 08:25:18 UTC 2025
Thu Aug 28 08:25:19 UTC 2025
Thu Aug 28 08:25:20 UTC 2025
Thu Aug 28 08:25:21 UTC 2025
Thu Aug 28 08:25:22 UTC 2025
Thu Aug 28 08:25:23 UTC 2025
Thu Aug 28 08:25:24 UTC 2025
Thu Aug 28 08:25:25 UTC 2025
Thu Aug 28 08:25:26 UTC 2025

4. Run Busybox
   1. Command: `tail -f /dck2/date.log`
   2. Volume to mount: `$(pwd)/sidecar:/dck2`
   3. State: attached
docker run -it --rm -v "$(pwd)/sidecar:/dck2" busybox sh -c "tail -f /dck2/date.log"

5. Check content of `dck2/date.log` with `tail -f`
Thu Aug 28 08:26:32 UTC 2025
Thu Aug 28 08:26:33 UTC 2025
Thu Aug 28 08:26:34 UTC 2025
Thu Aug 28 08:26:35 UTC 2025
Thu Aug 28 08:26:36 UTC 2025

6. Exit container
7. Run `docker kill gen_date`
   1. Why is the container stoped ?
Comme le processus principal du conteneur est mort, Docker arrête le conteneur.

### In memory 

1. Run busybox with `--tmpfs /test`
2. Check with `mount | grep test` that tmpfs is used 

