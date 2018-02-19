# To run chaincode in hyperledger

## Prerequisites
Install [hyperledger fabric](https://hyperledger-fabric.readthedocs.io/en/release/prereqs.html) and fabric samples

```
$ curl -sSL https://goo.gl/byy2Qj | bash -s 1.0.5
$ git clone https://github.com/hyperledger/fabric-samples.git
```

set GOPATH and PATH
```
$ export GOPATH=$HOME/go
$ export PATH=$PATH:$GOPATH/bin
$ export PATH=<path to download location>/bin:$PATH
```

 
## Building the chaincode
```
$ go get -u --tags nopkcs11 github.com/hyperledger/fabric/core/chaincode/shim
$ go build --tags nopkcs11
```

## Install Hyperleder Fabric Samples
```
$ cd chaincode-docker-devmode
```

## Start Terminal 1 - Start the network
```
$ docker-compose -f docker-compose-simple.yaml up
```

## Start Terminal 2 - Build and start the chaincode
```
$ docker exec -it chaincode bash
$ cd sacc
$ go build
$ CORE_PEER_ADDRESS=peer:7051 CORE_CHAINCODE_ID_NAME=mycc:0 ./sacc
```
## Start Terminal 3 - Use the Chaincode
```
$ docker exec -it cli bash
$ peer chaincode isntall -p chaincode/sacc -n mycc -v 0
$ peer chaincode instantiate -n mycc -v 0 -c '{"Args":["a","10"]}' -C myc
```

## Invoke
```
$ peer chaincode invoke -n mycc -c '{"Args":["set", "a", "20"]}' -C myc
```

## Query
```
$ peer chaincode query -n mycc -c '{"Args":["query", "a"]}' -C myc
```
