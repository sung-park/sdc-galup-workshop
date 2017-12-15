# 타블렛을 이용한 디지털 액자 만들기 실습

## WIFI 설정하기
```
adb pull /data/misc/wifi/wpa_supplicant.conf
adb push wpa_supplicant.conf /data/misc/wifi/
adb reboot
adb shell ip addr
```

## Node-RED 실행하기
```
docker run --privileged --net=host -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup -v /dev:/dev -v /system:/system -v /vendor:/vendor -v /sys:/sys -v /data:/data -v /vendor/lib:/vendor/lib -it sungpark/sdc-galup-workshop:0.8 /solution/run.sh
adb forward tcp:1880 tcp:1880
```

## Telegram 봇 만들기
see https://bakyeono.net/post/2015-08-24-using-telegram-bot-api.html#botfather------

Telegram Bot 생성 후 HTTP API 사용을 위한 Token 값 메모가 필요합니다.






