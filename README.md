NESTJS + AWS CodeDeploy 배포 테스트

## AWS CodeDeploy
 - appspec.yml : 이 파일은 배토가 서버에서 수행할 전체 단계를 제어합니다. 일련의 단계는 배포할 파일과 배포를 완료하기 위해 서버에서 실행할 스크립트를 정의합니다. 배포 방법과 대상을 제어하기 위해 아래 섹션을 정의합니다.
 - files : 입력 아티팩트에서 배포할 파일과 대상 서버의 폴더를 정의합니다.
 - BeforeInstall : 이 단계에서는 앱이 서버에서 실행하는데 필요한 모든 종속설을 설치합니다. 보통 NodeJs & npm을 설치합니다.
 - AfterInstall : 여기에서 최종 앱을 실행하기 위한 전체 조건인 서버에서 다른 스크립트 또는 명령을 실행할 수 있습니다. 이 예에서는 npm install 명령을 실행하여 애플리케이션의 package.json에 정의된 NodeJs 패키지를 설치합니다.
 - ApplicationStart : 이것은 응용 프로그램을 시작하기 위해 스크립트를 실행하는 마지막 단계입니다. 스크립트와 명령은 시작하는 응용 프로그램에 따라 다릅니다. 여기에 npm 명령을 실행하여 NodeJs 앱에서 익스프레스 서버를 시작합니다 이 애플리케이션이 성공적으로 시작되면 배포의 마지막 단계를 보여줍니다.

## 서버를 생성하자 마자 해야하는 codedeploy 관련 작업
#!/bin/bash
sudo yum -y update
sudo yum -y install ruby
sudo yum -y install wget
cd /home/ec2-user
wget https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install
sudo chmod +x ./install
sudo ./install auto

## ec2 서버를 만들고 nvm을 통해 node, npm 및 pm2를 미리 다운로드
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install 14
npm i -g pm2