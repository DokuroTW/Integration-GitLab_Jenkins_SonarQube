# 安裝Gitlab 在ubuntu
### 1.先安裝依賴套件
```CMD
sudo apt update
sudo apt install ca-certificates curl openssh-server postfix tzdata perl
```
### 2.將repo用到本地
```CMD
curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
less /tmp/script.deb.sh
sudo bash /tmp/script.deb.sh
```
### 3.安裝GitLab Community

`sudo apt install gitlab-ce`

### 4.設定Domain
```
sudo nano /etc/gitlab/gitlab.rb

nano 中 external_url 'https://your_domain'
```
### 5.設定root 密碼
```
sudo nano /etc/gitlab/initial_root_password

nano 中 Password: YOUR_PASSWORD
```
### 6.啟動GitLab

`sudo gitlab-ctl reconfigure`

# 安裝Jenkins LTS
```
sudo apt update
sudo apt install fontconfig openjdk-17-jre
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

#安裝SonarQube LTS

### 1.安裝JAVA
```
sudo apt update
sudo apt install fontconfig openjdk-17-jre
```

### 2.安裝PostgreSQL
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
sudo apt-get -y install postgresql postgresql-contrib

sudo systemctl start postgresql
sudo systemctl enable postgresql
```
### 3.設定PostgreSQL帳號
```
sudo passwd postgres
su - postgres
createuser sonar

psql
ALTER USER sonar WITH ENCRYPTED password 'sonar';
CREATE DATABASE sonarqube OWNER sonar;
grant all privileges on DATABASE sonarqube to sonar;
\q
exit
```
### 4.安裝SonarQube
```
cd /tmp
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip
sudo unzip sonarqube-9.9.0.65466.zip -d /opt
sudo mv /opt/sonarqube-9.9.0.65466 /opt/sonarqube
```

### 5.SonarQube 設定檔
```
sudo nano /opt/sonarqube/conf/sonar.properties
--nano中--
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube
```
```
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh
--nano中--
APP_NAME=="SonarQube"
RUN_AS_USER=sonar
```

### 6.啟動SonarQube
```
sudo su sonar
cd /opt/sonarqube/bin/linux-x86-64/

./sonar.sh start
```
# Integration-GitLab_Jenkins_SonarQube
使用webhook與板機使源碼掃描自動化
![1716735638506](https://github.com/DokuroTW/Integration-GitLab_Jenkins_SonarQube/assets/100449940/b0dd5802-0ded-4b63-a0bf-17114769936b)

在透過Jenkins來整合GitLab與SonarQube

當Gitlab發生push事件，將觸發SonarQube對源碼掃描
![1716736812427](https://github.com/DokuroTW/Integration-GitLab_Jenkins_SonarQube/assets/100449940/06ff0872-1687-4563-a631-59dacb5cf802)

在Jenkins設定好憑證與pipeline script
![1716736099699](https://github.com/DokuroTW/Integration-GitLab_Jenkins_SonarQube/assets/100449940/68be9a19-a42c-443a-b783-5809a9cbdd8f)

![1716735776770](https://github.com/DokuroTW/Integration-GitLab_Jenkins_SonarQube/assets/100449940/46cbed3a-f370-410d-99a9-e23a52481742)

在Gitlab設定好webhook與Access Tokens

![1716735934620](https://github.com/DokuroTW/Integration-GitLab_Jenkins_SonarQube/assets/100449940/30287715-6174-4b10-bb58-31161aa7ab01)

在SonarQube設定好Token

![1716736236129](https://github.com/DokuroTW/Integration-GitLab_Jenkins_SonarQube/assets/100449940/dfffa63e-ff12-494d-9f54-c8a99b694ff8)

就可以很好的整合起來，並且掃描結束後更可以透過SonarQube套件來產生像ZAP一樣的報表。
![1716736357449](https://github.com/DokuroTW/Integration-GitLab_Jenkins_SonarQube/assets/100449940/dbeb9995-c437-4b26-87b5-919466405816)

