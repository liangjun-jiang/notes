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
| Stream Window      |  weak support     |   good support |
| Fault Tolerance   | Ack(http://storm.apache.org/releases/1.1.0/Guaranteeing-message-processing.html)      |    [Checkpoint](https://ci.apache.org/projects/flink/flink-docs-master/internals/stream_checkpointing.html#checkpointing) |


## a Sample Use Case
Let's walk through some possible use cases of Flink stream processing. The example is originally written by Zhong Wu, published [here](https://mp.weixin.qq.com/s/U23xaOrxlg4h8hYPgMZrHg).

Let's assume you are running a ride share company at New York City, you are interested in understanding the following metrics:
0. We want to understand car counts of having 1 passenger, 2 passengers, 3 passengers, etc
1. In 5 minutes, how many cars are entering individual area of the city? 
2. In 10 minutes, how many passengers are riding? 
For sure you can add your cases to fullfil your role. For example, you don't want to just look at cold numbers, you actually want to use a web application to visualize those metrics. 

Will those metrics be helpful for you to run your business? For a ride sharing economy, without doubt, real time information is everything. 
If you run an e-commerce website, will real-time information be matter that much? Alibaba company uses stream processing to adjust their search recommendations in special occasions such as Nov. 11 - the promotion day. The reason is that users will search the products having most discount in that day. The personalized recommendation will not work.

So you have the basic `rides` data model, 
| rideId | carId| isStart | lon| lat  | psgCount
| ------ |:-----:| -----:|:-----:| -----:| -----:|
| Long  | Long   | Yes   | Float | Float  | Integer |







