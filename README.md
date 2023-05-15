## Integrate Apache Kafka Connect support on Azure Event Hubs for Change Data Capture

This project walks through how to set up a change data capture based system on Azure using Event Hubs (for Kafka), MSSQL and Debezium|JDBC. It will use the Debezium-MSSQL or JDBC-MSSQL connector to stream database modifications from MSSQL Server to Kafka topics in Event Hubs.

### Pre-requisites
- Azure subscription
- Linux
- Kafka release, available from [kafka.apache.org](https://kafka.apache.org/downloads) 


#### Configure Kafka Connect for Event Hubs

The following [connect-standalone.properties](/kafka_2.13-3.4.0/config/connect-standalone.properties) sample illustrates how to configure Connect to authenticate and communicate with the Kafka endpoint on Event Hubs


#### Download and setup Debezium|JDBC connector

Connectors are packaged as Kafka Connect plugins. Kafka Connect isolates each plugin so that the plugin libraries do not conflict with each other.

To manually install a connector:
1. Find your connector on Confluent Hub and download the connector ZIP file.
2. Extract the ZIP file contents and copy the contents to the desired location. 
3. Add this to the plugin path in your Connect properties file.

The following [plugins](/kafka_2.13-3.4.0/plugins) sample contains two plugins: debezium-2.1 and jdbc-10.7.1

Then, create a configuration file [connect-debezium-mssql.properties](/kafka_2.13-3.4.0/config/connect-debezium-mssql.properties) for the Debezium-MSSQL source connector and [connect-jdbc-mssql-sink.properties](/kafka_2.13-3.4.0/config/connect-debezium-mssql.properties) for the Jdbc-MSSQL sink connector.


#### Run Kafka Connect

```bash
> cd kafka_2.13-3.4.0/
> bin/connect-standalone.sh config/connect-standalone.properties config/connect-debezium-mssql.properties config/connect-jdbc-mssql-sink.properties
```

> **_NOTE:_** List all available Kafka Connect plugins
>
>```bash
>> curl -s -XGET http://localhost:8083/connector-plugins|jq '.[].class'
>````

> **_NOTE:_** List all available Kafka Connect worker
>
>```bash
>> curl -H "Accept:application/json" localhost:8083/connectors
>````
