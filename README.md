# Fork of the OpenMessaging Benchmark Framework for Popov R&D experiments

This repository is a research-oriented *fork* of the [**OpenMessaging Benchmark (OMB)*** framework](https://github.com/openmessaging/benchmark), used in Popov R&D to study real-world performance and architectural trade-offs of streaming and messaging systems (RabbitMQ, ActiveMQ Artemis, Kafka-like semantics, etc.).

## What is OMB?

OMB is an open-source, JVM-based benchmarking framework designed to measure end-to-end behavior across different brokers and protocols.

## Packaging

OMB is built with Maven and produces a runnable distribution.

> mvn clean package -DskipTests

Artifacts are generated under:

*openmessaging-benchmark/package/target/*

This folder contains an archive:

*openmessaging-benchmark-0.0.1-SNAPSHOT-bin.tar.gz*

You just extract the content of this archive in any working dir.

Once extracted, you will see *bin/* folder containing several bash scripts:

- **benchmark**: bash script that launches a coordinator process;
- **benchmark-worker**: bash script that launches an execution engine process.

## Launching

Benchmark is launched as a JVM coordinator process with two YAML config files passed as startup params:

- **Driver**: config. defining the target system (system-specific settings: url address, credentials, etc.);
- **Workload**: config. describing the benchmark scenario (system-agnostic params: producers, topics, consumers, rates, duration).

Based on these configs, OMB orchestrates producers and consumers, controls publish rates and payload sizes, and collects metrics such as throughput and latency throughout the run.

Generalized benchmark launch command looks like this:

```
./bin/benchmark \
--drivers <driver-path>[,<driver-path>...] \
[--workers <worker-url>[,<worker-url>...]] \
<workload-path> [<workload-path>...]
```

where:

- -- drivers defines comma separated list of systems being benchmarked sequentially;
- -- workers [optional] defines a comma separated list of separately deployable workers URLs.
- workload-path(s) define a whitespace-separated list of workload files paths.

## Results analysis

OMB computes metrics in fixed *10-second* reporting windows. At the end of a benchmark run, these windowed statistics are aggregated and emitted as a JSON (by default) result file, stored in the directory from which the test was launched.

See the next section for more detailed explanation of how to interpret the results.

## Further reading
- **Andrii Popov**: [*OpenMessaging Benchmark framework*: overview](https://blog.popov-rnd.com/posts/omb-framework);
- **Official documentation***: [*OpenMessaging Benchmark framework*](https://openmessaging.cloud/docs/benchmarks/).