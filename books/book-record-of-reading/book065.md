---
title: '2026.10: Software Design for Beginners② はじめてのLinux'
---

## 🌱 書籍情報
https://gihyo.jp/book/2026/978-4-297-15755-5

## 🌱 本の概要
公式ページの紹介文を要約しています。

Software Design誌の人気特集記事を再編集して収録した、Linuxの入門書。Webアプリ開発でもAI開発でも実行基盤の多くはLinuxであることから、コマンドや権限管理、ディレクトリ構造、パッケージ管理などの基本を、操作しながら理解できるように解説している。

## 🌱 読む目的
- AIの登場により、エンジニアとしての基礎能力が大事だと考えてOSの理解力を高めるため

## 🌱 読書メモ

### 📌 NATとは？
NAT（Network Address Translation）は、プライベートIPアドレスとグローバルIPアドレスを変換する仕組み。家庭や会社の中にある複数の機器が、ルーターが持つ1つのグローバルIPアドレスを共有して、インターネットに接続できる。

![家庭のネットワークにあるPC AとPC Bがプライベートの送信元アドレスでルーターに送り、ルーターが変換表に記録したうえで送信元をグローバルIPとポート番号に書き換えてWebサーバーへ送り、返事は変換表を見てPC Aに戻すことを示した図](/images/books/book-record-of-reading/book065-nat-basics.drawio.png)

:::details 🤖 Claude に相談した内容
**なぜ NAT が必要なのか**

- IPv4 のアドレスは約43億個しかなく、インターネットにつながるすべての機器に1つずつグローバルIPアドレスを配るには足りない。
- そこで、家庭や会社の中ではプライベートIPアドレスを使う。プライベートIPアドレスは、組織の中だけで自由に使ってよい範囲として決められている。

| 範囲 | よく見かける場所 |
| --- | --- |
| `10.0.0.0`〜`10.255.255.255`（`10.0.0.0/8`） | 会社のネットワーク、VirtualBox の NAT（`10.0.2.x`） |
| `172.16.0.0`〜`172.31.255.255`（`172.16.0.0/12`） | 会社のネットワーク、Docker |
| `192.168.0.0`〜`192.168.255.255`（`192.168.0.0/16`） | 家庭のネットワーク |

- プライベートIPアドレスはほかの組織と重なってもよい代わりに、インターネット上では使えない。そのため、インターネットへ出るときにルーターがグローバルIPアドレスに書き換える。これが NAT。

**NAT の動き**

1. PC A が Webサーバーにアクセスすると、パケットの送信元は `192.168.1.10:50001`（IPアドレス:ポート番号）になっている。
2. ルーターは送信元を自分のグローバルIPアドレス `203.0.113.5:40001` に書き換えて送り、「`40001` は PC A の `50001`」という対応を変換表に記録する。
3. Webサーバーは `203.0.113.5:40001` 宛てに返事を送る。Webサーバーからは、PC A ではなくルーターと通信しているように見える。
4. ルーターは変換表を見て、宛先を `192.168.1.10:50001` に書き戻し、PC A に渡す。

- PC A と PC B が同じポート番号 `50001` を使っていても、ルーターが外側で別のポート番号（`40001` と `40002`）を割り当てるので、返事を正しく振り分けられる。
- 変換表には内側から始めた通信だけが記録される。そのため、外から始まる通信は、どの機器に渡せばよいかが分からず届かない。インターネットから家庭内のPCに直接アクセスされにくいのは、このため。

**用語の補足**

- 厳密には、IPアドレスだけを変換するものを NAT、ポート番号も合わせて変換するものを NAPT（IPマスカレードとも呼ぶ）という。家庭のルーターが行っているのは NAPT だが、一般には NAPT もまとめて NAT と呼ばれることが多い。
- 次に説明する VirtualBox の NAT モードでは、VirtualBox の NAT エンジンがこのルーターの役割を担う。VM が PC A、NAT エンジンがルーター、ホストPCの IP アドレスがグローバルIPアドレスの役にあたる。

参考: https://www.rfc-editor.org/rfc/rfc1918

参考: https://www.rfc-editor.org/rfc/rfc3022
:::

### 📌 VirtualBoxのNATでUbuntuがインターネットに接続できる仕組みとは？
VirtualBox の NAT モードでは、VirtualBox が VM 専用の小さなルーターになる。VM の通信をいったん受け取り、ホストPCの通信として送り直すことで、Ubuntu からインターネットに接続できる。

![UbuntuがゲートウェイのVirtualBoxのNATエンジンにパケットを送り、NATエンジンがホストの通信として送り直し、ホストのNICから家庭のルーターを通ってインターネットへ出て、返事は対応表をもとにUbuntuへ戻る流れを示した図](/images/books/book-record-of-reading/book065-virtualbox-nat.drawio.png)

