# RabbitMQ
- Message Broker: An Intermediate between person who is publishing and who is consuming
- Protocols: AMQP (more common, based on the TCP), MQTT, STOMP and HTTP
- Developed in the erlang language (too fast)
- Usually used for decouple services
- Fast and powerful. By default, the messages keeps in the memory
- It creates only one TCP connection. However, it creates channels inside this connection (called multiplexing connections). Each channel is one thread. 
- Path: Publisher -> Exchange (processes the message and discover which queue it will be sent) -> Queue -> Consumer

## Exchanges Types:
  1) Direct: send the message to exchange and the exchange send this message to determined queue (with the routing key)
  2) Fanout: send the message to the exchange and the exchange sends this message to all queue related.
  3) Topic: depending on the message type (routing key), we can send the message to the queue that we want. It can have rules. Ex: Regex in the routing key)
  4) Headers (rare to use): in the header of the message, we determine which queue it will be delivered.

## Queues:
```
  1) FIFO: By default, First-in First-out!
  2) Properties: 
    1) Durability:
      - Durable: if it has to be saved even after the broker restarts. 
      - Transient: if the machine turns down, the queue will not be available
    2) Auto-delete: remove the queue if the consumer disconnects.
    3) Expiry: if there is no client consuming this queue for X time, destroy.
    4) Message-TTL: Time to Live. If no one consumes the message during this time, the message will be removed from the queue.
    5) Overflow: 
      1) Drop-Head: remove the last message if the queue is full to receive a new one
      2) Reject publish: if the queue is full, the publisher will not be able to publish in the queue, and he will receive the rejection
    6) Exclusive: only the publishers that are connected in the same channel will be able to access this queue.
    7) Max length or bytes: max allowed per message or the quantity messages inside the queue in terms of bytes. You can decide to receive the new one, delete the last one, etc.
```

## Dead Letter Queues:
  1) some messages cannot be delivered for some reason
  2) they can be forwarded for a specific exchange that routes the traffic for a dead letter queue
  3) these messages can be consumed later. we can put some logs on it to comprehend later why it was not consumed. 

## Lazy Queues:
  1) Messages are saved on disks.
  2) high requirement in IO
  3) when we have millions of messages in a queue, for any reason, there is the possibility to free memory and send specific messages in the diskOBS:
  
## Reliability (Confiability):
```
  1) Consumer Acknowledgement: the consumer says: "I received!"
    - Basic Ack: the consumer says "ack", everything is ok, I received.
    - Basic Reject: the consumer rejects, throws an exception, or whatever. It sends the message back to the queue
    - Basic Nack: the same as the reject, but it can reject more than one message at the same time
  3) Publisher Confirm: everytime that the publisher sends a message, we are sure that it arrived at the exchange. the message has an id (the publisher gives the id, it is an integer.). If it did not arrive at the exchange, the exchange sends a “Nack” to the publisher
  4) Queues and Messages durable: can persists in disk (it becomes a little bit slow)
```

## OBS:
- Once the message is consumed by one consumer, the other consumer will not get it anymore, even if both are consuming the same queue
- RabbitMQ is scalable with more than one node.
- Virtual Hosts: A way to separate contexts. A Virtual Host does not talk with another Virtual Host.
- By default, the RabbitMQ comes with default exchanges (but it is good to use it only for testing). The correct way to do this is to create a new exchange to work properly.
- Simulator: http://tryrabbitmq.com/
