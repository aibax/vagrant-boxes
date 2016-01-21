---------- [事前設定] （VirtualBoxコンソール） ----------
1. ネットワークインタフェースを有効化
2. SELinuxを無効化

# sed -i -e 's/^ONBOOT=.*$/ONBOOT=yes/g' /etc/sysconfig/network-scripts/ifcfg-(インタフェース名)
# sed -i -e 's/^SELINUX=.*$/SELINUX=disabled/g' /etc/sysconfig/selinux
# reboot

---------- Ansibleで環境構築 （ローカル） ----------

$ ansible-playbook -i production centos7.yml --ask-pass

---------- [事後設定]  （VirtualBoxコンソール）  ----------
1. Ansibleの作業ファイルを削除
2. 仮想HDDの領域を最適化
3. コマンド実行履歴を削除

# rm -Rf ~/.ansible
# dd if=/dev/zero of=/ZERO bs=1M
# rm -f /ZERO
# export HISTSIZE=0
# shutdown -h now

---------- Boxの作成 （ローカル） ----------
$ vagrant package --base centos7 package.box
$ vagrant box add --name CentOS-7-x86_64-minimal-ja_JP package.box
$ vagrant box list
$ rm package.box

---------- Vagrantで起動 （ローカル） ----------
Vagrantfileを作成して起動
$ vagrant init CentOS-7-x86_64-minimal-ja_JP
$ vagrant up
$ vagrant ssh
