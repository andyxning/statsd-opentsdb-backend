## Notice
This repo is targeted for [NSQ](https://github.com/nsqio/nsq) + [OpenTSDB](https://github.com/OpenTSDB/opentsdb).

According to [NSQ doc](http://nsq.io/components/nsqd.html#a-namestatsdstatsd--graphite-integrationa), we can use [statsd](https://github.com/etsy/statsd) to collect nsqd metrics about topic, channel and go GC and send them to an OpenTSDB backend.

However, the problem is that metric names nsqd emits are mainly targeted for [Graphite](https://graphiteapp.org/). They are dot-separated. By using this metric format, we can not do any aggregation with OpenTSDB.

Thus, we customize nsqd metrics to OpenTSDB and make them taged.

## Assumptions
* **Both nsq topic and channel contain no dots event though [nsq officially support it](http://nsq.io/clients/tcp_protocol_spec.html#a-namenotesnotesa)**.
* **The value for `--statsd-prefix` has the pattern of `nsq_cluster_<CLUSTER_NAME>`. Tag key `cluster`'s value is `CLUSTER_NAME`**.

## TagKV
|TagKey|TagValue|
|---|---|
|nsqd| nsqd_host_nsqd_port |
|topic| topic name |
|channel| channel name |
|cluster| cluster name |

## metrics
|Metrics|
|---|
|nsq.topic.backend_depth|
|nsq.topic.depth|
|nsq.topic.message_count|
|nsq.channel.backend_depth|
|nsq.channel.clients|
|nsq.channel.deferred_count|
|nsq.channel.depth|
|nsq.channel.in_flight_count|
|nsq.channel.message_count|
|nsq.channel.requeue_count|
|nsq.channel.timeout_count|
|nsq.mem.heap_objects|
|nsq.mem.heap_idle_bytes|
|nsq.mem.heap_in_use_bytes|
|nsq.mem.heap_released_bytes|
|nsq.mem.gc_pause_usec_100|
|nsq.mem.gc_pause_usec_99|
|nsq.mem.gc_pause_usec_95|
|nsq.mem.mem.next_gc_bytes|
|nsq.mem.gc_runs|
|`nsq.topic.e2e_processing_latency_<percent>`|
|`nsq.channel.e2e_processing_latency_<percent>`|

## Overview
This is a pluggable backend for [StatsD](https://github.com/etsy/statsd), which
publishes stats to OpenTSDB (http://opentsdb.net)

## Installation

    npm install statsd-opentsdb-backend

## Configuration
You have to give basic information about your OpenTSDB server to use
```
{ opentsdbHost: 'localhost'
, opentsdbPort: 4242
, opentsdbTagPrefix: '_t_'
}
```

## Tag support
This backend allows you to attach OpenTSDB tags to your metrics. To add a counter
called `gorets` and tag the data `foo=bar`, you'd write the following to statsd:

    gorets._t_foo.bar:261|c

## Dependencies
- none

## Development
- [Bugs](https://github.com/emurphy/statsd-opentsdb-backend/issues)

## Issues
If you want to contribute:

1. Clone your fork
2. Hack away
3. If you are adding new functionality, document it in the README
4. Push the branch up to GitHub
5. Send a pull request
