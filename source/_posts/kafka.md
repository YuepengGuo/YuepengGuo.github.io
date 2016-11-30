---
title: kafka
date: 2016-09-29 10:17:20
tags: 实时,工具
---
# Apache Kafka Cookbook

### Storage internals
Kafka's storage unit is a partition, a partition is an ordered, immutable sequences that are appended to.
A partition cannot be split across multiple brokers or even multiple disks.

Partitions are split into segments to regularly find the messages on disk that need purged.

When Kafka writes to a partition, it writes to a segment — the active segment. If the segment’s size limit is reached, a new segment is opened and that becomes the new active segment.

Segments are named by their base offset. The base offset of a segment is an offset greater than offsets in previous segments and less than or equal to offsets in that segment.

On disk a partition is a directory and each segment is an index file and a log file.

Segments logs are where messages are stored
Each message is its value, offset, timestamp, key, message size, compression codec, checksum, and version of the message format.
The data format on disk is exactly the same as what the broker receives from the producer over the network and sends to its consumers. This allows Kafka to efficiently transfer data with zero copy.

 A smaller size means that files must be closed and allocated more often, which reduces the overall efficiency of disk writes.

 This means that smaller log segments will provide more accurate answers for offset requests by timestamp.

```
$ bin/kafka-run-class.sh kafka.tools.DumpLogSegments --deep-iteration --print-data-log --files /data/kafka/events-1/00000000003065011416.log | head -n 4
Dumping /data/kafka/appusers-1/00000000003065011416.log
Starting offset: 3065011416
offset: 3065011416 position: 0 isvalid: true payloadsize: 2820 magic: 1 compresscodec: NoCompressionCodec crc: 811055132 payload: {"name": "Travis", msg: "Hey, what's up?"}
offset: 3065011417 position: 1779 isvalid: true payloadsize: 2244 magic: 1 compresscodec: NoCompressionCodec crc: 151590202 payload: {"name": "Wale", msg: "Starving."}
```
Segment indexes map message offsets to their position in the log
The segment index maps offsets to their message’s position in the segment log.

The index file is memory mapped, and the offset look up uses binary search to find the nearest offset less than or equal to the target offset.

The index file is made up of 8 byte entries, 4 bytes to store the offset relative to the base offset and 4 bytes to store the position. The offset is relative to the base offset so that only 4 bytes is needed to store the offset. For example: let’s say the base offset is 10000000000000000000, rather than having to store subsequent offsets 10000000000000000001 and 10000000000000000002 they are just 1 and 2.

Kafka wraps compressed messages together
Producers sending compressed messages will compress the batch together and send it as the payload of a wrapped message. And as before, the data on disk is exactly the same as what the broker receives from the producer over the network and sends to its consumers.

KAFKA CONSUMERS AND ZOOKEEPER
Prior to Apache Kafka 0.9.0.0, consumers, in addition to the brokers, utilized Zookeeper, to directly store information about the composition of the consumer group, what topics it was consuming, and to periodically commit offsets for each partition being consumed (to enable failover between consumers in the group). With version 0.9.0.0, a new consumer interface was introduced that allows this to be managed directly with the Kafka brokers.

