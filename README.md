# @jennifer/statsd

[![test](https://github.com/jennifer-language/deck-statsd/actions/workflows/test.yml/badge.svg)](https://github.com/jennifer-language/deck-statsd/actions/workflows/test.yml)
[![Documentation](https://img.shields.io/badge/docs-github%20pages-blue)](https://jennifer-language.github.io/deck-statsd/)
[![release](https://img.shields.io/github/v/release/jennifer-language/deck-statsd)](https://github.com/jennifer-language/deck-statsd/releases/latest)

A [Jennifer](https://jennifer-lang.dev/) deck that pushes metrics to a
StatsD agent (StatsD, Datadog's DogStatsD, Telegraf) over UDP: counters,
gauges, timings and sets, with sample rates, DogStatsD tags and batching.
Fire-and-forget: a metric is one datagram and never blocks the program.

It was Jennifer's bundled `statsd.j` module until 0.27; the surface is the same,
so moving over changes only the import:

```jennifer
import "@jennifer/statsd/" as statsd;   # was: import "statsd.j" as statsd;
```

**[Documentation](https://jennifer-language.github.io/deck-statsd/)**

## Install

```sh
jvc add @jennifer/statsd
```

This vendors the deck to `vendor/<scope>/statsd/`. Without `jvc`, unpack the
`src/` subtree of a release archive (`<scope>-statsd-<version>.tar.gz`) there.

## Usage

```jennifer
import "@jennifer/statsd/" as statsd;

def c as statsd.Client init statsd.clientWith("127.0.0.1:8125", "web");
statsd.increment($c, "requests");
statsd.timing($c, "response", 42);
statsd.close($c);
```

## Requirements

Jennifer 0.25.0 or newer, the default `jennifer` binary (`net`), not
`jennifer-tiny`.

## Development

```sh
tools/check-all.sh                   # the full gate, as CI runs it
jennifer test src/statsd_test.j      # the test overlay
tools/check-docs.sh                  # docs against the code
tools/pack.sh dist                   # build the release archive
```

## Licence

LGPL-3.0-only, see [LICENSE](LICENSE). Copyright (C) 2026 mplx &lt;mplx@jennifer-lang.dev&gt;.

`tools/check-headers.sh` checks every source file's SPDX header against
`deck.toml`.
