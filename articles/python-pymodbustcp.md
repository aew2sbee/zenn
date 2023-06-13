---
title: "【Python】ModbusTCP通信環境を構築する" # 記事のタイトル
emoji: "🥝" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "modbustcp", "pymodbustcp"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
PythonプロジェクトでModbusTCP通信環境が必要であり、
実現した経験があります。

その時にpyModbusTCPライブラリーに関する情報が少なく、大変でした。
今回実装方法を紹介して、今後の**誰かの開発の役に立てば良いな**と思います。

### 対象読者
- Pythonの基本文法を理解している方
- pyModbusTCPでModbusTCP通信環境を構築したい方
- サーバー/クライアント等の単語を理解している方

### この記事でわかること
- pyModbusTCPの基本的な使い方


### 前提条件
- Python 3.9.10
- pyModbusTCP 0.2.0

# 手順解説
## ライブラリーの準備
### 1. pyModbusTCPライブラリーをインストールする
下記コマンドでインストールする
```bash
pip install pyModbusTCP
```
### 2. pyModbusTCPライブラリーを確認する
`pyModbusTCP 0.2.0`がインストールされている事が確認できます。
```bash
$ pip show pyModbusTCP
Name: pyModbusTCP
Version: 0.2.0
Summary: A simple Modbus/TCP library for Python
Home-page: https://github.com/sourceperl/pyModbusTCP
Author: Loic Lefebvre
Author-email: loic.celine@free.fr
License: MIT
Location: /home/furuta/.local/lib/python3.9/site-packages
Requires:
Required-by:
```
## サーバー側の実装
### 1. ライブラリー等をインポートする
`ModbusServer`をインポートしてサーバー側に必要な関数等を使えるようにします。
`datetime`は、ログ用にインポートします。
```python
from pyModbusTCP.server import ModbusServer
from datetime import datetime
```

### 2. サーバー用のクラスを作成する

インポートした`ModbusServer`に、下記を引数として渡せるようにします。
- IPアドレス
- ポート
- ノンブロッキング動作の設定値

インスタンス化した`ModbusServer`、`IPアドレス`、`ポート`は、
クラス内部で使用したり、呼び出しためにselfで情報を保持します。
```python
class ModbusTCPServer():

    def __init__(self, ipaddress="127.0.0.1", port=12345):
        self.__ipaddress = ipaddress
        self.__port = port
        self.__server = ModbusServer(ipaddress, port, no_block=True)
        pass
```

:::message
サンプルコード用としてIPアドレス:`127.0.0.1`、ポート:`12345`を使用します。
プロジェクトに合わせてIPアドレスとポートを選択してください。
:::
#### 補足情報
- Q: ノンブロッキング動作(no_block)とは？
- A: 入出力処理の完了を待たずに次の処理を実行することができる非同期処理のことを指します。
このように、ノンブロッキング動作を行うことで、Modbusサーバーは他の処理を同時に実行することができ、パフォーマンスを向上させることができます。

- Q: no_block=Falseを指定すると？
- A: Modbusサーバーはブロッキング動作を行うようになります。
ブロッキング動作を行う場合、入出力処理が完了するまで次の処理を実行することができず、パフォーマンスが低下することがあります。


### 3. 状態を取得できる関数を作成する
`@property`を使って、下記をオブジェクトに対して値を取得できるように関数を作成します。
- タイムスタンプ
- サーバーのIPアドレス
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

### 4. サーバーを起動する関数の作成する
`ModbusServer`の`.start()`を使って任意のタイミングでサーバーを起動が出来るようにします。
サーバーが起動したら、**Start ModbusTCP Server**を出力します。

```python
    def start_server(self):
        """ サーバーを起動する """
        print(f"{self.get_nowtime} Start ModbusTCP Server")
        return self.__server.start()
```

### 5. サーバーを停止する関数の作成する
`ModbusServer`の`.stop()`を使って任意のタイミングでサーバーを停止が出来るようにします。
サーバーが起動したら、**Stop ModbusTCP Serve**を出力します。

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
**設定するアドレス**と**取得する値の数**の値を渡して保持レジスターの値を取得する。
その時の設定するアドレスと取得した値を出力させます。

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
- A: Modbusスレーブデバイスが持つ一連の16ビットのレジスターのことを指します。
スレーブデバイスに対して書き込み要求を送信することで書き込みができ、
また読み出し要求を送信することでその値を読み出すことができます。

- Q: 保持レジスターのアドレス範囲は？
- A: 0x0000から0xFFFFまでの65,536個のアドレスがあり、
各レジスターは0から65535の値を取ることができます。


