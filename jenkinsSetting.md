## 젠킨스 setting하기
1. Java 설치
sudo apt update
sudo apt install openjdk-11-jdk -y
java -version

2. Jenkins설치
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
-> ec2 보안그룸 8080포트 추가

3. 젠킨스 접근
http://your_server_ip:8080
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
비밀번호 확인

4. Organization Folder를 사용해 각 레포리토리 내용들을 CI/CD
jenkinsfile을 고쳐서 수행 -> webhook으로 변경사항발생하면 바로바로 빌드되도록 !
