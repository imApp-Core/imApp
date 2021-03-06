# The document of the adaptation for DAPP and Imapp wallet

## All DAPPs that support the scatter web can be adapted to the imApp wallet with the following changes.

#### （1）	Add the password parameter watchword to the identify interface. DAPP takes the watchword in the result of the identify, and the DAPP obtains the authorization code as followed:
```javascript
    let result = await ScatterJS.scatter.getIdentity({accounts:[this.network]})      
    this.currentAccount = result.accounts[0];  
    this.watchword = result.accounts[0]['watchword'];  //imApp’s new param，DAPP needs to save watchword
```

#### （2）	When the DAPP initiated transaction (EOS or other token transactions), the password obtained from the scatter.getIdentity interface needs to be filled in the transaction memo.
    For example, the password that imApp passes to DAPP through the Scatter-JS identify interface is:  [imapp2832F2B86F]
    1.the transstion before adapt to the imAPP：
        imappbcdefgh → onedex123451
        0.1000 EOS (eosio.token)
        MEMO
        {
          "from": "imappbcdefgh",
          "to": "onedex123451",
          "quantity": "0.1000 EOS",
          "memo": ""
        }

    2.the transstion after adapt to the imAPP : add the password of imapp at memo
      imappbcdefgh → onedex123451
      0.1000 EOS (eosio.token)
      MEMO
      {
        "from": "imappbcdefgh",
        "to": "onedex123451",
        "quantity": "0.1000 EOS",
        "memo": "[imapp2832F2B86F]"
      }
#### (3) After DAPP is adapted, if account token amount is needed. You need to access the imApp new interface "getBalance()".
```javascript
 For example, the DAPP  needs to obtain the current account (EOS/FC/DICE) and other token balances, which can be returned in the following format
let balance = await ScatterJS.scatter.getBalance({"symbol" :["eosio.token-eos","fcfundadmins-fc","betdicetoken-dice"]})
//Among them, eosio.token-eos, fcfundadmins-fc, and beticetoken-dice are the [contract account name-token name] corresponding to the token.
 if Success, balance==
{
  "eosio.token-eos":"0.2300",
  "fcfundadmins-fc":"3542.0000",
  "betdicetoken-dice":"99.2100"
}
```
<img src="https://github.com/imApp-Core/imApp/blob/master/src/imapp3_en.png" width=400 height=650 />

##  DAPP will not change the process after adapting to the imApp through the above two changes. It only adds the watchword parameter to the scatter identify interface. The DAPP and imApp delivery timing diagram as following.

<img src="https://github.com/imApp-Core/imApp/blob/master/src/imapp1_en.jpg" width=700 height=400 />  

<img src="https://github.com/imApp-Core/imApp/blob/master/src/imapp2_en.jpg" width=500 height=200 />
