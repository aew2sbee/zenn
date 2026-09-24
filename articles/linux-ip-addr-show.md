---
title: "[Linux] ip addr showで何が確認できるのか？" # 記事のタイトル
emoji: "🐧" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["linux"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`Claude Code`などのAIの登場により、基礎をさらに理解する必要があると感じ、今回は`Linux`について学びます。
その時に、技術評論社さんからちょうど良い書籍が出版されたので、こちらを教科書として学習を進めます。

https://gihyo.jp/book/2026/978-4-297-15755-5

## 🌱 前提条件
`VirtualBox(バーチャルボックス)`に`Ubuntu`をインストールして検証作業を行います。

## 🌱 結論

:::message
```bash
ip addr show
```
**ip addr show**とは、Linuxでネットワークインターフェースの情報を表示するコマンドです。

- `ip` : ネットワーク設定を管理するコマンド(旧来の`ifconfig`の後継)
- `addr` : `address`の略。IPアドレス関連の操作を指定
- `show` : 現在の設定を表示するサブコマンド(省略して`ip addr`でも同じ結果になります)
:::

## 🌱 何が確認できるのか？
そのマシンに存在するすべての**ネットワークインターフェースの現在の状態を一覧**で確認できます。

1. インターフェースの一覧と種類
   - lo(ループバック)と enp0s3(イーサネット/NIC)の2つが存在することが分かります
2. 各インターフェースの状態
   - `<>`内の`UP` : インターフェースが有効化されている
   - `<>`内の`LOWER_UP` : ケーブル(仮想NIC)がつながっている
   - 例:enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> → 有効化されていて、ケーブル(仮想NIC)もつながっている
3. IPアドレス(inet / inet6)
   - そのマシンが今どんなIPアドレスを持っているか(IPv4・IPv6両方)
   - 例:enp0s3は 10.0.2.15/24 というIPv4アドレスを持っている → VirtualBoxのNATモードでゲストOSに割り当てられるプライベートIPアドレス(外部からはホストOSのIPアドレスに変換されて見える)
4. MACアドレス(link/ether)
   - NICに割り当てられたMACアドレス(例:08:00:27:69:3b:88。今回はVirtualBoxが生成した仮想NICのもの)
5. サブネット情報(CIDR表記)
   - /24などの表記から、そのネットワークがどの範囲のIPアドレスをカバーしているかが分かる


## 🌱 どんなときに使うのか？
1. 自分のマシンのIPアドレスは何か?を調べたいとき
   - 今回の場合は、 ゲストOS(VirtualBox上で動いているUbuntu)のIPアドレスを調べられます。
2. 「ネットワークがちゃんと繋がっているか(UPかどうか)」を確認したいとき
   - 今回の場合、`enp0s3`に`UP`と`LOWER_UP`の両方が表示されていることで「インターフェースが有効化されていて、UbuntuのLANケーブル(仮想NIC)もつながっている」と確認できます。もし`LOWER_UP`がなく`NO-CARRIER`や`state DOWN`と表示されていたら、「ケーブルが挿さっていない(=VirtualBoxのネットワーク設定でケーブルが未接続になっている)」ようなイメージで、`curl`や`apt update`が失敗する原因調査の第一歩になります。
3. 「複数のネットワーク(NAT用・ホストオンリー用など)にどう繋がっているか」を把握したいとき
   - 今回のUbuntuには、インターネットに出るための`enp0s3`(NATアダプター)に加えて、アダプター2を追加すると`enp0s8`(ホストオンリーアダプター)も表示されるようになります。これはUbuntuという1台のPCに「外出用の玄関(NAT)」と「自宅内だけで使う勝手口(ホストオンリー)」という2つの出入り口が付いているようなもので、`ip addr show`を見ればどちらの出入り口が今どのIPアドレスで開いているかが一目で分かります。

## 🌱 検証
### 1. コマンドを実行する
```bash
ip addr show
```
下記が実行結果です。

```bash
ubuntu@ubuntu:~$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:69:3b:88 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 86189sec preferred_lft 86189sec
    inet6 fd17:625c:f037:2:a00:27ff:fe69:3b88/64 scope global dynamic noprefixroute
       valid_lft 86190sec preferred_lft 14190sec
    inet6 fe80::a00:27ff:fe69:3b88/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

ツリー形式でイメージすると、次のようになります。

```text
Ubuntu
│
├── 1. lo (Ubuntu自身との通信)
│    └── 127.0.0.1
│         → 自分自身と通信するため
│
└── 2. enp0s3 (VirtualBoxのUbuntuがネットワーク通信に使っているもの)
     ├── 10.0.2.15
     │    → UbuntuのIPv4アドレス
     │
     ├── IPv6アドレス
     │
     └── MACアドレス
          08:00:27:69:3b:88
```

コマンドで表示されている項目の説明です。

| 項目 | 説明 |
| --- | --- |
| lo, enp0s3 | インターフェース名(lo=ループバック、enp0s3=イーサネット) |
| UP / LOWER_UP | UP=インターフェースが有効化されている、LOWER_UP=ケーブル(リンク)がつながっている |
| link/ether | リンク層がEthernetであることを示し、続く値がMACアドレス |
| inet | 割り当てられているIPv4アドレスとサブネットマスク(CIDR形式) |
| inet6 | IPv6アドレス(表示される場合) |


### 2. 結果を確認する-ループバック
実行結果の読み方は分かりにくいので、重要な行に絞って1行ずつ解説します。

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
```
まず、`lo: <LOOPBACK`から、ループバックであることが分かります。
次に、`UP`から、インターフェースが有効化されていることが分かります。
なお、ループバックにはケーブル(リンク)の概念がないため、`state UNKNOWN`と表示されるのが正常です。

```bash
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```
`link/loopback`は、リンク層がループバックであることを示します。
ループバックには物理的なMACアドレスがないため、すべて0になります。

```bash
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
```
`inet`とは、割り当てられているIPv4アドレスとサブネットマスク(CIDR形式)になります。
この場合だと、ローカルホストのIPであることが分かります。
`valid_lft`とは、アドレスが有効な残り時間です。foreverなので期限なし(ずっと使える)です。

```bash
    inet6 ::1/128 scope host noprefixroute
```
`inet6`とは、IPv6アドレスです。

### 3. 結果を確認する-イーサネット

```bash
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
```
まず、`enp0s3`から、ループバックではない通常のネットワークインターフェースであることが分かります。(`BROADCAST,MULTICAST`は、ブロードキャストとマルチキャストに対応していることを示します)
次に、`UP`と`LOWER_UP`から、インターフェースが有効化されていて、ケーブル(仮想NIC)もつながっていることが分かります。

```bash
    link/ether 08:00:27:69:3b:88 brd ff:ff:ff:ff:ff:ff
```
`link/ether`は、リンク層がEthernet(イーサネット)であることを示します。
続く`08:00:27:69:3b:88`がMACアドレス、`brd ff:ff:ff:ff:ff:ff`がブロードキャストアドレスです。

```bash
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 86189sec preferred_lft 86189sec
```
`inet`とは、割り当てられているIPv4アドレスとサブネットマスク(CIDR形式)になります。
この場合だと、VirtualBoxのNATが割り当てたUbuntuのIPアドレスであることが分かります。
`dynamic`とあるので、固定ではなくDHCPによって自動で割り当てられたことも分かります。
`valid_lft`とは、アドレスが有効な残り時間です。DHCPによる自動割り当てのため、`forever`ではなく秒数(86189sec)で決まっています。
`preferred_lft`は、新しい通信に優先して使われる残り時間です。

```bash
    inet6 fd17:625c:f037:2:a00:27ff:fe69:3b88/64 scope global dynamic noprefixroute
```
`inet6`とは、IPv6アドレスです。
`fd`から始まるアドレスはULA(ユニークローカルアドレス)と呼ばれ、インターネットには出ない内輪用のプライベートなIPv6アドレスになります。
こちらは、VirtualBoxのNATのルーター広告(SLAAC)によって自動で設定されたアドレスです。

```bash
    inet6 fe80::a00:27ff:fe69:3b88/64 scope link noprefixroute
```
こちらも`inet6`のIPv6アドレスです。
`fe80`から始まるアドレスはリンクローカルアドレスと呼ばれ、同じリンク(ルーターを越えない同一セグメント)内だけで自動的に使われるアドレスになります。

## 🌱 補足
### lo(ループバック)とは
「ループバック(loopback)」とは、自分自身に戻る・自分自身と通信する仕組みのことです。
**ループバックが指すのは「コマンドを実行したOSそのもの」** であり、環境によって対象が変わります。

- 例1: 今回のようにUbuntu(ゲストOS)のターミナルで実行した場合 → `lo`はUbuntu自身を指す
- 例2: Windows(ホストOS)には`ip`コマンドも`lo`という名前のインターフェースもありませんが、ループバックの仕組み自体はあり、`127.0.0.1`はWindows自身を指す

### enp0s3(イーサネット)とは
enp0s3(イーサネット)は、Linuxで使われるネットワークインターフェース(NIC)の名前です。
旧来は`eth0`のような検出順の名前でしたが、現在は`udev`(systemd)による接続位置に基づく命名規則(`Predictable Network Interface Names`)が標準です。
- 今回の場合は、ゲストOS(VirtualBox上のUbuntu自身)から**外へ通信する窓口**になる
   - 例えば、パブリックなネットワークへの接続、プライベートなネットワークへの接続