:::details 🤖 Claude に相談した内容
Ubuntu で `curl https://example.com` を実行したときの流れは次のとおり。

1. 宛先がネットワークの外なので、Ubuntu はデフォルトゲートウェイの `10.0.2.2` にパケットを送る。`10.0.2.2` は実在する機械ではなく、VirtualBox が用意した仮想ルーター。
2. VirtualBox はパケットを受け取ると、ホストOS上のアプリとして通信を始める。外から見ると「ホストPCの VirtualBox が example.com に接続した」ように見える。このとき「この通信は VM の `10.0.2.15` のもの」という対応を記録しておく。
3. ホストの `192.168.1.10` はプライベートIPなので、家庭のルーターがグローバルIPに変換する。つまり NAT が2段重なっている。
4. グローバルIPでインターネットに出る。
5. 返事は家庭のルーターからホストに届き、VirtualBox が対応表を見て Ubuntu の `10.0.2.15` に渡す。

| 項目 | 内容 |
| --- | --- |
| IP の割り当て | VirtualBox 内蔵の DHCP が `10.0.2.15` を配る。NAT は VM ごとに独立しているので、複数の VM が同じ `10.0.2.15` でも問題ない |
| DNS | `10.0.2.3` が名前解決の問い合わせをホスト側に中継する。Ubuntu では `systemd-resolved`（`127.0.0.53`）がまず受け取り、そこから `10.0.2.3` に転送する |
| 外からの接続 | 内側から外への通信だけが通る。外から VM には直接届かない（対応表にない通信は、どの VM に渡せばよいか分からないため） |
| 外から接続したい場合 | ポートフォワーディングを設定する。例えばホストの `2222` 番を VM の `22` 番に転送すると、ホストから `ssh -p 2222 ユーザー名@127.0.0.1` で接続できる |
| VM 同士の通信 | NAT モードではできない。必要な場合は「NAT ネットワーク」や「ホストオンリーアダプター」を使う |

Ubuntu では、次のコマンドで NAT モードの構成を確認できる。

```bash
ip addr show        # enp0s3 などのNICに 10.0.2.15 が割り当てられている
ip route            # default via 10.0.2.2 と表示される
resolvectl status   # DNS Servers に 10.0.2.3 が表示される
```

- `10.0.2.15`（VM）、`10.0.2.2`（ゲートウェイ）、`10.0.2.3`（DNS）は、VirtualBox の NAT モードの標準の構成。
- 図の `192.168.1.10` は、ホストPCのIPアドレスの一例。

参考: https://www.virtualbox.org/manual/topics/networkingdetails.html#network_nat
:::

### 📌 NATモードのVMにホストOSからアクセスできないのはなぜ？
DHCP は VM に IP アドレスを配るだけで、NAT は VM から始めた通信の返事を VM に戻す仕組み。VM の `10.0.2.15` は VirtualBox の NAT エンジンの内側だけで使われるアドレスなので、ホストOSからは届かない。ホストOSから接続したい場合は、ポートフォワーディングで入口を作る。

![ホストOS、VirtualBoxのNATエンジン、Ubuntuを並べ、VMから外への通信は対応表に記録されて通ること、ホストOSからssh 10.0.2.15で接続しようとしても経路も対応表の記録もないため届かないこと、ポートフォワーディングでホストの2222番をVMの22番へ転送すれば接続できることを示した図](/images/books/book-record-of-reading/book065-virtualbox-nat-host-access.drawio.png)

:::details 🤖 Claude に相談した内容
ホストOSからアクセスできない理由は2つある。

1. **ホストOSに `10.0.2.0/24` への経路がない**
   NAT モードでは、ホストOS側に `10.0.2.x` のアドレスを持つ NIC が作られない。そのため、ホストOSで `ssh 10.0.2.15` を実行すると、宛先が分からないパケットとしてデフォルトゲートウェイ（家庭のルーター）に送られ、VM には届かない。そもそも NAT は VM ごとに独立していて、複数の VM が同じ `10.0.2.15` を持てるため、ホストOSから見て `10.0.2.15` がどの VM かを決められない。
2. **外から始まる通信は対応表にない**
   NAT エンジンは、VM から始めた通信を対応表に記録し、その返事だけを VM に戻す。外から始まる通信には記録がないため、NAT エンジンに届いたとしても、どの VM に渡せばよいか分からない。

DHCP と NAT の役割を分けると、次のようになる。

