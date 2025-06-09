# Stock Market Data Streaming Platform using Kafka & AWS

This project demonstrates a real-time data pipeline that simulates stock market data, streams it via Apache Kafka, and ingests it into Amazon S3. Utilizing medatadata stored in AWS Glue Catalog and querying capabilities through Athena, it empowers scalable, on-demand analysis of high-velocity financial data - making modern market analytics accessible, reliable, and efficient.

## Data Architecture
<p align="center">
  <img src="https://github.com/user-attachments/assets/97bf3ad7-f862-4b46-83c9-fdcdc14bfb4c">
<h6 align = "center" > Source: Author </h6>
</p>

## Key Components & Value

1. **Stock Data Simulation**: Python-based simulation through (kafkaproducer.py) mimics real-world market events for continuous data flow testing and analytics prototyping.
2. **Kafka Producer**: Publishes simulated stock events to a Kafka topic, enabling real-time data streaming.
3. **Kafka Consumer**: Listens to market events and pushes raw, real-time data directly to Amazon S3, enabling robust and scalable storage.
4. **Data Storage**: Stores data in Amazon S3 in JSON format.
5. **Data Processing and Analysis**: AWS Glue crawlers instantly catalog new S3 data, while Athena provides interactive, serverless SQL analytics for rapid business decisions.

## Prerequisites

- AWS account with S3, EC2, and Glue services enabled with IAM role.
- Python 3.x installed.
- Apache Kafka installed on an EC2 instance.
- Kafka-Python library installed.

## Project Execution 

Firstly, We start the ```Zookeeper``` and the ```Kafka server```. After that create a topic in the Kafka Server then execute the producer command in the Kafka Server with the consumer command open in another window.

This will allow us the real time processing of the data after being fed to the producer. In our project the data was fed to the ```KafkaProducer.ipynb``` with the following code :
```
    while True:
        dict_stock = df.sample(1).to_dict(orient="records")[0]
        producer.send('test_topic1',value = dict_stock)
        sleep(1)
```
In the consumer file ```KafkaConsumer.ipynb``` the following code consume the data in real time :
```
    for count, i in enumerate(consumer):
      with s3.open("s3://kafka-stock-market-platform/data_{}.json".format(count), 'w') as file:
        json.dump(i.value, file)        
```

We installed the Kafka Server on the  ```Amazon EC2``` for the computation purpose.

After the above steps we stored our data on ```Amazon S3``` Bucket , in the form of ```data_*.json``` objects.

Later, with the help of crawler we scrape the data for extracting metadata and preparing a data catalog using Amazon Glue. 

In the end using ```Amazon Athena``` we can query our data iusing SQL queries on the data stored in our Amazon S3 Bucket. 
