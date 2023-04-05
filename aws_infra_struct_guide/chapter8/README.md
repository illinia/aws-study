### 모니터링의 목적과 영역
* 인프라에 대한 모니터링 : 애플리케이션이 실행되고 있는 인프라에 장애가 발생하거나 징후가 있지는 않은지 파악한다.
* 클라이언트의 요청에 대한 모니터링 : 클라이언트에서 우리가 의도한 대로 올바른 요청을 보내고 있는지, 공격 시도가 들어오지는 않는지, 얼마만큼의 요청량을 보내고 있는지 파악
* 애플리케이션에 대한 모니터링 : 작성하고 배포한 코드가 예상했던 대로 동작하고 있는지, 애플리케이션의 어떤 부분이 병목이 되어 성능 저하를 일으키는지 파악
* 데이터에 대한 모니터링 : 의도한 대로 데이터가 올바른 형태로 쌓이고 있는지, 서비스가 운영되면서 쌓이는 데이터들이 어떤 속도로 쌓이고 있는지 파악

### CloudWatch 에 사용자 지정 지표 기록
```shell
cd /var/www

mkdir cloudwatch-custom

cd cloudwatch-custom/

# test_data.json 파일을 해당 디렉토리에 저장
aws cloudwatch put-metric-data --namespace "Exercise People" --metric-data file://test_data.json
```

### CloudWatch Agent 로 메모리, 디스크 사용량 지표, 로그 기록
```shell
sudo yum install amazon-cloudwatch-agent -y

sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard

sudo amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s
```