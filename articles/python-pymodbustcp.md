---
title: "[Python] ModbusTCP通信環境を構築する" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "modbustcp", "pymodbustcp"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Python プロジェクトで ModbusTCP 通信環境が必要であり、
実現した経験があります。

その時に pyModbusTCP ライブラリに関する情報が少なく、大変でした。
今回実装方法を紹介して、今後の**誰かの開発の役に立てば良いな**と思います。

### 対象読者

- Python の基本文法を理解している方
- pyModbusTCP で ModbusTCP 通信環境を構築したい方
- サーバー/クライアント等の単語を理解している方

### この記事でわかること

- pyModbusTCP の基本的な使い方

### 前提条件

- Python 3.9.10
- pyModbusTCP 0.2.0（0.2.0 で動作確認しています）

## 🌱 ライブラリの準備

### 1. pyModbusTCP ライブラリをインストールする

下記コマンドでインストールします。

```bash
pip install pyModbusTCP
```

### 2. pyModbusTCP ライブラリを確認する

`pyModbusTCP 0.2.0`がインストールされていることが確認できます。

```bash
$ pip show pyModbusTCP
Name: pyModbusTCP
Version: 0.2.0
Summary: A simple Modbus/TCP library for Python
Home-page: https://github.com/sourceperl/pyModbusTCP
Author: Loic Lefebvre
Author-email: loic.celine@free.fr
License: MIT
Location: /home/user/.local/lib/python3.9/site-packages
Requires:
Required-by:
```

## 🌱 サーバー側の実装

### 1. ライブラリ等をインポートする

`ModbusServer`をインポートしてサーバー側に必要な関数等を使えるようにします。
`datetime`は、ログ用にインポートします。

```python
from pyModbusTCP.server import ModbusServer
from datetime import datetime
```

### 2. サーバー用のクラスを作成する

インポートした`ModbusServer`に、下記を引数として渡せるようにします。

- IP アドレス
- ポート
- ノンブロッキング動作の設定値

インスタンス化した`ModbusServer`、`IPアドレス`、`ポート`は、
クラス内部で使用したり、呼び出すために self で情報を保持します。

```python
class ModbusTCPServer():

    def __init__(self, ipaddress="127.0.0.1", port=12345):
        self.__ipaddress = ipaddress
        self.__port = port
        self.__server = ModbusServer(ipaddress, port, no_block=True)
        pass
```

:::message
サンプルコード用として IP アドレス:`127.0.0.1`、ポート:`12345`を使用します。
プロジェクトに合わせて IP アドレスとポートを選択してください。
:::

:::message alert
Modbus/TCP は認証・暗号化のないプロトコルです。`0.0.0.0`やインターネット側から届くアドレスで公開せず、隔離したネットワーク内や、ファイアウォールで接続元を制限した環境で使ってください。
:::

#### 補足情報

- Q: ノンブロッキング動作(no_block)とは？
- A: `no_block=True`の場合、`start()`はサーバーを別スレッドで起動してすぐに戻ります。
  そのため、同じスクリプト内でクライアントの処理を続けられます。

- Q: no_block=False を指定すると？
- A: `no_block=False`（既定値）の場合、`start()`は`stop()`されるまで戻りません。
  本記事のように1つのスクリプトでサーバーとクライアントを動かす場合は、`no_block=True`が必要です。

### 3. 状態を取得できる関数を作成する

`@property`を使って、下記をオブジェクトに対して値を取得できるように関数を作成します。

- タイムスタンプ
- サーバーの IP アドレス
- サーバーのポート

```python
    @property
    def get_nowtime(self):
        """ ログ用のタイムスタンプ """
        return datetime.now()

    @property
    def get_ipaddress(self):
        """ サーバーのIPアドレスを取得する """
        return self.__ipaddress

    @property
    def get_port(self):
        """ サーバーのポートを取得する """
        return self.__port
```

### 4. サーバーを起動する関数を作成する

`ModbusServer`の`.start()`を使って任意のタイミングでサーバーを起動できるようにします。
サーバーが起動したら、**Start ModbusTCP Server**を出力します。

```python
    def start_server(self):
        """ サーバーを起動する """
        print(f"{self.get_nowtime} Start ModbusTCP Server")
        return self.__server.start()
```

### 5. サーバーを停止する関数を作成する

`ModbusServer`の`.stop()`を使って任意のタイミングでサーバーを停止できるようにします。
サーバーを停止する際に、**Stop ModbusTCP Server**を出力します。

