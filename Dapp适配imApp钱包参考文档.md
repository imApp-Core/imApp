# DAPP 适配到imApp钱包参考文档

## 所有支持scatter web插件钱包的DAPP可以通过以下三点改动适配到imApp钱包

#### （1）在identify接口中新增口令参数watchword。DAPP在identify的result中取出watchword， 如下DAPP获取授权结果代码:
```javascript
    let result = await ScatterJS.scatter.getIdentity({accounts:[this.network]})      
    this.currentAccount = result.accounts[0];  
    this.watchword = result.accounts[0]['watchword'];  //imApp新增的参数，DAPP保存watchword
```

#### （2）DAPP发起的交易（EOS和所有代币的交易）时，需要把在scatter.getIdentity接口中获取的口令填入到交易memo中.
    例如imApp通过Scatter-JS identify接口传递给DAPP的口令是：  [imapp2832F2B86F]
    1.适配到imApp之前的交易：
        代币转账
        imappbcdefgh → onedex123451
        0.1000 EOS (eosio.token)
        MEMO
        {
          "from": "imappbcdefgh",
          "to": "onedex123451",
          "quantity": "0.1000 EOS",
          "memo": ""
        }

    2.适配到imApp之后的交易：在memo中增加了imapp的口令
      代币转账
      imappbcdefgh → onedex123451
      0.1000 EOS (eosio.token)
      MEMO
      {
        "from": "imappbcdefgh",
        "to": "onedex123451",
        "quantity": "0.1000 EOS",
        "memo": "[imapp2832F2B86F]"
      }
#### （3）DAPP适配后，获取账户代币金额，需要访问imApp新增接口getBalance()
```javascript
例如：DAPP方需要获取当前账户(EOS/FC/DICE)等代币余额，可按如下格式
let balance = await ScatterJS.scatter.getBalance({"symbol" :["eosio.token-eos","fcfundadmins-fc","betdicetoken-dice"]})
//其中eosio.token-eos、fcfundadmins-fc、betdicetoken-dice为代币对应的[合约账户名称-代币名称]
若返回成功，则balance=
{
  "eosio.token-eos":"0.2300",
  "fcfundadmins-fc":"3542.0000",
  "betdicetoken-dice":"99.2100"
}
```

#### （4）DAPP的EOS账号显示：用imApp口令替换原有的EOS账号。

<img src="https://github.com/imApp-Core/imApp/blob/master/src/imapp3.jpg" width=400 height=650 />  

##  DAPP通过上述三点改动适配到imApp后不会改变原来流程，只是在scatter identify接口中增加了watchword参数，以下是DAPP 与 imApp部分交付时序图.

<img src="https://github.com/imApp-Core/imApp/blob/master/src/imapp1.png" width=800 height=400 />  

<img src="https://github.com/imApp-Core/imApp/blob/master/src/imapp2.png" width=600 height=200 />
