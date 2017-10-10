# Setup date
date -s 20171010.165000

# Run docker image
docker run --privileged --net=host -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup -v /dev:/dev -v /system:/system -v /vendor:/vendor -v /sys:/sys -v /data:/data -v /vendor/lib:/vendor/lib -it sungpark/sdc-galup-workshop:0.3 /solution/run.sh
