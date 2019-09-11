# Apache Flink Basic


## Preface
New technologies come and go. Some are there to stay, to be used. Some are created to solve certain problems. 

What problem is Apache Flink solving? while there are some other stream processing tools such as [Apache Storm](https://storm.apache.org/), [Spark Streaming](https://spark.apache.org/streaming/) existed already. 

Of course, you can argue that some tools are created to solve the same problems as others, but they are better tools. I would argue, do we really know how `better` it is, do we really need the `better` features?

Keep in mind that while a new technology claims it has better features, most likely the existing technology will attempt to catch up with the new features or develop new future-proof features.

This is not something I try to discount Apache Flink. Those are questions I asked myself before I started working on a new technology.

## the Value of Real Time Stream Processing
The best use case for a new technology is, arguably, to create new economy. In the end, technology is a tool, economy value is the king. The parent company of Flink, [Ververica](https://www.ververica.com), use the following words to describe the value of real time stream processing:
> In a range of industries, customer interaction has evolved **from transactional and product centric to relationship based and services centric**

This is interesting, isn't it?

## Let's Talk About `Better`
[The key features of Apache Flink](https://www.ververica.com/) are claimed as
* Performance 
* Scalability
* Exactly-once Semantics

I have to add:
* Stateful

To me, it is fairly easy to understand the meaning of *performance*, *scalability*. How *Exactly-once Semantics* and *Stateful* matters?

*Exactly-once Semantics* - it basically means Flink will give accurate results to the data it processes. One use case is that the data arrives later for processing, Apache Flink is still able to process the data, and put whatever transformation or calculation in the right place. 

*Stateful*  - is another way to say that Apache Flink is able to persistent results forever. 

As a concrete scene, *Exactly-once* + *Stateful* will be super helpful for building your real-time data warehouse.

It is very important to remember that Apache Flink is scalable. Scalability definitely is crucial for real life applications.

What about other technologies such as *Storm*, or *Spark Streaming*?
Shamelessly, let me use this table, [stolen from Meituan's technology blog](https://tech.meituan.com/2017/11/17/flink-benchmark.html), list the differences between *Storm* and *Flink*. Keep in mind that the facts listed might be outdated.

|         | Storm           | Flink  |
| ------------- |:-------------:| -----:|
| Stateful      | No            | Yes   |
| Window      |  weak support     |   good support |
| Fault Tolerance   | Ack(http://storm.apache.org/releases/1.1.0/Guaranteeing-message-processing.html)      |    [Checkpoint](https://ci.apache.org/projects/flink/flink-docs-master/internals/stream_checkpointing.html#checkpointing) |
|                  |                       |


## the Journey of a Sample Data