```python
    def stop_server(self):
        """ サーバーを停止する """
        print(f"{self.get_nowtime} Stop ModbusTCP Server")
        return self.__server.stop()
```

### 6. 保持レジスターの値に関する関数を作成する

下記関数を作成します。

- 保持レジスターの値を設定する
- 保持レジスターの値を取得する

`set_holding_registers`は、
**設定するアドレス**と**設定する値**を渡して保持レジスターに設定します。
その時の設定するアドレスと設定する値を出力させます。

`get_holding_registers`は、
**取得するアドレス**と**取得する値の数**を渡して保持レジスターの値を取得します。
その時の取得するアドレスと取得した値を出力させます。

```python
    def get_holding_registers(self, address, num=1):
        """ 保持レジスターの値を取得する """
        value = self.__server.data_bank.get_holding_registers(address, number=num)
        print(f"{self.get_nowtime} [server] get_holding_registers[address:{address}]: {value}")
        return value

    def set_holding_registers(self, address, value):
        """ 保持レジスターの値を設定する """
        print(f"{self.get_nowtime} [server] set_holding_registers[address:{address}]: {[value]}")
        return self.__server.data_bank.set_holding_registers(address, [value])
```

#### 補足情報

- Q: 保持レジスターとは？
- A: Modbus スレーブデバイスが持つ一連の 16 ビットのレジスターのことを指します。
  スレーブデバイスに対して書き込み要求を送信することで書き込みができ、
  また読み出し要求を送信することでその値を読み出すことができます。

- Q: 保持レジスターのアドレス範囲は？
- A: 0x0000 から 0xFFFF までの 65,536 個のアドレスがあり、
  各レジスターは 0 から 65535 の値を取ることができます。

### サーバー側のサンプルコード

先ほどの手順解説をまとめたコードになります。

```python:server.py

from pyModbusTCP.server import ModbusServer
from datetime import datetime

class ModbusTCPServer():

    def __init__(self, ipaddress="127.0.0.1", port=12345):
        self.__ipaddress = ipaddress
        self.__port = port
        self.__server = ModbusServer(ipaddress, port, no_block=True)
        pass

    @property
    def get_nowtime(self):
        """ ログ用のタイムスタンプ """
        return datetime.now()

    @property
    def get_ipaddress(self):
        """ サーバーのIPアドレスを取得する """
        return self.__ipaddress

    @property
    def get_port(self):
        """ サーバーのポートを取得する """
        return self.__port

    def start_server(self):
        """ サーバーを起動する """
        print(f"{self.get_nowtime} Start ModbusTCP Server")
        return self.__server.start()

    def stop_server(self):
        """ サーバーを停止する """
        print(f"{self.get_nowtime} Stop ModbusTCP Server")
        return self.__server.stop()

    def get_holding_registers(self, address, num=1):
        """ 保持レジスターの値を取得する """
        value = self.__server.data_bank.get_holding_registers(address, number=num)
        print(f"{self.get_nowtime} [server] get_holding_registers[address:{address}]: {value}")
        return value

    def set_holding_registers(self, address, value):
        """ 保持レジスターの値を設定する """
        print(f"{self.get_nowtime} [server] set_holding_registers[address:{address}]: {[value]}")
        return self.__server.data_bank.set_holding_registers(address, [value])

```

## 🌱 クライアント側の実装

### 1. ライブラリ等をインポートする

`ModbusClient`をインポートしてクライアント側に必要な関数等を使えるようにします。
`datetime`は、ログ用にインポートします。
`time`は、スリープ用にインポートします。

```python
from pyModbusTCP.client import ModbusClient
from datetime import datetime
import time
```

### 2. クライアント用のクラスを作成する

インポートした`ModbusClient`に IP アドレス/ポートを引数として渡せるようにします。
インスタンス化した`ModbusClient`は、クラス内部で使用したり、呼び出すために self で情報を保持します。

```python
class ModbusTCPClient():

    def __init__(self, ipaddress="127.0.0.1", port=12345):
        self.__client = ModbusClient(host=ipaddress, port=port)
        pass
```

### 3. 状態を取得できる関数を作成する

`@property`を使って、下記をオブジェクトに対して値を取得できるように関数を作成します。

- タイムスタンプ
- サーバーとの接続状態

```python
    @property
    def get_nowtime(self):
        """ ログ用のタイムスタンプ """
        return datetime.now()

    @property
    def get_connect_state(self):
        """ サーバーとの接続状態を確認する """
        return self.__client.is_open
```

