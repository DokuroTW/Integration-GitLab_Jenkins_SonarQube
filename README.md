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

安裝Gitlab 在ubuntu
1.先安裝依賴套件
```CMD
sudo apt update
sudo apt install ca-certificates curl openssh-server postfix tzdata perl
```
2.將repo用到本地
```CMD
curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
less /tmp/script.deb.sh
sudo bash /tmp/script.deb.sh
```
3.安裝GitLab Community
`sudo apt install gitlab-ce`
4.設定Domain
```
sudo nano /etc/gitlab/gitlab.rb

nano 中 external_url 'https://your_domain'
```
5.設定root 密碼
```
sudo nano /etc/gitlab/initial_root_password

nano 中 Password: YOUR_PASSWORD
```
6.啟動GitLab
`sudo gitlab-ctl reconfigure`
