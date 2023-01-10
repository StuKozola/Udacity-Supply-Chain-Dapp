# Supply chain & data auditing

This repository containts an Ethereum DApp that demonstrates a Supply Chain flow between a Seller and Buyer. The user story is similar to any commonly used supply chain process. A Seller can add items to the inventory system stored in the blockchain. A Buyer can purchase such items from the inventory system. Additionally a Seller can mark an item as Shipped, and similarly a Buyer can mark an item as Received.

The DApp User Interface when running should look like...

![truffle test](images/ftc_product_overview.png)

![truffle test](images/ftc_farm_details.png)

![truffle test](images/ftc_product_details.png)

![truffle test](images/ftc_transaction_history.png)


## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

Please make sure you've already installed ganache-cli, Truffle and enabled MetaMask extension in your browser.

```
Give examples (to be clarified)
```

### Installing

> The starter code is written for **Solidity v0.4.24**. At the time of writing, the current Truffle v5 comes with Solidity v0.5 that requires function *mutability* and *visibility* to be specified (please refer to Solidity [documentation](https://docs.soliditylang.org/en/v0.5.0/050-breaking-changes.html) for more details). To use this starter code, please run `npm i -g truffle@4.1.14` to install Truffle v4 with Solidity v0.4.24. 

A step by step series of examples that tell you have to get a development env running

Clone this repository:

```
git clone https://github.com/udacity/nd1309/tree/master/course-5/project-6
```

Change directory to ```project-6``` folder and install all requisite npm packages (as listed in ```package.json```):

```
cd project-6
npm install
```

Launch Ganache:

```
ganache-cli -m "spirit supply whale amount human item harsh scare congress discover talent hamster"
```

Your terminal should look something like this:

![truffle test](images/ganache-cli.png)

In a separate terminal window, Compile smart contracts:

```
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

In a separate terminal window, launch the DApp:

```
npm run dev
```

## Built With

* [Ethereum](https://www.ethereum.org/) - Ethereum is a decentralized platform that runs smart contracts
* [IPFS](https://ipfs.io/) - IPFS is the Distributed Web | A peer-to-peer hypermedia protocol
to make the web faster, safer, and more open.
* [Truffle Framework](http://truffleframework.com/) - Truffle is the most popular development framework for Ethereum with a mission to make your life a whole lot easier.


## Authors

See also the list of [contributors](https://github.com/your/project/contributors.md) who participated in this project.

## Acknowledgments

* Solidity
* Ganache-cli
* Truffle
* IPFS

## Activity Diagram
```plantuml
@startuml
|f|Farmer
start
:harvest coffee beans;
note: track authenticity
:process coffee bans;
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

## Sequence Diagram
```plantuml
@startuml
participant Coffee as b
participant Farmer as f
participant Distributor as d
participant Retailer as r
participant Consumer as c
activate b
b -> f: harvestItem()
activate f
b -> f: processItem()
b -> f: packItem()
b -> f: selltItem()
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
## State Diagram
```plantuml
@startuml
state Coffee {
    [*] -right->  Harvested
    Harvested -right-> Processed
    Processed -right-> Packed
    Packed -right-> ForSale
    ForSale -right-> Sold
    Sold -right-> Shipped
    Shipped -right-> Received
    Received -right-> Purchased
    Purchased -right-> [*]
}

[*] -right->Farmer: harvestIItem
Farmer -down-> Harvested: harvestItem


@enduml
```

## Class Diagram
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
