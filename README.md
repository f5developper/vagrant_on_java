### やること
- [ ] centos5.5のインストール
- [ ] vagrantのプロビジョニング
- [ ] javaのインストール
- [ ] tomcatのインストール

#### centos5.5のインストール
* ファイルのダウンロード  
 http://archive.kernel.org/centos-vault/5.5/isos/x86_64/　からCentOS-5.5-x86_64-bin-1of8.isoファイルを8までダウンロード  
* ダウンロードしたisoファイルの結合  
virtualboxでisoを指定する場合一つしか指定する方法がわからないので結合する。
```
//コピーコマンドで指定8個分繰り返す
copy /b  centos-1.iso + centos-2.iso centos.iso
```
* vguestAddtionのインストール
これがないとクリップボードとかをホストとやりとりできないので、やる
* yumリポジトリの更新
osが古すぎてyumが使えないので、baseurlを変更する。  
/etc/yum.repose.d/配下の以下の記述を修正
```
name=RHEL $releasever - local repo`
#baseurl=http://10.235.26.47/rhel-x86_64-server-5/getPackage`
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
baseurl=http://vault.centos.org/5.5/os/$basearch/
enabled=1
gpgcheck=0
```
* 下準備
```
$ yum install gcc
$ yum install make
$ yum install kernel-devel
```
VirtualBoxでCentOSを立ち上げて、メニューバー＞＞デバイス＞＞Guest Additions CDイメージの挿入  
ターミナルを立ち上げて、以下コマンド実施
```
$ mkdir /tmp/package
$ mount -r /dev/cdrom /tmp/package
$ ./VBoxLinuxAdditions.run install
//正常したかどうかログが出力されるので確認を
```
イメージの再起動を行う


* SSH公開鍵で接続可能な状態に設定   
```
mkdir /home/vagrant/.ssh
chmod 700 /home/vagrant/.ssh
wget --no-check-certificate "https://raw.github.com/mitchellh/vagrant/master/keys/vagrant.pub" -O /home/vagrant/.ssh/authorized_keys
chmod 600 /home/vagrant/.ssh/authorized_keys
chown -R vagrant /home/vagrant/.ssh
```
imageにログインしてvagrantユーザを作成する。
```
$ useradd vagrant
$ passwd vagrant vagrant123
```
結合したisoファイルを指定して、virtualboxに突っ込んで起動する。
その後birtualboxを保存しているディレクトリに移動して、以下コマンド実行
```
$ vagrant package --base centos55 --output centos55vb
$ vagrant box add centos55 centos55vb
```
vagrantファイル作成
```
$ vagrant init centos55
```
