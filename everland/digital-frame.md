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

adb shell
mkdir -p /data/photo/
```

## Telegram 봇 만들기

see https://bakyeono.net/post/2015-08-24-using-telegram-bot-api.html#botfather------

Telegram Bot 생성 후 HTTP API 사용을 위한 Token 값 메모가 필요합니다.

## 브라우저로 연결하기

브라우저에서 http://127.0.0.1:1880/ 주소를 엽니다.

##
```
[{"id":"ec650388.3ad018","type":"telegram receiver","z":"87bd13b9.d6a7b","name":"EverlandBot","bot":"7206db31.f08154","saveDataDir":"/data/photo","x":117.5,"y":66,"wires":[["a2c75b88.e84ad8"],[]]},{"id":"a2c75b88.e84ad8","type":"function","z":"87bd13b9.d6a7b","name":"사진첨부가 있나요?","func":"if (msg.payload.path) {\n    return msg;\n} else {\n    return null;\n}","outputs":1,"noerr":0,"x":326.5,"y":109,"wires":[["75b1bcfc.44eb1c"]]},{"id":"75b1bcfc.44eb1c","type":"template","z":"87bd13b9.d6a7b","name":"HTML 만들기","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<!DOCTYPE HTML>\n<html>\n<head>\n<style type=\"text/css\">\nbody {\n  width: 100%;\n  height: 100%;\n  margin: 0px;\n  padding: 0px;\n}\ndiv {\n  -webkit-transform: rotate(90deg);\n}\n</style>\n</head>\n<body bgcolor=\"#000000\">\n  <div>\n    <center>\n      <img src=\"{{{payload.path}}}\" alt=\"starting\" style=\"width: 100%;\" />\n    </center>\n  </div>\n</body>\n</html>","output":"str","x":609.5,"y":159,"wires":[["2d89dbf6.ff8714","1ebeb8f9.750837","9df6a55b.9ec57"]]},{"id":"2d89dbf6.ff8714","type":"file","z":"87bd13b9.d6a7b","name":"","filename":"/data/photo.html","appendNewline":true,"createDir":true,"overwriteFile":"true","x":829.5,"y":185,"wires":[]},{"id":"c0e67aca.f3c3e8","type":"exec","z":"87bd13b9.d6a7b","command":"/root/webloader","addpay":true,"append":"","useSpawn":"false","timer":"","oldrc":false,"name":"","x":1193.5,"y":309.5,"wires":[[],[],[]]},{"id":"ce9c796.6cf3e08","type":"function","z":"87bd13b9.d6a7b","name":"/data/photo.html","func":"msg.payload = '/data/photo.html';\nreturn msg;","outputs":1,"noerr":0,"x":995.5,"y":263,"wires":[["c0e67aca.f3c3e8"]]},{"id":"1ebeb8f9.750837","type":"delay","z":"87bd13b9.d6a7b","name":"","pauseType":"delay","timeout":"1","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"x":811.5,"y":217,"wires":[["ce9c796.6cf3e08"]]},{"id":"9df6a55b.9ec57","type":"debug","z":"87bd13b9.d6a7b","name":"","active":true,"console":"false","complete":"true","x":798.5,"y":106,"wires":[]},{"id":"aa3a2425.6df3c8","type":"inject","z":"87bd13b9.d6a7b","name":"30초마다","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":121,"y":362,"wires":[["4d4305df.32d7d4"]]},{"id":"4d4305df.32d7d4","type":"fs-ops-dir","z":"87bd13b9.d6a7b","name":"사진 목록 가져오기","path":"/data/photo","pathType":"str","filter":"*","filterType":"str","dir":"files","dirType":"msg","x":285,"y":304,"wires":[["a51368fe.282888"]]},{"id":"a51368fe.282888","type":"function","z":"87bd13b9.d6a7b","name":"임의 사진 고르기","func":"var values = msg.files;\nvalueToUse = values[Math.floor(Math.random() * values.length)];\nconsole.log(valueToUse);\n//msg.payload.path = '/data/photo/' + valueToUse;\nmsg.payload = {path: '/data/photo/' + valueToUse};\nreturn msg;","outputs":1,"noerr":0,"x":471,"y":249,"wires":[["75b1bcfc.44eb1c"]]},{"id":"7206db31.f08154","type":"telegram bot","z":"","botname":"EverlandBot","usernames":"","chatids":""}]
```

