helm install my-release oci://registry-1.docker.io/bitnamicharts/nats --values k8s/bitnami/values.yaml

helm install nats oci://harbor.rvision.pro/charts/nats --version 1.2.4 -f k8s/values.yaml


`make`

Testing the Clusters
Now, the following should work: make a subscription on one of the nodes and publish it from another node. You should be able to receive the message without problems.

`docker run --network nats --rm -it natsio/nats-box`

`nats sub -s nats://nats:4222 hello & nats pub -s "nats://nats-1:4222" hello first`

`nats pub -s "nats://nats-2:4222" hello second`

```console
691b03275aa2:~# nats sub -s nats://nats:4222 hello &
691b03275aa2:~# nats pub -s "nats://nats-1:4222" hello first18:20:36 Subscribing on hello 

18:20:37 Published 5 bytes to "hello"
[#1] Received on "hello"
first

691b03275aa2:~# nats pub -s "nats://nats-2:4222" hello second
18:20:43 Published 6 bytes to "hello"
[#2] Received on "hello"
second
```

## Stopping the seed node, which received the subscription, should trigger an automatic failover to the other nodes:

`make stop`

## Publishing again will continue to work after the reconnection:

`nats pub -s "nats://nats-1:4222" hello again`

`nats pub -s "nats://nats-2:4222" hello again`

`make start`
