<!-- SPDX-License-Identifier: LGPL-3.0-only -->
<!-- SPDX-FileCopyrightText: Copyright (C) 2026 mplx <mplx@jennifer-lang.dev> -->

# statsd cheatsheet

Every export of `@jennifer/statsd`. The guide is [the introduction](index.md).

```jennifer
import "@jennifer/statsd/" as statsd;
```

## Functions

| Function | Returns | Does |
|---|---|---|
| `statsd.client(host as string)` | `Client` | Open a client to a StatsD agent on `host` at the default port (8125), with no metric prefix |
| `statsd.clientWith(address as string, prefix as string)` | `Client` | Open a client to a StatsD agent at a full `host:port` address, with a metric-name prefix ("" for none) |
| `statsd.count(c as Client, name as string, value as int)` |  | Adjust a counter by `value` (may be negative) |
| `statsd.increment(c as Client, name as string)` |  | Increment a counter by 1 |
| `statsd.decrement(c as Client, name as string)` |  | Decrement a counter by 1 |
| `statsd.gauge(c as Client, name as string, value as int)` |  | Set a gauge to an absolute value |
| `statsd.timing(c as Client, name as string, ms as int)` |  | Record a timing in milliseconds |
| `statsd.set(c as Client, name as string, value as string)` |  | Record a unique member in a set (the agent counts distinct values) |
| `statsd.countRate(c as Client, name as string, value as int, rate as float)` |  | Adjust a counter by `value` at a sample `rate` |
| `statsd.timingRate(c as Client, name as string, ms as int, rate as float)` |  | Record a timing in milliseconds at a sample `rate` |
| `statsd.countTagged(c as Client, name as string, value as int, tags as map of string to string)` |  | Adjust a counter by `value`, carrying DogStatsD tags |
| `statsd.incrementTagged(c as Client, name as string, tags as map of string to string)` |  | Increment a counter by 1, carrying DogStatsD tags |
| `statsd.decrementTagged(c as Client, name as string, tags as map of string to string)` |  | Decrement a counter by 1, carrying DogStatsD tags |
| `statsd.gaugeTagged(c as Client, name as string, value as int, tags as map of string to string)` |  | Set a gauge to an absolute value, carrying DogStatsD tags |
| `statsd.timingTagged(c as Client, name as string, ms as int, tags as map of string to string)` |  | Record a timing in milliseconds, carrying DogStatsD tags |
| `statsd.setTagged(c as Client, name as string, value as string, tags as map of string to string)` |  | Record a unique set member, carrying DogStatsD tags |
| `statsd.countFloat(c as Client, name as string, value as float)` |  | Adjust a counter by a fractional `value` |
| `statsd.gaugeFloat(c as Client, name as string, value as float)` |  | Set a gauge to a fractional absolute value (e.g. a load average) |
| `statsd.batch(c as Client)` | `Batch` | Start an empty `Batch`, capturing the client's metric-name prefix |
| `statsd.addCount(b as Batch, name as string, value as int)` | `Batch` | Append a counter delta to the batch |
| `statsd.addIncrement(b as Batch, name as string)` | `Batch` | Append a counter +1 to the batch |
| `statsd.addDecrement(b as Batch, name as string)` | `Batch` | Append a counter -1 to the batch |
| `statsd.addGauge(b as Batch, name as string, value as int)` | `Batch` | Append an absolute gauge to the batch |
| `statsd.addTiming(b as Batch, name as string, ms as int)` | `Batch` | Append a timing (milliseconds) to the batch |
| `statsd.addSet(b as Batch, name as string, value as string)` | `Batch` | Append a unique set member to the batch |
| `statsd.flush(c as Client, b as Batch)` |  | Send every accumulated line, joined by "\n", in as few UDP datagrams as fit 1432 bytes each (more would be fragmented, and a fragmented datagram is often dropped whole); the split falls between lines |
| `statsd.close(c as Client)` |  | Close the client's sending socket |

## Types

### `Client`

A StatsD client.

| Field | Meaning |
|---|---|
| `socket as net.UDPSocket` | the sending socket |
| `address as string` | the agent "host:port" |
| `prefix as string` | a metric-name namespace ("" for none) |

### `Batch`

An accumulator of formatted metric lines to pack into UDP datagrams (the StatsD multi-metric packet, lines separated by "\n").

| Field | Meaning |
|---|---|
| `prefix as string` | the metric-name namespace copied from the client |
| `lines as list of string` | the accumulated wire lines |
