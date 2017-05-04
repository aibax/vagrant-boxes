# 初期インストール （VirtualBoxコンソール）

## VirtualBoxでCentOS 6のイメージを作成
- 名前: centos7
- タイプ: Linux
- バージョン: Red Hat (64-bit)
- メモリーサイズ: 768MB （デフォルト）
- ハードディスク: 仮想ハードディスクを作成する
- ファイルの場所: 任意の場所を指定
- ファイルサイズ: 8.00GB （デフォルト）
- ハードディスクのファイルタイプ: VDI
- 物理ハードディスクにあるストレージ: 可変サイズ

## 追加設定
- ストレージ > コントローラー:IDE  
    光学ドライブとしてOSのISOイメージを指定 : CentOS-7-x86_64-Minimal-1511.iso
- ネットワーク > アダプター1 > ポートフォワーディング  
    SSHのポートフォワーディング設定を追加 : プロトコル:TCP, ホストポート:10022, ゲストポート: 22

## インストール設定
- 言語: 日本語
- 地域設定
    - 日付と時刻: アジア/東京 タイムゾーン（デフォルト）
    - 言語サポート: 日本語（デフォルト）
    - キーボード: 日本語（デフォルト）
- SECURITY
    - SECURITY POLICY: No profile selected（デフォルト）
- ソフトウェア
    - インストールソース: ローカルメディア
    - ソフトウェアの選択: 最小限のインストール
- システム
    - インストール先: 自動パーティション設定
    - ネットワークとホスト名: 初期値
- ユーザーの設定
    - ROOTパスワード: vagrant
    - ユーザーの作成: 作成しない

--------------------------------------------------------------------------------

# 事前設定 （VirtualBoxコンソール）

1. ネットワークインタフェースを有効化

```
# sed -i -e 's/^ONBOOT=.*$/ONBOOT=yes/g' /etc/sysconfig/network-scripts/ifcfg-(インタフェース名)
```

2. SELinuxを無効化

```
# sed -i -e 's/^SELINUX=.*$/SELINUX=disabled/g' /etc/selinux/config
# reboot
```

--------------------------------------------------------------------------------

# Ansibleで環境構築 （ローカル）

```
$ ansible-playbook -i production centos7.yml --ask-pass
```

--------------------------------------------------------------------------------

# 事後設定  （VirtualBoxコンソール）

1. Ansibleの作業ファイルを削除

```
# rm -Rf ~/.ansible
```

2. 仮想HDDの領域を最適化

```
# dd if=/dev/zero of=/ZERO bs=1M
# rm -f /ZERO
```

3. コマンド実行履歴を削除

```
# export HISTSIZE=0
```

4. シャットダウン

```
# shutdown -h now
```

--------------------------------------------------------------------------------

# Boxの作成 （ローカル）

```
$ vagrant package --base centos7 package.box
$ vagrant box add --name CentOS-7-x86_64-minimal-ja_JP package.box
$ vagrant box list
$ rm package.box
```

--------------------------------------------------------------------------------
# Vagrantで起動 （ローカル）
Vagrantfileを作成して起動

```
$ vagrant init CentOS-7-x86_64-minimal-ja_JP
$ vagrant up
$ vagrant ssh
```
