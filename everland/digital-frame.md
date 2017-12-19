# Node-RED를 이용한 디지털 액자 만들기 실습

(단말 설정하기는 맨 아래로 이동)

## Telegram 봇 만들기

see https://bakyeono.net/post/2015-08-24-using-telegram-bot-api.html#botfather------

Telegram Bot 생성 후 HTTP API 사용을 위한 Token 값 메모가 필요합니다.

## 브라우저로 연결하기

브라우저에서 http://127.0.0.1:1880/ 주소를 엽니다.

## Flow 작성하기

### 텔레그램 메세지 받기 노드 작성하기

1. Telegram Receiver를 좌측 노드 팔레트에서 드래그해서 Flow 편집창으로 옮깁니다.
1. 노드를 더블클릭해서 Edit 화면으로 들어간 뒤 Bot 우측의 연필 버튼을 눌러 새로운 Bot을 추가합니다.
1. 위의 'Telegram 봇 만들기'를 참조하여 Bot을 생성하고 받은 Token을 입력합니다.
1. Download Directory를 /data/photo로 입력합니다.

### 사진첨부 확인 노드 작성하기

1. Function을 좌측 노드 팔레드에서 드래그하여 Flow 편집창으로 옮깁니다.
1. 노드를 더블클릭하고 아래 코드를 붙여넣습니다.

```
if (msg.payload.path) {
    return msg;
} else {
    return null;
}
```

### 사진 표시를 위한 명령어 구성
사진을 폰 화면 전체에 표시하기 위해서는 fbi라는 명령어를 사용합니다. 예를 들어 everland.jpg를 화면에 표시하기 위해서는

```
fbi -T 1 -a everland.jpg
```

1. Function을 좌측 노드 팔레드에서 드래그하여 Flow 편집창으로 옮깁니다.
1. 노드를 더블클릭하고 아래 코드를 붙여넣습니다.

```
msg.payload = '-T 1 -a ' + msg.payload.path;
return msg;
```

### 사진 출력하기

1. exec를 좌측 노드 팔레드에서 드래그하여 Flow 편집창으로 옮깁니다.
1. 노드를 더블클릭하여 Edit 화면으로 들어간 뒤 command 항목에 fbi를 입력합니다.

### 30초마다 동작하기

1. inject를 좌측 노드 팔레드에서 드래그하여 Flow 편집창으로 옮깁니다.
1. 노드를 더블클릭하여 Edit 화면으로 들어간 뒤 repeat 항목을 interval 30초로 변경합니다.

### 사진 목록 가져오기

1. fs dir ops를 좌측 노드 팔레드에서 드래그하여 Flow 편집창으로 옮깁니다.
1. 노드를 더블클릭하여 Edit 화면으로 들어간 뒤 path 항목에 /data/photo를 입력합니다.

### 임의사진 고르기

1. Function을 좌측 노드 팔레드에서 드래그하여 Flow 편집창으로 옮깁니다.
1. 노드를 더블클릭하고 아래 코드를 붙여넣습니다.

```
var values = msg.files;
valueToUse = values[Math.floor(Math.random() * values.length)];
msg.payload = {path: '/data/photo/' + valueToUse};
return msg;
```

# 단말 설정하기

## WIFI 설정하기

```
adb pull /data/misc/wifi/wpa_supplicant.conf
adb push wpa_supplicant.conf /data/misc/wifi/
adb reboot
adb shell ip addr
```

## 날짜 맞추기

```
date -s 20171219.154000
```

## Node-RED 실행하기

```
docker rm -f $(docker ps -a -q)

docker run --privileged --net=host -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup -v /dev:/dev -v /system:/system -v /vendor:/vendor -v /sys:/sys -v /data:/data -v /vendor/lib:/vendor/lib -it sungpark/everland-node-red:0.1 /solution/run.sh

adb forward tcp:1880 tcp:1880

adb shell
mkdir -p /data/photo/
```


