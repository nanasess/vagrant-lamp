vagrant-lamp
=============

## 概要

﻿CentOS, Apache, PostgreSQL, MySQL, PHP の環境を Vagrant で作ります。

EC-CUBE3 の下限動作環境です。

## 設定方法

## 事前準備

### Vagrant のインストール

1. <http://www.vagrantup.com/> からダウンロードしてインストール
2. 必要なプラグインをインストールしておく

```sh
vagrant plugin install vagrant-omnibus
```

### VirtualBox のインストール

<https://www.virtualbox.org/> からダウンロードしてインストール

### 起動

```sh
vagrant up
```

### ssh 接続

```sh
vargrant ssh
```

### 設定変更する場合

```sh
vargrant provision
```

### 仮想マシンを破棄する場合

```sh
vargrant destroy
```

## 参考

https://github.com/onehealth-cookbooks/apache2/tree/master
https://github.com/onehealth-cookbooks/apache2/pulls

http://www.vagrantbox.es

https://github.com/mattandersen/vagrant-lamp/tree/master/chef/certificates

http://tech.basicinc.jp/Vagrant/2014/02/02/vagrant_chef/

http://qiita.com/sampleb3/items/86d06b10989e58dc6dde

### 最近の記述の仕方
http://qiita.com/joker1007/items/1b62e3a36b4f435c53a2
