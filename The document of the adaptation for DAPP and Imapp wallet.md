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

##  DAPP will not change the process after adapting to the imApp through the above two changes. It only adds the watchword parameter to the scatter identify interface. The DAPP and imApp delivery timing diagram as following.

<img src="https://github.com/imApp-Core/imApp/blob/master/src/imapp1.png" width=800 height=400 />  

<img src="https://github.com/imApp-Core/imApp/blob/master/src/imapp2.png" width=600 height=200 />
