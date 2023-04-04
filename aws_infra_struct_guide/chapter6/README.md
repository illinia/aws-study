### AWS IAM
* 권한 : aws 서비스나 자원에 어떤 작업을 할 수 있는지 명시해두는 규칙
* 정책 : 권한들의 모음
* 사용자 : aws 의 기능과 자원을 이용하는 객체. 사용자별로 권한을 세분화해서 지정할 수 있다.
* 그룹 : 여러 사용자에게 공통으로 권한을 부여할 수 있게 만들어진 개념
* 역할 : 객체에 여러 정책을 적용한다는 점에서 사용자와 비슷하지만 객체가 사용자가 아닌 서비스나 다른 aws 계정의 사용자라는 점에서 차이가 있다.
* 인스턴스 프로파일 : ec2 인스턴스를 구분하고 인스턴스에 권한을 주기 위한 개념


### AWS CodeDeploy
* CodeDeploy Agent : 자동 배포가 진행될 ec2 인스턴스에 설치되어 CodeDeploy 명령을 기다리고 있는 프로그램

### CodeDeploy 로 현재 위치 배포 진행하기
AMI 생성용 EC2 인스턴스 시작
```shell
cd /var/www

rm -rf aws-exercise-*

wget https://aws-codedeploy-ap-northeast-2.s3.amazonaws.com/latest/install

chmod +x ./install

sudo yum install ruby -y

sudo ./install auto

sudo service codedeploy-agent status

sudo shutdown -h now
```