### サーバー側のサンプルコード
先ほどの手順解説をまとめたコードになります。
```python: server.py

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

## クライアント側の実装
### 1. ライブラリー等をインポートする
`ModbusClient`をインポートしてクライアント側に必要な関数等を使えるようにします。
`datetime`は、ログ用にインポートします。
`time`は、スリープ用にインポートします。
```python
from pyModbusTCP.client import ModbusClient
from datetime import datetime
import time
```

### 2. クライアント用のクラスを作成する
インポートした`ModbusClient`にIPアドレス/ポートを引数として渡せるようにします。
インスタンス化した`ModbusClient`は、クラス内部で使用したり、呼び出しためにselfで情報を保持します。


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
デフォルトで10秒間 **接続**を監視する
接続が出来たら、**Connect: Success !!** と表示するようにする

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

### 5. サーバーへ切断する関数を作成する
デフォルトで10秒間 **切断**を監視する
接続が出来たら、**Disconnect: Success !!** と表示するようにする

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

### 6. 保持レジスターの値を取得する関数を作成する
取得したい保持レジスターのアドレスを指定して保持レジスターの値を取得する
```python
    def read_holding_registers(self, address):
        """ 保持レジスターの値を取得する """
        value = self.__client.read_holding_registers(address)
        print(f"{self.get_nowtime} [client] read_holding_registers[address:{address}]: {value}")
```

### クライアント側のサンプルコード
先ほどの手順解説をまとめたコードになります。
```python: client.py
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
        """ シングルレジスターに値を設定する """
        print(f"{self.get_nowtime} [client] write_single_register[address:{address}]: {value}")
        return self.__client.write_single_register(address, value)

    def write_multiple_registers(self, address, value: list):
        """ マルチレジスターに値を設定する """
        print(f"{self.get_nowtime} [client] write_multiple_registers[address:{address}]: {value}")
        return self.__client.write_multiple_registers(address, value)
```
## ModbusTCP通信環境の実装
### 1. 自作のクライアントとサーバーをインポートする
`time`は、サーバーとクライアントを十分に接続できるようにスリープさせるためにインポートします
```python
from server import ModbusTCPServer
from client import ModbusTCPClient
import time
```
### 2. 通信環境の準備する
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
### 4. 保持レジスターのアドレスの0番目に値を操作する
1. クライアント側でaddress=0の値が **[0]** の初期値である事を確認する
2. サーバー側でaddress=0の値に**23**を設定する。
3. クライアント側にデータが十分に反映されるまで待つ
4. クライアント側でaddress=0の値が **[23]** に更新されている事を確認する
```python
modbus_client.read_holding_registers(address=0)
modbus_server.set_holding_registers(address=0, value=23)
time.sleep(2)
modbus_client.read_holding_registers(address=0)
```
### 5. 保持レジスターのアドレスの1番目に値を操作する
`address=1`の保持レジスターでも同様な操作が可能か確認します
```python
modbus_client.read_holding_registers(address=1)
modbus_server.set_holding_registers(address=1, value=23)
time.sleep(2)
modbus_client.read_holding_registers(address=1)
```

### 6. 保持レジスターのアドレスの1番目に値を更新する
1. サーバー側でaddress=1の値に**25**を上書きする。
2. クライアント側にデータが十分に反映されるまで待つ
3. クライアント側でaddress=1の値が **[25]** に更新されている事を確認する
```python
modbus_server.set_holding_registers(address=1, value=25)
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

### サーバー側->クライアント側へサンプルコード
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

# クライアント側でaddress=0の値が[0]の初期値である事を確認する
modbus_client.read_holding_registers(address=0)
# サーバー側でaddress=0の値に23を設定する。
modbus_server.set_holding_registers(address=0, value=23)
# クライアント側にデータが十分に反映されるまで待つ
time.sleep(2)
# クライアント側でaddress=0の値が[23]に更新されている事を確認する
modbus_client.read_holding_registers(address=0)

# クライアント側でaddress=1の値が[0]の初期値である事を確認する
modbus_client.read_holding_registers(address=1)
# サーバー側でaddress=1の値に25を設定する。
modbus_server.set_holding_registers(address=1, value=25)
# クライアント側にデータが十分に反映されるまで待つ
time.sleep(2)
# クライアント側でaddress=1の値が[25]に更新されている事を確認する
modbus_client.read_holding_registers(address=1)

# サーバー側でaddress=1の値に25を上書きする。
modbus_server.set_holding_registers(address=1, value=20)
# クライアント側にデータが十分に反映されるまで待つ
time.sleep(2)
# クライアント側でaddress=1の値が[20]に更新されている事を確認する
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

### クライアント側→サーバー側へサンプルコード
似たような解説文が増える為、こちらの解説は行いません。
```python sample_client_to_server.py
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

# サーバー側でaddress=0の値が[0]の初期値である事を確認する
modbus_server.get_holding_registers(address=0)
# クライアント側でaddress=0の値を122に設定する
modbus_client.write_single_register(address=0, value=122)
# サーバー側にデータが十分に反映されるまで待つ
time.sleep(2)
# サーバー側でaddress=0の値が[122]である事を確認する
modbus_server.get_holding_registers(address=0)

# サーバー側でaddress=1の値が[0]の初期値である事を確認する
modbus_server.get_holding_registers(address=1)
# クライアント側でaddress=1の値を[1, 2, 3]に設定する
modbus_client.write_multiple_registers(address=1, value=[1, 2, 3])
# サーバー側にデータが十分に反映されるまで待つ
time.sleep(2)
# サーバー側でaddress=1の値が[1, 2, 3]である事を確認する
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
## おわりに
pyModbusTCPライブラリーでModbusTCP通信環境を構築しました。
pyModbusTCPライブラリーについて情報が少なく苦戦しましたが、自分の記事が参考になったら幸いです。

