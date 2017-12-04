# Check adb environment (notebook <--usb--> device)
* adb devices
* adb wait-for-device
* adb shell
* adb shell setprop persist.sys.usb.config adb

# Set date
* date -s 20171010.165000

# Change wifi ssid and password
* adb pull /data/misc/wifi/wpa_supplicant.conf
* adb push wpa_supplicant.conf /data/misc/wifi/

* adb reboot
* adb shell ip addr

# Run docker image
* docker run --privileged --net=host -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup -v /dev:/dev -v /system:/system -v /vendor:/vendor -v /sys:/sys -v /data:/data -v /vendor/lib:/vendor/lib -it sungpark/sdc-galup-workshop:0.4 /solution/run.sh
* docker run --privileged --net=host -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup -v /dev:/dev -v /system:/system -v /vendor:/vendor -v /sys:/sys -v /data:/data -v /vendor/lib:/vendor/lib -it sungpark/sdc-galup-workshop:0.5 /solution/run.sh

# Open Node-RED webpage
* Open browser with address http://xx.xx.xx.xx:1880/