| 仕組み | 役割 | ホストOSからのアクセスとの関係 |
| --- | --- | --- |
| DHCP | VM に IP アドレス（`10.0.2.15`）、ゲートウェイ（`10.0.2.2`）、DNS（`10.0.2.3`）を配る | アドレスを配るだけで、外から届く道は作らない |
| NAT | VM の通信をホストの通信として送り直し、返事を VM に戻す | VM から始めた通信だけが対象で、外から始まる通信は通さない |

ホストOSから VM に接続する方法は次のとおり。

| 方法 | 仕組み | 向いている場面 |
| --- | --- | --- |
| ポートフォワーディング | NAT のまま、ホストの特定のポートに届いた通信を VM のポートへ転送する | SSH など、決まったポートだけ使えればよい場合 |
| ホストオンリーアダプター | ホストOSに仮想 NIC（例: `192.168.56.1`）が作られ、ホストOSと VM が同じネットワークに入る | ホストOSから VM の複数のポートに自由に接続したい場合 |
| ブリッジアダプター | VM が家庭のルーターに直接つながり、ホストPCと同じネットワークの IP アドレスをもらう | 同じネットワークの他の PC からも VM に接続したい場合 |

ポートフォワーディングで SSH 接続する場合の例は次のとおり。VM 名は自分の環境に合わせて置き換える。

```bash
# Ubuntu に SSH サーバーを入れる
sudo apt install openssh-server
```

```bash
# ホストOS（Windows）で、ホストの 2222 番を VM の 22 番に転送する規則を追加する（VMを停止した状態で実行）
VBoxManage modifyvm "Ubuntu" --natpf1 "guestssh,tcp,,2222,,22"

# ホストOSから接続する
ssh -p 2222 ユーザー名@127.0.0.1
```

- 規則は、VirtualBox の画面の「設定」→「ネットワーク」→「アダプター1」→「ポートフォワーディング」からも追加できる。
- 起動中の VM に追加する場合は、`VBoxManage controlvm "Ubuntu" natpf1 "guestssh,tcp,,2222,,22"` を使う。
- ホストOS（Windows）で `ipconfig` を実行しても、NAT モードだけなら `10.0.2.x` のアドレスは表示されない。ホストオンリーアダプターを追加すると、`VirtualBox Host-Only Ethernet Adapter` が表示される。

参考: https://www.virtualbox.org/manual/topics/networkingdetails.html#natforward

参考: https://www.virtualbox.org/manual/topics/networkingdetails.html#network_hostonly
:::

### 📌 DHCPでIPアドレスが変わると何が困る？固定するには？
DHCP で配られる IP アドレスは「一定期間の貸し出し」なので、VM の再起動や貸し出し期間（リース）の期限切れをきっかけに変わることがある。アドレスが変わると、古いアドレスで接続しようとしてつながらなくなる。サーバーのように「いつも同じ場所にいてほしい」機器は、IP アドレスを固定しておくと、接続先の設定を変えずに使い続けられる。

![DHCPの場合は再起動後にUbuntuのアドレスが192.168.56.101から192.168.56.102に変わり、古いアドレスへのsshがつながらなくなること、固定IPの場合は192.168.56.10のまま変わらず、いつでもつながることを左右で比べ、下にホストオンリーネットワークのアドレスをホストOS、固定IPに使う範囲、DHCPサーバー、DHCPで配る範囲に分けて使う例を示した図](/images/books/book-record-of-reading/book065-dhcp-vs-static.drawio.png)

:::details 🤖 Claude に相談した内容
**IP アドレスが変わる仕組み**

- DHCP サーバーは、IP アドレスを期限付きで貸し出す。この期限をリース期間という。
- 多くの場合、同じ機器には前回と同じアドレスが配られるが、必ず同じになるとは限らない。例えば、次のようなときに変わる。
  - VM を止めているあいだにリースが切れ、そのアドレスが別の VM に貸し出された
  - VM を複製して MAC アドレスが変わった（DHCP サーバーは MAC アドレスで機器を見分けている）
  - DHCP サーバーの設定や貸し出しの記録がリセットされた

**アドレスが変わることで起きる困りごと**

| 困りごと | 例 |
| --- | --- |
| 接続できなくなる | `ssh ユーザー名@192.168.56.101` や `~/.ssh/config` に書いたアドレスでつながらなくなる |
| 別の機器につながる | 古いアドレスが別の VM に貸し出されていると、意図しない機器に接続してしまう |
| アプリの設定が合わなくなる | Web アプリの設定に書いたデータベースサーバーのアドレスが古くなり、接続エラーになる |
| アクセス制限が効かなくなる | ファイアウォールで「`192.168.56.101` からだけ許可」としていると、許可したい相手がはじかれる |
| 毎回確認する手間が増える | 起動するたびに `ip addr show` でアドレスを調べ直すことになる |

**固定するとメリットになる理由**

