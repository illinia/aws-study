### Secrets Manager 사용법
```shell
cd /var/www

git clone https://github.com/deopard/aws-exercise-a.git

cd aws-exercise-a/

git checkout secrets-manager

vi app.js
# accessKeyId, secretAccessKey 에 IAM 사용자의 것을 입력

npm install

sudo service nginx restart

rm -rf aws-exercise-*

sudo shutdown -h now
```