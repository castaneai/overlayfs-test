## Using overlayfs mount between two containers

```sh
mkdir -p merged
docker-compose up -d

docker-compose exec setup bash
mkdir -p /tmp/overlay
mount -t tmpfs tmpfs /tmp/overlay
mkdir -p /tmp/overlay/{upper,work}
mount -t overlay overlay -o lowerdir=/lower,upperdir=/tmp/overlay/upper,workdir=/tmp/overlay/work /merged
touch /merged/upper.txt
exit

docker-compose exec user bash
ls -l /merged/
# total 4
# -rw-rw-r-- 1 1000 1000 13 Mar  7 14:19 lower.txt
# -rw-r--r-- 1 root root  0 Mar  7 14:44 upper.txt
```