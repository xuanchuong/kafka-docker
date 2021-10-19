1. Set up a Kafka broker

2. Start the Kafka broker

From a directory containing the docker-compose.yml file created in the previous step, run this command to start all services in the correct order.

docker-compose up -d

3. Create a topic

Kafka stores messages in topics. It’s good practice to explicitly create them before using them, even if Kafka is configured to automagically create them when referenced.

Run this command to create a new topic into which we’ll write and read some test messages.
```
docker exec broker \
kafka-topics --bootstrap-server broker:9092 \
             --create \
             --topic quickstart
```
4. Write messages to the topic

You can use the kafka-console-producer command line tool to write messages to a topic. This is useful for experimentation (and troubleshooting), but in practice you’ll use the Producer API in your application code, or Kafka Connect for pulling data in from other systems to Kafka.

Run this command. You’ll notice that nothing seems to happen - fear not! It is waiting for your input.
```
docker exec --interactive --tty broker \
kafka-console-producer --bootstrap-server broker:9092 \
                       --topic quickstart
```
Type in some lines of text. Each line is a new message.
```
this is my first kafka message
hello world!
this is my third kafka message. I’m on a roll :-D
```

When you’ve finished, press Ctrl-D to return to your command prompt.

5. Read messages from the topic

Now that we’ve written message to the topic, we’ll read those messages back. Run this command to launch the kafka-console-consumer. The --from-beginning argument means that messages will be read from the start of the topic.
```
docker exec --interactive --tty broker \
kafka-console-consumer --bootstrap-server broker:9092 \
                       --topic quickstart \
                       --from-beginning
```
As before, this is useful for trialling things on the command line, but in practice you’ll use the Consumer API in your application code, or Kafka Connect for reading data from Kafka to push to other systems.

You’ll see the messages that you entered in the previous step.
```
this is my first kafka message
hello world!
this is my third kafka message. I’m on a roll :-D
```
6. Write some more messages

Leave the kafka-console-consumer command from the previous step running. If you’ve already closed it, just re-run it.

Now open a new terminal window and run the kafka-console-producer again.
```
docker exec --interactive --tty broker \
kafka-console-producer --bootstrap-server broker:9092 \
                       --topic quickstart
```
Enter some more messages and note how they are displayed almost instantaneously in the consumer terminal.

Press Ctrl-D to exit the producer, and Ctrl-C to stop the consumer.

7. Stop the Kafka broker

Once you’ve finished, you can shut down the Kafka broker. Note that doing this will destroy all messages in the topics that you’ve written.

From a directory containing the docker-compose.yml file created earlier, run this command to stop all services in the correct order.
```
docker-compose down
```