### 4. サーバーへ接続する関数を作成する

デフォルトで 10 秒間 **接続**を試みます。
接続を試みたら、**Connect: Success !!** と表示するようにします。

```python
    def connect_to_server(self, sec=10):
        """ サーバーとの接続を試みる """
        i = 0
        while i < sec:
            result = self.__client.open()
            print(f"{self.get_nowtime} Connect: Success !!")
            if result:
                break
            else:
                i += 1
                time.sleep(1)
```

### 5. サーバーから切断する関数を作成する

デフォルトで 10 秒間 **切断**を試みます。
切断を試みたら、**Disconnect: Success !!** と表示するようにします。

```python
    def disconnect_to_server(self, sec=10):
        """ サーバーとの切断を試みる """
        i = 0
        while i < sec:
            result = self.__client.close()
            if not result:
                print(f"{self.get_nowtime} Disconnect: Success !!")
                break
            else:
                i += 1
                time.sleep(1)
```

:::message alert
上記の接続・切断の関数は、成否を判定する前にメッセージを出力しています。また、pyModbusTCP 0.2.0 の`close()`は戻り値が`None`のため、切断の判定は常に1回目で成功になります。
実際に使う場合は、`if self.__client.open():`で判定してから出力する、切断後に`self.__client.is_open`で確認する、などの修正をしてください。
:::

### 6. 保持レジスターの値を取得する関数を作成する

取得したい保持レジスターのアドレスを指定して、保持レジスターの値を取得します。

```python
    def read_holding_registers(self, address):
        """ 保持レジスターの値を取得する """
        value = self.__client.read_holding_registers(address)
        print(f"{self.get_nowtime} [client] read_holding_registers[address:{address}]: {value}")
```

### クライアント側のサンプルコード

先ほどの手順解説をまとめたコードになります。

```python:client.py
from pyModbusTCP.client import ModbusClient
from datetime import datetime
import time


class ModbusTCPClient():

    def __init__(self, ipaddress="127.0.0.1", port=12345):
        self.__client = ModbusClient(host=ipaddress, port=port)
        pass

    @property
    def get_nowtime(self):
        """ ログ用のタイムスタンプ """
        return datetime.now()

    @property
    def get_connect_state(self):
        """ サーバーとの接続状態を確認する """
        return self.__client.is_open

    def connect_to_server(self, sec=10):
        """ サーバーとの接続を試みる """
        i = 0
        while i < sec:
            result = self.__client.open()
            print(f"{self.get_nowtime} Connect: Success !!")
            if result:
                break
            else:
                i += 1
                time.sleep(1)

    def disconnect_to_server(self, sec=10):
        """ サーバーとの切断を試みる """
        i = 0
        while i < sec:
            result = self.__client.close()
            if not result:
                print(f"{self.get_nowtime} Disconnect: Success !!")
                break
            else:
                i += 1
                time.sleep(1)

    def read_holding_registers(self, address):
        """ 保持レジスターの値を取得する """
        value = self.__client.read_holding_registers(address)
        print(f"{self.get_nowtime} [client] read_holding_registers[address:{address}]: {value}")

    def write_single_register(self, address, value):
        """ 保持レジスター1つに値を書き込む（FC06: Write Single Register） """
        print(f"{self.get_nowtime} [client] write_single_register[address:{address}]: {value}")
        return self.__client.write_single_register(address, value)

    def write_multiple_registers(self, address, value: list):
        """ 連続する複数の保持レジスターに値を書き込む（FC16: Write Multiple Registers） """
        print(f"{self.get_nowtime} [client] write_multiple_registers[address:{address}]: {value}")
        return self.__client.write_multiple_registers(address, value)
```

## 🌱 ModbusTCP 通信環境の実装

### 1. 自作のクライアントとサーバーをインポートする

`time`は、動作を目で追いやすくするためのスリープ用にインポートします。

:::message
Modbus はクライアントの要求にサーバーが応答する方式で、サーバーの値はその場で更新されます。そのため、Modbus の仕様上は`time.sleep()`による待機は不要です。本記事では、ログを目で追いやすくするために待機しています。
:::

```python
from server import ModbusTCPServer
from client import ModbusTCPClient
import time
```

### 2. 通信環境を準備する

インポートした自作クラスをインスタンス化し、通信環境を準備します。

```python
ipaddress = "127.0.0.1"
port = 12345

# インスタンス化
modbus_server = ModbusTCPServer(ipaddress=ipaddress, port=port)
modbus_client = ModbusTCPClient(ipaddress=ipaddress, port=port)
```

