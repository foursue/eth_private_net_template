# Private Net Template

このリポジトリはEthereumのプライベートネットを立ち上げるときのテンプレートです。

## Usage

使い方

### ブロックチェーンネットワークの初期化

```
geth --datadir ./ init ./myGenesis.json
```

#### ブロックチェーンネットワークの起動

##### コンソール付きで起動

ネットワークIDはmyGenesis.jsonに記載されてるものを使いましょう。

```
geth --networkid 15 --nodiscover --datadir ./ console 2>> ./geth_err.log
```

##### コンソールなしで起動

ネットワークIDはmyGenesis.jsonに記載されてるものを使いましょう。

```
geth --networkid 15 --nodiscover --datadir ./ 2>> ./geth_err.log &
```

```
geth --datadir ./ attach ipc:./geth.ipc

```

### アカウント(EOA)の作成

gechでコンソールにつないで下記コマンドでEOAが作れます。
※パスワードは長く複雑なものにしてください。

```
> personal.newAccount("passwd")
> eth.accounts
["0xd3dfe313d2a0e02f4a5b40cde5b95f1d9754bd71"]
```

### マイニング

マイニングする前にマイニング報酬がどのEOAに紐付いているか確認します。空だった場合はEOAが登録されていないかもしれないので、確認してみてください。

```
> eth.coinbase
"0xd3dfe313d2a0e02f4a5b40cde5b95f1d9754bd71"
```

次のコマンドでマイニングが始まります。

```
> miner.start()
null
> eth.blockNumber
0
> eth.mining
true
```

マイニングを止める場合は次のコマンドを実行します

```
> miner.stop()
true
> eth.mining
false
```

マイニング報酬は次のように確認できます
※ 1 ether = 1,000,000,000,000,000,000 wei

```
> eth.blockNumber // => 現在のブロック
97
> eth.getBalance(eth.accounts[0]) // #=> wei単位での口座残高の表示
485000000000000000000
> web3.fromWei(eth.getBalance(eth.accounts[0]),"ether") // #=> ETH単位での口座残高の表示
485
```

### EOA間で送金する

まず、送金元のEOAをアンロックします。※パスワードが求められます。

```
> personal.unlockAccount(eth.accounts[0])
Unlock account 0xd3dfe313d2a0e02f4a5b40cde5b95f1d9754bd71
Passphrase: 
true
```

10 ether送ってみます。返ってくるのは取引アドレスです。

```
> eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[1], value: web3.toWei(10, "ether")})
"0x6be68942e0e03b61e3d1e4aff911f266cf2aa719903919c2feda8fe191b2fcfe"
```

取引が完了すると、下記のように送信先のアドレスに10 etherが付与されます。

```
> eth.getBalance(eth.accounts[1])
10000000000000000000
```


## Config

ネットワークIDを自由に設定できます

```
vi myGenesis.json
=====================
"config": {
  "chainId": 15
},
=====================
```

ネットワークIDを変えた場合は起動時の `networkid` もそれに合わせましょう