- サーバーは「ほかの機器から接続される側」なので、住所が変わらないことが大事になる。アドレスが変わらなければ、接続先・アプリの設定・ファイアウォールのルール・手順書を一度書けば使い続けられる。
- 一方で、PC やスマートフォンのように「接続しに行く側」の機器は、アドレスが変わっても困らないので DHCP のままでよい。
- 固定にもデメリットはある。アドレスを手で管理する必要があり、ほかの機器と同じアドレスを設定すると、どちらも正しく通信できなくなる（アドレスの重複）。

**固定するときの注意: DHCP で配る範囲を避ける**

- DHCP で配る範囲のアドレスを固定に使うと、DHCP サーバーが同じアドレスをほかの VM に配ってしまい、重複する。
- 図のように、DHCP で配る範囲とは別の範囲（例: `.2`〜`.99`）から選ぶ。VirtualBox のホストオンリーネットワークで DHCP が配る範囲は、「ファイル」→「ツール」→「ネットワークマネージャー」の「DHCPサーバー」タブで確認できる。
- NAT モードの `10.0.2.15` は、ホストOSから直接アクセスできないため、固定しても意味がない。固定が役立つのは、ホストオンリーアダプターやブリッジアダプターで、ほかの機器から接続するアドレスのほう。

**固定する方法1: Ubuntu Server（netplan）**

Ubuntu Server では、ネットワークの設定を netplan の YAML ファイルに書く。ホストオンリーアダプター（ここでは `enp0s8`）を `192.168.56.10` に固定する例は次のとおり。NIC の名前は `ip addr show` で確認する。

```bash
sudo vi /etc/netplan/60-hostonly.yaml
```

```yaml
network:
  version: 2
  ethernets:
    enp0s8:
      dhcp4: false
      addresses:
        - 192.168.56.10/24
```

```bash
sudo chmod 600 /etc/netplan/60-hostonly.yaml   # 他のユーザーが読めると netplan が警告を出す
sudo netplan try     # 設定を試す。Enter を押さないと120秒後に元に戻る
sudo netplan apply   # 設定を反映する
ip addr show enp0s8  # 192.168.56.10 になったかを確認する
```

- `netplan try` は、設定を間違えて接続できなくなっても自動で元に戻るので、SSH で接続している最中に変更するときに安全。
- NAT 用のアダプター（`enp0s3`）は、インターネットに出るために DHCP のままにしておく。
- 家庭のネットワーク（ブリッジアダプター）で固定する場合は、インターネットに出るためのゲートウェイと DNS も書く必要がある。

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 192.168.1.50/24
      routes:
        - to: default
          via: 192.168.1.1     # 家庭のルーター
      nameservers:
        addresses: [192.168.1.1]