### 3. サーバーを起動後、接続する

```python
modbus_server.start_server()
modbus_client.connect_to_server()
```

### 4. 保持レジスターのアドレス0の値を操作する

1. クライアント側で address=0 の値が **[0]** の初期値であることを確認する
2. サーバー側で address=0 の値に **23** を設定する
3. 2秒待つ
4. クライアント側で address=0 の値が **[23]** に更新されていることを確認する

```python
modbus_client.read_holding_registers(address=0)
modbus_server.set_holding_registers(address=0, value=23)
time.sleep(2)
modbus_client.read_holding_registers(address=0)
```

### 5. 保持レジスターのアドレス1の値を操作する

`address=1`の保持レジスターでも同様の操作が可能か確認します。

```python
modbus_client.read_holding_registers(address=1)
modbus_server.set_holding_registers(address=1, value=25)
time.sleep(2)
modbus_client.read_holding_registers(address=1)
```

### 6. 保持レジスターのアドレス1の値を更新する

1. サーバー側で address=1 の値を **20** で上書きする
2. 2秒待つ
3. クライアント側で address=1 の値が **[20]** に更新されていることを確認する

```python
modbus_server.set_holding_registers(address=1, value=20)
time.sleep(2)
modbus_client.read_holding_registers(address=1)
```

### 7. サーバーとの切断後、終了する

`modbus_client.get_connect_state`で接続状態を確認しながらサーバーを終了させます。

```python
print(f"{modbus_client.get_nowtime} from client to server connected state:{modbus_client.get_connect_state}")
modbus_client.disconnect_to_server()
print(f"{modbus_client.get_nowtime} from client to server connected state:{modbus_client.get_connect_state}")
modbus_server.stop_server()
```

### サーバー側 → クライアント側のサンプルコード

先ほどの手順解説をまとめたコードになります。

```python:sample_server_to_client.py
from server import ModbusTCPServer
from client import ModbusTCPClient
import time

ipaddress = "127.0.0.1"
port = 12345

# インスタンス化
modbus_server = ModbusTCPServer(ipaddress=ipaddress, port=port)
modbus_client = ModbusTCPClient(ipaddress=ipaddress, port=port)

print(f"{modbus_client.get_nowtime} from client to server connected state:{modbus_client.get_connect_state}")
# modbusTCPサーバーを立ち上げる
modbus_server.start_server()
# サーバーへの接続を試みる
modbus_client.connect_to_server()
print(f"{modbus_client.get_nowtime} from client to server connected state:{modbus_client.get_connect_state}")

# クライアント側でaddress=0の値が[0]の初期値であることを確認する
modbus_client.read_holding_registers(address=0)
# サーバー側でaddress=0の値に23を設定する。
modbus_server.set_holding_registers(address=0, value=23)
# 2秒待つ
time.sleep(2)
# クライアント側でaddress=0の値が[23]に更新されていることを確認する
modbus_client.read_holding_registers(address=0)

# クライアント側でaddress=1の値が[0]の初期値であることを確認する
modbus_client.read_holding_registers(address=1)
# サーバー側でaddress=1の値に25を設定する。
modbus_server.set_holding_registers(address=1, value=25)
# 2秒待つ
time.sleep(2)
# クライアント側でaddress=1の値が[25]に更新されていることを確認する
modbus_client.read_holding_registers(address=1)

# サーバー側でaddress=1の値を20で上書きする。
modbus_server.set_holding_registers(address=1, value=20)
# 2秒待つ
time.sleep(2)
# クライアント側でaddress=1の値が[20]に更新されていることを確認する
modbus_client.read_holding_registers(address=1)

print(f"{modbus_client.get_nowtime} from client to server connected state:{modbus_client.get_connect_state}")
# サーバーとの切断を試みる
modbus_client.disconnect_to_server()
print(f"{modbus_client.get_nowtime} from client to server connected state:{modbus_client.get_connect_state}")
# modbusTCPサーバーを終了させる
modbus_server.stop_server()
```

出力結果を確認します。

