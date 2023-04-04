### Auto Scaling 을 통한 인스턴스 자동 추가, 제거
```shell
sudo amazon-linux-extras install epel -y

sudo yum install stress -y

stress --cpu 1 --timeout 600
```

### Auto Scaling 그룹과 로드 밸런서를 통한 장애 조치
```shell
cd /opt/nginx/logs


```