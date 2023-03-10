# Supply Chain Dapp - Udacity Blockchain Nanodegree Project
This repo contains the solution to the Supply Chain Dapp project deployed to the Ethereum platform.  The project starter code used is from [here](https://github.com/udacity/nd1309-Project-6b-Example-Template).

- [Supply Chain Dapp - Udacity Blockchain Nanodegree Project](#supply-chain-dapp---udacity-blockchain-nanodegree-project)
  - [Overview](#overview)
  - [Getting Started](#getting-started)
    - [Prerquisites](#prerquisites)
    - [Installing](#installing)
  - [UML Diagrams](#uml-diagrams)
    - [Activity Diagram](#activity-diagram)
    - [Sequence Diagram](#sequence-diagram)
    - [State Diagram](#state-diagram)
    - [Class Diagram](#class-diagram)
  - [Built With](#built-with)
  - [Acknowledgments](#acknowledgments)
  
## Overview
The supply chain modeled in this project is for the process of harvesting coffee beans thru delivery to the end consumer.  More details are described in the sections below.

The completed and deployed app information:
| Key | Value |
|---|---|
| Goerli Network Contract Address | [0xd401AF0e681B846D20fa71003d68Cc589d1216B9](https://goerli.etherscan.io/address/0xd401AF0e681B846D20fa71003d68Cc589d1216B9)|
| Transaction ID | [0x1eb9b72458efe29e77817a6b8e7e6d4ab62f5ebed4a940682826182c6927e3b6](https://goerli.etherscan.io/tx/0x1eb9b72458efe29e77817a6b8e7e6d4ab62f5ebed4a940682826182c6927e3b6) |
| Truffle | v5.7.2 (core: 5.7.2) |
| Ganache |  v7.6.0 |
| Solidity | v0.5.16 (solc-js) |
| Node | v16.19.0 |
| Web3 | v1.8.1 |

## Getting Started
Instructions in this section will get a copy of this project set up and running on a local machine for development and testing as well as deploy to a the Goerli test network thrugh Infura.

### Prerquisites
You will need to install node.js with npm and install metamask in your browser.

### Installing
Clone this repository to a local folder:
```
git clone https://github.com/StuKozola/Udacity-Supply-Chain-Dapp
```
This code was developed and tested with Truffle v4.1.14 and Solidity v0.4.24. Install Truffle with Solidity:
```
npm i -g truffle@4.1.14
```
Install Ganache globally:
```
npm install ganache --global
```

Change the directory to `project-6` and install all required npm packages listed in package.json:
```
cd project-6
npm install
```
Launch Ganache:
```
ganache-cli -m "spirit supply whale amount human item harsh scare congress discover talent hamster"
```
or use the shell script
```
./1_start_ganache-cli.sh
```
Your terminal should look something like this:

![truffle test](images/ganache-cli.png)

In a separate terminal window, Compile smart contracts:

```
cd project-6
truffle compile
```

Your terminal should look something like this:

![truffle test](images/truffle_compile.png)

This will create the smart contract artifacts in folder ```build\contracts```.

Migrate smart contracts to the locally running blockchain, ganache-cli:

```
truffle migrate
```

Your terminal should look something like this:

![truffle test](images/truffle_migrate.png)

Test smart contracts:

```
truffle test
```

All 10 tests should pass.

![truffle test](images/truffle_test.png)

To deploy the contracts to the Goelib Public Network using Infura, create a `.env` file with the following:
```
INFURA_KEY=<YOUR INFURA PROJECT KEY FOR GOERLI>
MNEMONIC=<YOUR MNEMONIC FROM INFURA>
```
Then deploy with Truffle:
```
truffle migrate --reset --network goerli
```

In a separate terminal window, launch the DApp:

```
cd project-6
npm run dev
```

Open the app by navigating to [localhost:3000](http://localhost:3000/)

The DApp User Interface when running should look like...

![truffle test](images/ftc_product_overview.png)

![truffle test](images/ftc_farm_details.png)

![truffle test](images/ftc_product_details.png)

![truffle test](images/ftc_transaction_history.png)

## UML Diagrams
The design of this project is captured in the following diagrams.
###  Activity Diagram
4 actors in a coffee supply chain are:
* Farmer: The Farmer can harvest coffee beans, process coffee beans, pack coffee palettes, add coffee palettes, ship coffee palettes, and track authenticity.
* Distributor: The Distributor can buy coffee palettes and track authenticity.
* Retailer: The Retailer can receive coffee palettes and track authenticity.
* Consumer: The consumer can buy coffee palettes and track authenticity.

<!---
```plantuml
@startuml
|f|Farmer
start
:harvest coffee beans;
note: track authenticity
:process coffee beans;
note: track authenticity
:pack coffee palettes;
note: track authenticity
:add coffee palettes;
note: track authenticity
|d|Distributor
:buy coffee palettes;
note: track authenticity
|f|
:ship coffee palettes;
note: track authenticity
|r|Retailer
:receive coffee palettes;
note: track authenticity
|c|Consumer
:buy coffee;
note: track authenticity
end
@enduml
```
--->

![activity diagram](/images/activity_diagram.svg)

###  Sequence Diagram
The overall delivery of coffee beans to the end consumer is as follows:
* The farmer
  * harvests the coffee beans
  * processes the coffee beans
  * packs the coffee beans into palettes
  * lists the palettes for sale
* The distibutor
  * can buy a palette for sale from a farmer to have delievered to a retailer
  * if the distributor pays the right price, the farmer will ship the item to the reailer
* The retailer
  * will let the farmer know the coffee bean palette has been recieved
* The consumer
  * can purchase coffee beans from the retailer once the shipment from the farmer has been received
* All participants can track the state of the supply chain through data stored on the blockchain

<!---
```plantuml
@startuml
participant CoffeeBeans as b
participant Farmer as f
participant Distributor as d
participant Retailer as r
participant Consumer as c
activate b
b <- f: harvestItem()
activate f
b <- f: processItem()
b <- f: packItem()
b <- f: selltItem()
activate d
d -> f: buyItem()
activate r
deactivate d
f -> r: shipItem()
r->f: receiveItem()
activate c
deactivate f
c->r: purchaseItem()
deactivate r
deactivate c
b->c: fetchItem() buffer one
b->c: fetchItem() buffer two
@enduml
```
-->

![sequence diagram](/images/sequence_diagram.svg)

### State Diagram
The state of the coffee beans through the supply chain go from:
* Harvested by the farmer
* Processeed by the farmer
* Packed by the farmer
* ForSale by the famer
* Sold to a distributor
* Shipped to a retailer
* Received by a retailer
* Purchased by a consumer

<!---  
```plantuml
@startuml
state Coffee {
    [*] -down->  Harvested: isFarmer, Harvest
    Harvested -down-> Processed: isFarmer, isHarvested, Process
    Processed -down-> Packed: isFarmer, isProcessed, Pack
    Packed -down-> ForSale: isFarmer, isPacked, Sell
    ForSale -down-> Sold: isDistributor, isForSale, Buy
    Sold -down-> Shipped: isFarmer, isSold, Ship
    Shipped -down-> Received: isRetailer, isShipped, Receive
    Received -down-> Purchased: isConsumer, isRecieved, Purchase
    Purchased -down-> [*]
}
@enduml
```
-->

![state diagram](/images/state_diagram.svg)

### Class Diagram
The overall design of the contracts are captured in the class diagram below.

<!--
```plantuml
@startuml
Role -down-* Farmer
Role -down-*  Distributor
Role -down-* Retailer
Role -down-* Consumer
Farmer <-down- SupplyChain
Distributor <-down- SupplyChain
Retailer <-down- SupplyChain
Consumer <-down- SupplyChain
Ownable <-left- SupplyChain
abstract Role {
    add(role, address)
    remove(role, address)
    has(role, address): bool
}
class Farmer {
    farmers: Role
    FarmerAdded(address): Event
    FarmerRemoved(address): Event

    onlyFarmer()
    isFamer(address): bool
    addFarmer(adress)
    removeFarmer()
}
class Distributor {
    distributors: Role
    DistributorAdded(address): Event
    DistributorRemoved(address): Event

    onlyDistributor()
    isDistributor(address): bool
    addDistributor(address)
    removeDistributor()
}
class Retailer {
    retailers: Role
    RetailerAdded(address): Event
    RetailerRemoved(address): Event

    onlyDistributor()
    isRetailer(address): bool
    addRetailer(address)
    removeRetailer()
}
class Consumer {
    consumers: Role
    ConsumerAdded(address): Event
    ConsumerRemoved(address): Event

    onlyConsumer()
    isConsumer(address): bool
    addConsumer(address)
    removeConsumer()
}
struct Item {
    sku: uint
    upc: uint
    ownerID: address
    originFarmerID: address
    originFarmName: string
    originFarmInformation: string
    originFarmLatitude: string
    originFarmLongitude: string
    productID: uint
    productNotes: string
    productPrice: uint
    itemState: State
    distributorID: address
    retailerID: address
    consumerID: address
}
class SupplyChain {
    owner: address
    upc: uint
    sku: uint
    items: Item
    itemsHistory: string[]

    Harvested(uint): Event
    Processed(uint): Event
    Packed(uint): Event
    ForSale(uint): Event
    Sold(uint): Event
    Received(uint): Event
    Purchased(uint): Event

    harvestedItem()
    processItem()
    packItem()
    sellItem()
    buyItem()
    shipItem()
    receiveItem()
    purchaseItem()
    fetchItemBufferOne()
    fetchItemBufferTwo()
}
interface Ownable {
    owner: address
    TrasferOwnership(address, address): Event

    owner(): address
    onlyOwner()
    isOwner(): bool
    renosunceOwnership()
    transferOwnership(address)
}
@enduml
```
-->

![class diagram](/images/class_diagram.svg)

## Built With

* [Ethereum](https://www.ethereum.org/) - Ethereum is a decentralized platform that runs smart contracts
to make the web faster, safer, and more open.
* [Truffle Framework](http://truffleframework.com/) - Truffle is the most popular development framework for Ethereum with a mission to make your life a whole lot easier.

##  Acknowledgments

* Solidity
* Ganache-cli
* Truffle