```

**固定する方法2: Ubuntu Desktop や Red Hat 系（nmcli）**

Ubuntu Desktop や Red Hat 系では、NetworkManager がネットワークを管理しているので、`nmcli` コマンドで設定する。

```bash
nmcli connection show   # 接続名を確認する（例: 「有線接続 2」）
sudo nmcli connection modify "有線接続 2" ipv4.method manual ipv4.addresses 192.168.56.10/24
sudo nmcli connection up "有線接続 2"   # 設定を反映する
```

- 画面から設定する場合は、「設定」→「ネットワーク」→ 対象の有線接続の歯車アイコン →「IPv4」で「手動」を選び、アドレスとネットマスクを入力する。
- ゲートウェイと DNS が必要な場合は、`ipv4.gateway 192.168.1.1 ipv4.dns 192.168.1.1` を追加する。

**固定する方法3: DHCP サーバー側で予約する**

- VM 側は DHCP のままにして、DHCP サーバー（家庭のルーターなど）に「この MAC アドレスには、いつもこのアドレスを配る」と登録する方法もある。DHCP 予約や固定割り当てと呼ばれる。
- 機器側の設定を変えずに済み、アドレスの管理を DHCP サーバーにまとめられる。設定画面は、ルーターの機種によって異なる。

参考: https://netplan.readthedocs.io/en/stable/examples/

参考: https://networkmanager.dev/docs/api/latest/nmcli.html
:::

### 📌 ApacheのインストールはDebian系とRed Hat系で何が違う？
Apache は、Debian / Ubuntu では `apache2`、Red Hat 系（RHEL・Rocky Linux など）では `httpd` という名前でインストールする。公開するファイルを置く `/var/www/html/` は同じだが、設定ファイルとログの場所や名前、サイト設定の追加方法が違う。

![Debian/UbuntuとRed Hat系それぞれについて、設定ファイルの/etc/apache2と/etc/httpd、公開するファイルの/var/www/html、ログの/var/log/apache2と/var/log/httpd、プログラム本体とモジュールの/usr配下の場所を、役割ごとに色分けして並べた図](/images/books/book-record-of-reading/book065-apache-directories.drawio.png)

:::details 🤖 Claude に相談した内容
**インストールと起動の違い**

| | Debian / Ubuntu | Red Hat 系 |
| --- | --- | --- |
| インストール | `sudo apt install apache2` | `sudo dnf install httpd` |
| サービス名 | `apache2` | `httpd` |
| インストール後の起動 | 自動で起動し、OS の起動時にも立ち上がる設定になる | 起動しない。`sudo systemctl enable --now httpd` で起動と自動起動を設定する |
| ファイアウォール | Ubuntu の `ufw` は初期状態で無効なので、そのままアクセスできる | `firewalld` が有効なので、`sudo firewall-cmd --permanent --add-service=http` と `sudo firewall-cmd --reload` で HTTP を許可する |
| Apache を動かすユーザー | `www-data` | `apache` |
| 設定の文法チェック | `sudo apache2ctl configtest` | `sudo apachectl configtest` |

**どのディレクトリに何が入っているか**

| 役割 | Debian / Ubuntu | Red Hat 系 | 入っているもの |
| --- | --- | --- | --- |
| メインの設定 | `/etc/apache2/apache2.conf` | `/etc/httpd/conf/httpd.conf` | Apache 全体の設定 |
| サイトの設定 | `/etc/apache2/sites-available/` に置き、`sites-enabled/` にリンクを作って有効にする | `/etc/httpd/conf.d/` に `*.conf` を置くと読み込まれる | バーチャルホスト（サイトごとのドメイン名や公開ディレクトリ） |
| モジュールの設定 | `/etc/apache2/mods-available/`・`mods-enabled/` | `/etc/httpd/conf.modules.d/` | どのモジュールを読み込むか |
| 公開するファイル | `/var/www/html/` | `/var/www/html/` | HTML・画像など、ブラウザに返すファイル |
| ログ | `/var/log/apache2/access.log`・`error.log` | `/var/log/httpd/access_log`・`error_log` | アクセスの記録とエラーの記録 |
| プログラム | `/usr/sbin/apache2`・`/usr/lib/apache2/modules/` | `/usr/sbin/httpd`・`/usr/lib64/httpd/modules/` | Apache 本体とモジュールの本体 |

**自分のサイトを追加するときに何をどこに置くか**

- 設定ファイル: メインの設定ファイルは直接書き換えず、サイトごとに別のファイルを作る。パッケージを更新したときに、自分の変更と配布元の変更がぶつかりにくくなる。
  - Debian / Ubuntu: `/etc/apache2/sites-available/mysite.conf` を作り、`sudo a2ensite mysite` で有効にする（`sites-enabled/` にリンクが作られる）。モジュールは `sudo a2enmod rewrite` のように有効にする。
  - Red Hat 系: `/etc/httpd/conf.d/mysite.conf` を作るだけで読み込まれる。ファイル名は `.conf` で終わらせる。
- 公開するファイル: `/var/www/html/` か、サイトごとに `/var/www/mysite/` のようなディレクトリを作って置く。
- 設定を変えた後は、文法チェックをしてから `sudo systemctl reload apache2`（Red Hat 系は `httpd`）で反映する。

**そのほかの違い**

- インストール直後のトップページ: Debian / Ubuntu は `/var/www/html/index.html` が用意されている。Red Hat 系は `/var/www/html/` が空で、`/etc/httpd/conf.d/welcome.conf` の設定によってテスト用のページが表示される。
- Red Hat 系では SELinux が有効なので、`/var/www/` の外にファイルを置くと、ファイルの権限が正しくてもアクセスが拒否されることがある。

参考: https://cwiki.apache.org/confluence/spaces/HTTPD/pages/115522293/DistrosDefaultLayout

参考: https://ubuntu.com/server/docs/how-to/web-services/install-apache2/

参考: https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/deploying_web_servers_and_reverse_proxies/setting-apache-http-server_deploying-web-servers-and-reverse-proxies
:::

### 📌 サービスやプロセスが動いているかを確認するには？
サービス（systemd が管理しているもの）が動いているかは `service` コマンドや `systemctl status` で、OS 上で動いているプロセス全体は `ps` コマンドで確認する。Apache のようなサービスは `systemctl status` で、自分で起動したスクリプトなども含めて見たいときは `ps` を使う。

![ps auxではOS上のすべてのプロセスが見え、その中のapache2.serviceに属するrootの親プロセスとwww-dataの子プロセスだけがsystemctl status apache2で見える範囲であり、sshdやnohupで起動したスクリプト、ログイン中のシェルはps auxでしか見えないことを示した図](/images/books/book-record-of-reading/book065-service-vs-ps.drawio.png)

:::details 🤖 Claude に相談した内容
| | `service` / `systemctl status` | `ps` |
| --- | --- | --- |
| 見る単位 | サービス（systemd が管理しているもの） | プロセス（OS 上で動いているものすべて） |
| 対象 | `apache2` など、名前を指定した1つのサービス | 自分で起動したスクリプトやコマンドも含めた全部 |
| 分かること | 動いているか、自動起動するか、直近のログ | 誰が動かしているか、CPU やメモリをどれだけ使っているか |

**`service` コマンドでステータスを確認する**

```bash
sudo service apache2 status   # Debian / Ubuntu
sudo service httpd status     # Red Hat 系
```

| 場面 | 確認したいこと |
| --- | --- |
| インストールや起動の直後 | ちゃんと動き始めたか |
| 設定を変えて再読み込みや再起動をした後 | 設定ミスで止まっていないか |
| 「サイトが表示されない」とき | まず Apache が動いているかを切り分ける（動いていればファイアウォールや設定、止まっていれば Apache 自体を疑う） |
| サーバーを再起動した後 | OS の起動時に自動で立ち上がったか |

Ubuntu での出力例は次のとおり（内容は一例）。

```
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/apache2.service; enabled; preset: enabled)
     Active: active (running) since Thu 2026-10-08 10:15:32 JST; 2h 5min ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 1234 (apache2)
      Tasks: 55 (limit: 4558)
     Memory: 8.2M
        CPU: 312ms
     CGroup: /system.slice/apache2.service
             ├─1234 /usr/sbin/apache2 -k start
             ├─1235 /usr/sbin/apache2 -k start
             └─1236 /usr/sbin/apache2 -k start

