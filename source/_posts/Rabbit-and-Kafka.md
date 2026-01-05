---
title: Rabbit and Kafka
date: 2022-05-30 20:17:02
tags:
  - Technology
categories:
  - Technology
  - MQ
cover: /images/cover.jpg  # 站内图片
---

# RabbitMQ VS Kafka
# Kafka vs RabbitMQ: Technical Comparison

| Feature | Apache Kafka | RabbitMQ |
|---------|--------------|----------|
| **Architecture Type** | Distributed commit log | Message broker |
| **Messaging Pattern** | Publish-Subscribe | Multiple patterns supported |
| **Message Ordering** | ✅ Per-partition ordering | ✅ Per-queue ordering |
| **Message Persistence** | ✅ Disk-based storage | ⚠️ Memory with optional disk |
| **Throughput** | 🚀 Very High (millions/sec) | ✅ High (thousands/sec) |
| **Latency** | ⚠️ Higher (ms range) | ✅ Lower (μs range) |
| **Protocol** | Custom binary protocol | AMQP 0-9-1 |
| **Data Replay** | ✅ Native support | ❌ Not supported |
| **Delivery Semantics** | At-least-once, Exactly-once | At-least-once, At-most-once |

```Java
// Kafka Consumer
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("group.id", "test-group");
props.put("key.deserializer", "StringDeserializer");
props.put("value.deserializer", "StringDeserializer");

KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("topic"));

// RabbitMQ Consumer
ConnectionFactory factory = new ConnectionFactory();
factory.setHost("localhost");
Connection connection = factory.newConnection();
Channel channel = connection.createChannel();
DeliverCallback deliverCallback = (consumerTag, delivery) -> {
    String message = new String(delivery.getBody(), "UTF-8");
};
channel.basicConsume("queue", true, deliverCallback, consumerTag -> { });
```