```bash
$ python sample_server_to_client.py
2023-02-21 14:32:42.986585 from client to server connected state:False

2023-02-21 14:32:42.986683 Start ModbusTCP Server
2023-02-21 14:32:42.992917 Connect: Success !!

2023-02-21 14:32:42.993132 from client to server connected state:True

2023-02-21 14:32:42.996868 [client] read_holding_registers[address:0]: [0]
2023-02-21 14:32:44.999293 [server] set_holding_registers[address:0]: [23]
2023-02-21 14:32:47.002003 [client] read_holding_registers[address:0]: [23]

2023-02-21 14:32:47.002562 [client] read_holding_registers[address:1]: [0]
2023-02-21 14:32:47.002656 [server] set_holding_registers[address:1]: [25]
2023-02-21 14:32:49.005167 [client] read_holding_registers[address:1]: [25]

2023-02-21 14:32:49.005256 [server] set_holding_registers[address:1]: [20]
2023-02-21 14:32:51.007852 [client] read_holding_registers[address:1]: [20]

2023-02-21 14:32:51.008022 from client to server connected state:True
2023-02-21 14:32:51.008166 Disconnect: Success !!

2023-02-21 14:32:51.008322 from client to server connected state:False
2023-02-21 14:32:51.008598 Stop ModbusTCP Server
```

### クライアント側 → サーバー側のサンプルコード

似たような解説文が増えるため、こちらの解説は行いません。

```python:sample_client_to_server.py
from server import ModbusTCPServer
from client import ModbusTCPClient
import time

ipaddress = "127.0.0.1"
port = 12345

# インスタンス化
modbus_server = ModbusTCPServer(ipaddress=ipaddress, port=port)
modbus_client = ModbusTCPClient(ipaddress=ipaddress, port=port)

print(f"{modbus_client.get_nowtime} from client to server connected state:{modbus_client.get_connect_state}")
# modbusTCPサーバーを立ち上げる
modbus_server.start_server()
# サーバーへの接続を試みる
modbus_client.connect_to_server()
print(f"{modbus_client.get_nowtime} from client to server connected state:{modbus_client.get_connect_state}")

# サーバー側でaddress=0の値が[0]の初期値であることを確認する
modbus_server.get_holding_registers(address=0)
# クライアント側でaddress=0の値を122に設定する
modbus_client.write_single_register(address=0, value=122)
# サーバー側にデータが十分に反映されるまで待つ
time.sleep(2)
# サーバー側でaddress=0の値が[122]であることを確認する
modbus_server.get_holding_registers(address=0)

# サーバー側でaddress=1の値が[0]の初期値であることを確認する
modbus_server.get_holding_registers(address=1)
# クライアント側でaddress=1の値を[1, 2, 3]に設定する
modbus_client.write_multiple_registers(address=1, value=[1, 2, 3])
# サーバー側にデータが十分に反映されるまで待つ
time.sleep(2)
# サーバー側でaddress=1の値が[1, 2, 3]であることを確認する
modbus_server.get_holding_registers(address=1, num=3)

print(f"{modbus_client.get_nowtime} from client to server connected state:{modbus_client.get_connect_state}")
# サーバーとの切断を試みる
modbus_client.disconnect_to_server()
print(f"{modbus_client.get_nowtime} from client to server connected state:{modbus_client.get_connect_state}")
# modbusTCPサーバーを終了させる
modbus_server.stop_server()
```

出力結果を確認します。

```bash
$ python sample_client_to_server.py
2023-02-21 14:20:21.656062 from client to server connected state:False

2023-02-21 14:20:21.656305 Start ModbusTCP Server
2023-02-21 14:20:21.664870 Connect: Success !!

2023-02-21 14:20:21.665610 from client to server connected state:True

2023-02-21 14:20:21.665801 [server] get_holding_registers[address:0]: [0]
2023-02-21 14:20:21.665876 [client] write_single_register[address:0]: 122
2023-02-21 14:20:23.668568 [server] get_holding_registers[address:0]: [122]

2023-02-21 14:20:23.668771 [server] get_holding_registers[address:1]: [0]
2023-02-21 14:20:23.668844 [client] write_multiple_registers[address:1]: [1, 2, 3]
2023-02-21 14:20:25.671624 [server] get_holding_registers[address:1]: [1, 2, 3]

2023-02-21 14:20:25.671815 from client to server connected state:True
2023-02-21 14:20:25.672126 Disconnect: Success !!

2023-02-21 14:20:25.672307 from client to server connected state:False
2023-02-21 14:20:25.672378 Stop ModbusTCP Server
```

## 🌱 おわりに

pyModbusTCP ライブラリで ModbusTCP 通信環境を構築しました。
pyModbusTCP ライブラリについて情報が少なく苦戦しましたが、自分の記事が参考になったら幸いです。
