# 👒
👞
🌂
💼
👝
👛
👓
☕️
🍵
🍶 Dappar 📊
📈
📉 Amis 🍫
🍬
🍭
🍯
🍎
🍋
🍒
🍇
🍉

After building out a handful of Dapps in React, I decided to carve out all the common components and provide them in an NPM package.

Should help get a Dapp up and running quickly so you can focus on your functionality, not wrestling with the blockchain and ethereum network.

Watch the demo video using Dapparamis over in the [Clevis repo](https://github.com/austintgriffith/clevis).

A good example and development walkthrough is
[nifties-vs-nfties](https://github.com/austintgriffith/nifties-vs-nfties/blob/master/README.md).

🍏
## install 

```
npm install --save dapparamis
```
👜
## import

```
import { Metamask, Gas, ContractLoader, Transactions, Events, Scaler, Blockie, Address, Button } from "dapparamis"
```

## usage
🎣
### DapparAmis

```javascript

const AMIS = {
  endpoint:"http://0.0.0.0:10001/",
  contract:"0x949bed886c739f1a3273629b3320db0c5024c719",
  //accountGenerator: "//account.github.io",
}
const WEB3_PROVIDER = 'http://0.0.0.0:8545'


<Dapparamis
  config={{
    DEBUG:false,
    requiredNetwork:['Unknown','Rinkeby'],
  }}
  replaceName={replaceName}
  amistx={AMIS}
  fallbackWeb3Provider={new Web3.providers.HttpProvider(WEB3_PROVIDER)}
  onUpdate={(state)=>{
   console.log("metamask state update:",state)
   if(state.web3Provider) {
     state.web3 = new Web3(state.web3Provider)
     this.setState(state)
   }
  }}
/>
```
        

### Metamask
🍊
Looks for injected web3 and provides an interface to the rest of the components. Also displays a nice HUD for users to see what account is logged in, what network they are on, and how much Ethereum they have. 

```javascript
<Metamask
  /*config={{requiredNetwork:['Ropsten']}}*/
  onUpdate={(state)=>{
    console.log("metamask state update:",state)
    if(state.web3Provider) {
      state.web3 = new Web3(state.web3Provider)
      this.setState(state)
    }
  }}
/>
```

### Gas

Keeps track of the best gas price in gwei and delivers it to other components.

```javascript
<Gas
  onUpdate={(state)=>{
    console.log("Gas price update:",state)
    this.setState(state,()=>{
      console.log("GWEI set:",this.state)
    })
  }}
/>
```

### Transactions

Displays transactions and blocks as progress bars and provides a **tx** function to make calling smart contract functions and sending transactions easier and more transparent to the user.

```javascript
<Transactions
  account={account}
  gwei={gwei}
  web3={web3}
  block={block}
  avgBlockTime={avgBlockTime}
  etherscan={etherscan}
  onReady={(state)=>{
    //loads in tx() function
    // use to send transactions: tx(contracts.YOURCONTRACT.YOURFUNCTION(),GASLIMIT)
    console.log("Transactions component is ready:",state)
    this.setState(state)
  }}
/>
```

## ContractLoader

Loads your contracts published from [Clevis](https://github.com/austintgriffith/clevis) into **this.state.contracts**.

```javascript
<ContractLoader
  web3={web3}
  require={path => {return require(`${__dirname}/${path}`)}}
  onReady={(contracts)=>{
    console.log("contracts loaded",contracts)
    this.setState({contracts:contracts})
  }}
/>
```

### Events

Listens for events and parses down the chain. Use an **id** field for unique keys so it will only fire the **onUpdate** function when a new event is detected. Provide a **filter** object to filter indexed fields.

```javascript
<Events
  config={{hide:false}}
  contract={contracts.Nifties}
  eventName={"Create"}
  block={block} <-------- current block number!
  id={"_id"}
  filter={{_owner:account}}
  onUpdate={(eventData,allEvents)=>{
    console.log("EVENT DATA:",eventData)
    this.setState({events:allEvents})
  }}
/>
```

### Address

Renders an address with the blockie (identicon) and the current balance in Eth. 

```javascript
  <Address
    {...this.state}
    address={contracts.SomeContract._address}
  />
```

### Button

Renders a button

```javascript
    <Button color={"green"} size={"2"} onClick={()=>{
        //do some transaction on button click
        tx(contracts.SomeContract.someFunction(someArgument),(receipt)=>{
          //when the transaction goes through you'll have a receipt here
        })
      }}>
      Send
    </Button>
```

### Blockie

Renders an identicon for an address

```javascript
    <Blockie 
      address={someEthereumAddress.toLowerCase()} 
      config={{size:3}}
     />
```


### Scaler

Scales components based on a target screen width vs actual screen width. Get your Dapp looking awesome on mobile.

```javascript
<Scaler config={{startZoomAt:1000,origin:"50px 50px",adjustedZoom:1.3}}>
  <img style={{position:"absolute",left:10,top:10,maxHeight:120,margin:10}} src={titleImage}/>
</Scaler>
```


### Demo App

Demoapp is a demo app that uses Dapparamis for Amis transactions:


