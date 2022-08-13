# RabbitMQ
- Message Broker: An Intermediare between person who is publishing and who is consuming
- Protocols: AMQP (more common, based on the TCP), MQTT, STOMP and HTTP
- Developed in the erlang language (too fast)
- Usually used for decouple services
- Fast and powerful. By default, the messages keeps in the memory
- It creates only one connection TCP. However, it create channels inside this connection (called multiplexing connections). Each channel is one thread. 
- Path: Publisher -> Exchange (processes the message and discover which queue it will be sent) -> Queue -> Consumer

- Exchanges Types:
  1) Direct: send the message to exchange and the exchange send this message to determined queue (with the routing key)
  2) Fanout: send the message to exchange and the exchange send this message to all queue related.
  3) Topic: depending of the message type (rounting key), we can send the message to queue that we want. It can have rules. Ex: Regex in the routing key)
  4) Headers (rare to use): in the header of the message, we determine which queue it will be delivered.

- Queues:
  1) FIFO: By default, First-in First-out!
  2) Properties: 
    1) Durable: if it has to be saved even after the broker restart
    2) Auto-delete: remove the queue if the consumer disconnects.
    3) Expiry: if there is no client consuming this queue for X time, destroy.
    4) Message-TTL: Time to Live. If no one consumes the message during this time, the message will be removed from the queue.
    5) Overflow: 
      1) Drop-Head: remove the last message if the queue is full to receive a new one
      2) Reject publish: if the queue is full, the publisher wil not be able to publish in the queue, and he will receive the rejection
    6) Exclusive: only the publishers that are connected in the same channel will be able to access this queue.
    7) Max length or bytes: max allowed per message or the quantitity messages inside the queue in terms of bytes. You can decide decide to receive the new one, delete the last one, etc.
    
- Dead Letter Queues:
  1) 
