# mGate
mGate is an IoT API Gateway.

It is deployed in front of Mainflux system and can be used for authorization,
packet inspection and modification, logging and debugging and various other purposes.

## Usage
```bash
go get github.com/mainflux/mgate
cd $(GOPATH)/github.com/mainflux/mgate
make
./mgate
```

## Architecture
mProxy starts TCP, UDP and WS servers, offering connections to devices. Upon the connection, it establishes
a session with a remote MQTT and CoAP brokers.

Lorem ipsum...

## Deployment
mProxy does not do load balancing - just pure and simple proxying. This is why it should be deployed
right in front of some LB like NginX or Traefik that wil load-balance TCP and UDP packets.

## License
[Apache-2.0](LICENSE)
