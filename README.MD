# JDBC Sink connector

This is a project of sample standalone Apache Kafka Connect JDBC connector container.

## Quickstart

Clone repository locally.

```bash
git clone https://github.com/GTdevteamGT/kafka-jdbc
```

Configure it in `.env` variables file.
 
Start the stack with `docker-compose`

```bash
docker-compose up
```

Use `http://localhost:8000` to access GUI for Kafka Connect.
Add connector using "New" button. Then choose "Jdbc" connector in "Sinks" section.
Fill the configuration with the sample below and substitute variables:
- username - kafka user name
- password - kafka user password
- schema-servers - urls list of registry
- schema-auth - authentication for registry in format `schema-username:schema-password`
- input-topic-name - topic name to consume
- destination-table-name - destination table name to write to

```properties
name=router
connector.class=io.confluent.connect.jdbc.JdbcSinkConnector
topics=<input-topic-name>
tasks.max=1
connection.url=jdbc:mysql://db:3306/sink
connection.user=user
connection.password=password
value.converter=io.confluent.connect.avro.AvroConverter
key.converter=io.confluent.connect.avro.AvroConverter
key.converter.schema.registry.url=<schema-servers>
value.converter.schema.registry.url=<schema-servers>
key.converter.basic.auth.credentials.source=USER_INFO
key.converter.schema.registry.basic.auth.user.info=<schema-auth>
value.converter.basic.auth.credentials.source=USER_INFO
value.converter.schema.registry.basic.auth.user.info=<schema-auth>
consumer.override.sasl.mechanism=PLAIN
consumer.override.security.protocol=SASL_SSL
consumer.override.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="<username>" password="<password>";
consumer.override.group.id=<username>
table.name.format=<destination-table-name>
auto.create=true
insert.mode=upsert
pk.mode=record_value
pk.fields=transactionId
```

Then select the created connector in the GUI and press "Restart".