Oct 08 10:15:32 ubuntu systemd[1]: Starting apache2.service - The Apache HTTP Server...
Oct 08 10:15:32 ubuntu systemd[1]: Started apache2.service - The Apache HTTP Server.
```

| 項目 | 分かること |
| --- | --- |
| `Loaded` | サービスの定義ファイル（ユニットファイル）の場所と、OS の起動時に自動で起動するか。`enabled` なら自動起動する、`disabled` ならしない |
| `Active` | 今動いているかと、いつから動いているか。`active (running)` は動いている、`inactive (dead)` は停止中、`failed` は起動に失敗した状態 |
| `Main PID` / `CGroup` | Apache のプロセス番号と、動いているプロセスの一覧。`Memory` や `CPU` で使っている資源も分かる |
| 末尾のログ | 直近のログ数行。起動に失敗したときは、ここに原因のエラーメッセージが出ることが多い |

- `service` は、systemd が普及する前の SysV init の時代からあるコマンド。今の Ubuntu や RHEL では、`service apache2 status` を実行すると内部で `systemctl status apache2` が呼ばれ、同じ結果になる。自動起動の設定（`systemctl enable`）のように `systemctl` にしかない操作もあるため、新しく覚えるなら `systemctl` が基本になる。
- 出力が長いとページャー（`less`）で表示されるので、`q` で閉じる。`systemctl status apache2 --no-pager` なら、ページャーを使わずに表示する。
- ステータスに出るログは数行だけなので、それより前は `journalctl -u apache2` で確認する。アクセスやエラーの詳細は、`/var/log/apache2/`（Red Hat 系は `/var/log/httpd/`）のログを見る。

**`ps` コマンドでプロセスを確認する**

```bash
ps aux                  # すべてのプロセスを表示する
ps aux | grep apache2   # Apache のプロセスだけに絞る
```

| 場面 | 確認したいこと |
| --- | --- |
| サーバーが重いとき | どのプロセスが CPU やメモリを使っているか |
| 自分で起動したスクリプトや、`nohup` でバックグラウンドで動かしたプログラムを確認したいとき | まだ動いているか（systemd の管理外なので `systemctl` では見えない） |
| プロセスを止めたいとき | `kill` に渡すプロセス番号（PID）を調べる |
| どのユーザーで動いているかを確かめたいとき | Apache が `root` ではなく `www-data` で動いているかなど |
| コンテナの中など、systemd がない環境 | 何が動いているかを確認する |

Ubuntu で `ps aux | grep apache2` を実行したときの出力例は次のとおり（内容は一例）。

```
USER       PID %CPU %MEM    VSZ   RSS TTY   STAT START   TIME COMMAND
root      1234  0.0  0.2   6476  4700 ?     Ss   10:15   0:00 /usr/sbin/apache2 -k start
www-data  1235  0.0  0.3 752020  5320 ?     Sl   10:15   0:00 /usr/sbin/apache2 -k start
www-data  1236  0.0  0.3 752020  5320 ?     Sl   10:15   0:00 /usr/sbin/apache2 -k start
user      2001  0.0  0.1   6480  2300 pts/0 S+   12:20   0:00 grep --color=auto apache2
```

| 列 | 分かること |
| --- | --- |
| `USER` | プロセスを動かしているユーザー。Apache は、親プロセスが `root`、実際にリクエストを処理する子プロセスが `www-data`（Red Hat 系は `apache`）で動く |
| `PID` | プロセス番号。`kill 1235` のように、プロセスを止めるときに使う |
| `%CPU` / `%MEM` | CPU とメモリの使用率。重いプロセスを探すときに見る |
| `RSS` | 実際に使っているメモリの量（KB） |
| `TTY` | どの端末から起動したか。`?` は端末と結び付いていないプロセス（サービスなど） |
| `STAT` | プロセスの状態。`R` は実行中、`S` は待機中（多くのプロセスはこれ）、`Z` はゾンビ（終了したのに親プロセスに回収されていない）、`T` は一時停止中 |
| `START` / `TIME` | いつ起動したかと、これまでに使った CPU 時間 |
| `COMMAND` | 実行しているコマンド。どのプログラムをどんなオプションで起動したかが分かる |

- 最後の行は `grep` 自身で、`grep apache2` も「apache2 を含むコマンド」として表示される。`ps aux | grep [a]pache2` と書くか、`pgrep -a apache2` を使うと表示されない。
- CPU やメモリを多く使っている順に並べたいときは、`ps aux --sort=-%cpu | head` や `ps aux --sort=-%mem | head` を使う。
- `ps` は実行した瞬間の状態しか表示しないので、変化を見続けたいときは `top` を使う。
- `ps -ef` も全プロセスを表示する書き方で、`ps aux` とは表示される列が少し違うだけ。

参考: https://manpages.ubuntu.com/manpages/noble/man1/systemctl.1.html

参考: https://manpages.ubuntu.com/manpages/noble/man1/ps.1.html
:::

### 📌 パーミッションの読み方と数字の意味とは？
パーミッションは、「誰に」「何を許すか」を表す設定。`ls -l` で表示される `-rw-r--r--` のような10文字は、先頭の1文字がファイルの種類、残りの9文字が所有者・グループ・その他それぞれの r（読む）・w（書く）・x（実行する）の許可を表す。数字で書くときは r = 4、w = 2、x = 1 として3文字ごとに足し算し、`-rw-r--r--` は `644` になる。

![-rw-r--r--とdrwxr-xr-xを、種類・所有者・グループ・その他に色分けして1文字ずつ分解し、rを4、wを2、xを1、-を0として3文字ごとに足し算すると、それぞれ644と755になることを示した図](/images/books/book-record-of-reading/book065-permission.drawio.png)

:::details 🤖 Claude に相談した内容
**`ls -l` の表示の読み方**

```
$ ls -l /var/www/html
-rw-r--r-- 1 root root 10671 Oct  8 10:15 index.html
drwxr-xr-x 2 root root  4096 Oct  8 10:15 images
```

- 先頭の1文字は種類で、`-` はファイル、`d` はディレクトリ、`l` はシンボリックリンク。
- 残りの9文字は3文字ずつ、所有者 → グループ → その他の順に並ぶ。所有者とグループは、`ls -l` の3列目と4列目（上の例ではどちらも `root`）に表示されている。
- 各3文字は r → w → x の順に並び、許可されていない権限は `-` になる。

**r / w / x の意味（ファイルとディレクトリで違う）**

| 記号 | ファイルの場合 | ディレクトリの場合 |
| --- | --- | --- |
| `r`（read） | 中身を読める（`cat` など） | 中のファイルの一覧を見られる（`ls`） |
| `w`（write） | 中身を書き換えられる | 中にファイルを作る・削除する・名前を変えることができる |
| `x`（execute） | プログラムやスクリプトとして実行できる | 中に入れる（`cd`）、中のファイルにアクセスできる |

- ディレクトリの `x` がないと、`r` があっても中のファイルを開けない。そのため、ディレクトリには `r` と `x` をセットで付けるのが基本。

**数字の読み方**

- r = 4、w = 2、x = 1、`-` = 0 として、3文字ごとに足し算する。
- 数字は 0〜7 のどれかで、どの組み合わせも足し算の結果が重ならないため、数字から権限を一意に戻せる（例: 5 = 4 + 1 = `r-x`）。

| 数字 | 記号 | よく使う場面 |
| --- | --- | --- |
| `644` | `rw-r--r--` | 普通のファイル（HTML、設定ファイルなど）。所有者だけ書けて、ほかの人は読むだけ |
| `755` | `rwxr-xr-x` | ディレクトリや実行するスクリプト。所有者だけ書けて、ほかの人は読む・実行する（入る）だけ |
| `600` | `rw-------` | SSH の秘密鍵やパスワードを書いたファイル。所有者以外は読むことも禁止 |
| `700` | `rwx------` | 自分だけが使うディレクトリ（`~/.ssh` など） |
| `777` | `rwxrwxrwx` | 誰でも読み書き・実行できる。誰でも書き換えられてしまうため、原則として使わない |

**パーミッションを変えるには**

```bash
chmod 644 index.html                   # 数字で指定する
chmod u+x deploy.sh                    # 記号で指定する: 所有者（u）に実行（x）を追加
chmod go-w config.txt                  # グループ（g）とその他（o）から書き込み（w）を外す
sudo chown www-data:www-data upload/   # 所有者とグループを変える
```

- 記号で指定するときは、`u`（所有者）・`g`（グループ）・`o`（その他）・`a`（全員）と、`+`（追加）・`-`（削除）・`=`（その値にする）を組み合わせる。

**Apache の例で考える**

- Apache の子プロセスは `www-data`（Red Hat 系は `apache`）で動く。`/var/www/html/index.html` の所有者は `root` なので、`www-data` から見ると「その他」にあたる。
- `644` なら、その他にも `r` があるので Apache はファイルを読めて、ブラウザに返せる。
- `600` にすると、その他は読めなくなる。ブラウザには 403 Forbidden が返り、エラーログに `Permission denied` が記録される。
- 「表示されない」ときは、`ps` でどのユーザーで動いているかを確かめ、`ls -l` でそのユーザーが読めるパーミッションになっているかを見る。

**補足**

- 新しく作ったファイルは `644`、ディレクトリは `755` になることが多い。これは `umask`（初期値は多くの場合 `022`）が、最初の権限からグループとその他の `w` を外しているため。
- `chmod 1777` のように4桁で書く場合、先頭の桁は特殊な権限（setuid = 4、setgid = 2、スティッキービット = 1）を表す。例えば `/tmp` は `drwxrwxrwt` で、末尾の `t` がスティッキービット。誰でもファイルを作れるが、ほかの人のファイルは削除できない。

参考: https://manpages.ubuntu.com/manpages/noble/man1/chmod.1.html
:::

### 📌 なぜディレクトリの知識を覚える必要があるのか？
Linux では、ソフトウェアの種類を問わず「設定は `/etc`、ログは `/var/log`、プログラムは `/usr`」のように、置き場所の決まりがある。この決まりを知っていれば、初めて使うソフトウェアでも、設定を変えたいときやトラブルが起きたときに、どこを見ればよいか見当がつく。

:::details 🤖 Claude に相談した内容
置き場所の決まりは、FHS（Filesystem Hierarchy Standard）という標準で定められていて、多くのディストリビューションがこれに沿っている。

| ディレクトリ | 入っているもの | Apache の例 |
| --- | --- | --- |
| `/etc` | 設定ファイル | `/etc/apache2/`、`/etc/httpd/` |
| `/var` | 動いている間に変わるデータ（ログ、公開するファイル、キャッシュなど） | `/var/log/apache2/`、`/var/www/html/` |
| `/usr` | インストールしたプログラムやライブラリ | `/usr/sbin/apache2`、`/usr/sbin/httpd` |
| `/home` | ユーザーごとのファイル | ー |

ディレクトリの知識が役に立つ場面は次のとおり。

- **トラブルのときに原因を探せる**: 「サイトが表示されない」ときは、まず `/var/log` のエラーログを見る、という行動がすぐに取れる。ログの場所を知らないと、調べ始めることもできない。
- **初めてのソフトウェアでも迷わない**: Apache 以外の nginx や MySQL でも、設定は `/etc/nginx/` や `/etc/mysql/`、ログは `/var/log/` の下にある。1つ覚えれば、ほかのソフトウェアにも当てはめられる。
- **バックアップや移行の対象が分かる**: サーバーを作り直すときに残すべきなのは、主に `/etc` の設定と `/var` の中のデータ。`/usr` のプログラムは、インストールし直せば元に戻る。
- **ディストリビューションの違いに気付ける**: 上の Apache のように、同じソフトウェアでもディストリビューションによって場所や名前が違う。決まりを知っていれば、「Red Hat 系だから `/etc/httpd/` を探そう」と見当がつく。
- **AI の回答を確かめられる**: AI に聞くと、Debian 系の `/etc/apache2/` と Red Hat 系の `/etc/httpd/` が混ざった手順が返ってくることがある。ディレクトリの知識があれば、自分の環境に合っているかを判断できる。

参考: https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html
:::
