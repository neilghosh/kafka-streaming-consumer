This is an example use of Kafka streaming API in Java where multiple instance of this program monitors for new message in a specific topic in kafka and handles. The messages publiched to (produced) the Kafka topic is distributed across multiple consumers running as part of the instance. Same message is not consumed by more than one consumers because the consumers are defined as part of same consumer group in the application.yaml.

This also demonstrates it can be used alternative to the Google Cloud Pub/Sub's push subscription mode.

# Steps

1.   Install and run Apache Kafka
     
     ```
     brew install kafka
     cd /usr/local/Cellar/kafka/1.1.0/bin
     zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties & kafka-server-start /usr/local/etc/kafka/server.properties 
     ```
     Troubleshooting - sometimes server.properties may have to contain the localhost listener explicitly i.e. listeners=PLAINTEXT://localhost:9092

2.  run multiple instances of this program with following (it will always start with a random port     to avoid conflict if you are running in the same machine.
    ```
    mvn spring-boot:run
    ```

3.  Publish the message using command line or API
    
    Command Line 

    ```
    $ /usr/local/Cellar/kafka/1.1.0/bin/kafka-console-producer --broker-list localhost:9092 --topic greetings
    ```
    API

    ```
    $ curl http://localhost:<PORT>/greetings?message=hello
    ```
        
    The `<PORT>` can be found from any of the consoles while running the program.

4. Verify (from the consol log) that the messeges are going to only one of the instance of the  program not all. For debugging you can also consume from the command line as follows to see all the messages that are getting published. 
    ```
    $ /usr/local/Cellar/kafka/1.1.0/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic greetings --from-beginning
    ```
Reference - https://dzone.com/articles/kafka-streams-more-than-just-dumb-storage