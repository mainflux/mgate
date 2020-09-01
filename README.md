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
mGate is an API gateway that embeds [mProxy](https://github.com/mainflux/mproxy), which starts TCP, UDP and WS servers, offering connections to devices.
Upon the connection, it establishes a session with a remote MQTT and CoAP brokers.

To follow the highest security standards, embedded mProxy starts mTLS and DTLS connection and encryption handlers, and these can be tuned via configuraion. That way mGate can be used for TLS/DTLS termination.

<p align="center"><img src="docs/img/mgate.png"></p>

Here is the flow in more details:
- Device connects to mGate's TCP (for MQTT, WS and/or HTTP) or UDP (for CoAP) server
- mGate accepts the inbound (IN) connection and estabishes a new session with remote MQTT broker or CoAP server
(i.e. it dials out to MQTT broker / CoAP server only once it accepted new connection from a device.
This way one device-mProxy connection corresponds to one mGate-MQTT broker or mGate-CoAP server connection.)
- Every packet is inspected for authorization credentials (either via tokens or certificates), and requests are sent to Auth microservice. If authorized, then the packets are proxied to servers behind.
- Additionally, every packet is forwarded to NATS broker as well, which is an enterprise bus to which applications can be subscribed

## Deployment
mProxy does not do load balancing - just pure and simple proxying. This is why it should be deployed
right in front of some LB like NginX or Traefik that wil load-balance TCP and UDP packets.

## License
[Apache-2.0](LICENSE)
