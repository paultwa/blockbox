# blockbox


peer chaincode install -o orderer0:7050 -n marbles -v 1.0 -p
github.com/hyperledger/fabric/examples/chaincode/go/marbles02

peer chaincode instantiate -o orderer0:7050 --tls $CORE_PEER_TLS_ENABLED --cafile
/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/cacerts/ca.example.com-cert.pem -C mychannel -n marbles -v 1.0 -p
github.com/hyperledger/fabric/examples/chaincode/go/marbles02 -c '{"Args":["init"]}' -P "OR
('Org0MSP.member','Org1MSP.member')"


peer chaincode invoke -o orderer0:7050 --tls $CORE_PEER_TLS_ENABLED --cafile
/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/cacerts/ca.example.com-cert.pem -C mychannel -n marbles -c '{"Args":["initMarble","marble1","blue","35","tom"]}'


peer chaincode invoke -o orderer0:7050 --tls $CORE_PEER_TLS_ENABLED --cafile
/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/cacerts/ca.example.com-cert.pem -C mychannel -n marbles -c '{"Args":["initMarble","marble2","red","50","tom"]}'


peer chaincode invoke -o orderer0:7050 --tls $CORE_PEER_TLS_ENABLED --cafile
/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/cacerts/ca.example.com-cert.pem -C mychannel -n marbles -c '{"Args":["initMarble","marble3","blue","70","tom"]}'

peer chaincode invoke -o orderer0:7050 --tls $CORE_PEER_TLS_ENABLED --cafile
/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/cacerts/ca.example.com-cert.pem -C mychannel -n marbles -c '{"Args":["transferMarble","marble2","jerry"]}'

peer chaincode invoke -o orderer0:7050 --tls $CORE_PEER_TLS_ENABLED --cafile
/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/cacerts/ca.example.com-cert.pem -C mychannel -n marbles -c '{"Args":["transferMarblesBasedOnColor","blue","jerry"]}'


peer chaincode invoke -o orderer0:7050 --tls $CORE_PEER_TLS_ENABLED --cafile
/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/cacerts/ca.example.com-cert.pem -C mychannel -n marbles -c '{"Args":["delete","marble1"]}'

http://localhost:5984/_utils


peer chaincode query -C mychannel -n marbles -c '{"Args":["readMarble","marble2"]}'
