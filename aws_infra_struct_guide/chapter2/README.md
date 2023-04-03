### aws ec2 인스턴스 생성, ssh 연결
1. 리전 서울 선택
2. amazon linux 2 선택
3. 보안 그룹 ssh, http, https 모든 ipv4 접근 가능하게 설정
4. 키페어 설정, pem 파일 다운 저장
5. pem 파일 400 권한 부여
6. ssh -i "키페어파일위치" ec2-user@"퍼블릭 도메인 Or IP"

### 서버 환경 구성
1. sudo yum install git -y
2. git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.9.0
3. echo . $HOME/.asdf/asdf.sh >> ~/.bashrc
4. asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git
5. asdf install nodejs 16.14.0
6. asdf global nodejs 16.14.0
7. node -e "console.log('Running Node.js ' + process.version)"

### git 으로 ec2 인스턴스에 코드 배포하기
```shell
cd /var

# /var 디렉터리 소유 권한이 root 로 되어있기 때문에 파일이나 디렉터리를 추가하기 위해서는 root 권한이 필요하기 때문
sudo mkdir www
# 코드 관리는 일반 유저인 Ec2-user 로 처리할 것이므로 /var/www 경로를 Ec2-user 소유로 변경한다.
sudo chown ec2-user www

cd /var/www

git clone https://github.com/deopard/aws-exercise-a.git

cd aws-exercise-a/
```

### nginx, Phusion Passenger 설치 및 서비스
```shell
cd /var/www

# wget 명령어를 이용하여 웹에 있는 파일을 로컬로 내려받는다.
wget http://s3.amazonaws.com/phusion-passenger/releases/passenger-6.0.12.tar.gz

sudo mkdir /var/passenger

# passenger 파일은 ec2-user 로 처리할 것이기에 chown 명령어로 소유자를 변경한다.
sudo chown ec2-user /var/passenger/

# 압축 해제한다.
tar -xzvf passenger-6.0.12.tar.gz -C /var/passenger/

asdf plugin add ruby

sudo yum install gcc gcc-c++ glibc glibc-common gd gd-devel openssl-devel libcurl-devel -y

asdf install ruby 3.1.1

asdf global ruby 3.1.1

echo export PATH=/var/passenger/passenger-6.0.12/bin:$PATH >> ~/.bash_profile

source ~/.bash_profile

sudo dd if=/dev/zero of=/swap bs=1M count=1024
sudo mkswap /swap
sudo swapon /swap

/home/ec2-user/.asdf/installs/ruby/3.1.1/bin/ruby /home/ec2-user/.asdf/installs/ruby/3.1.1/bin/gem install rack

export ASDF_DATA_DIR=/home/ec2-user/.asdf

export ORIG_PATH="$PATH"
sudo -s -E
export PATH="$ORIG_PATH"
asdf global ruby 3.1.1

passenger-install-nginx-module

sudo vi /opt/nginx/conf/nginx.conf
# http 요소 첫 줄에 추가 server_names_hash_bucket_size 128;

sudo /opt/nginx/sbin/nginx
```

### nginx, Phusion Pasenger 서비스 명령어 추가
```shell
cd /etc/init.d

sudo vi nginx
# nginx 시작 스크립트(nginx_script.sh)의 내용을 복붙한다.
sudo chmod 755 nginx
```

### 시스템 시작 시 자동 시작 서비스에 등록
```shell
sudo chkconfig --add nginx

sudo ntsysv
```

### 하나의 서버에서 두 개의 애플리케이션 서비스하기
```shell
cd /var/www

git clone https://github.com/deopard/aws-exercise-b.git

cd aws-exercise-b/

npm install

sudo vi /opt/nginx/conf/nginx.conf

sudo service nginx restart
```