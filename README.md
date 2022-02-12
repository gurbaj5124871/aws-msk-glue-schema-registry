# aws-msk-glue-schema-registry

`@kafkajs/confluent-schema-registry` is a library that makes it easier to interact with the AWS Glue schema registry, it provides convenient methods to encode, decode and register new schemas using the Apache Avro serialization format and Confluent's [wire format](https://docs.confluent.io/current/schema-registry/docs/serializer-formatter.html#wire-format).

## Getting started

```sh
npm install @kafkajs/confluent-schema-registry
# yarn add @kafkajs/confluent-schema-registry
```

```javascript
const { Kafka } = require('kafkajs')
const { SchemaRegistry } = require('@kafkajs/confluent-schema-registry')

const kafka = new Kafka({ clientId: 'my-app', brokers: ['kafka1:9092'] })
const registry = new SchemaRegistry({ host: 'http://registry:8081/' })
const consumer = kafka.consumer({ groupId: 'test-group' })

const run = async () => {
  await consumer.connect()
  await consumer.subscribe({ topic: 'test-topic', fromBeginning: true })

  await consumer.run({
    eachMessage: async ({ topic, partition, message }) => {
      const decodedKey = await registry.decode(message.key)
      const decodedValue = await registry.decode(message.value)
      console.log({ decodedKey, decodedValue })
    },
  })
}

run().catch(console.error)
```

## Documentation

## License

See [LICENSE](https://github.com/kafkajs/confluent-schema-registry/blob/master/LICENSE) for more details.
