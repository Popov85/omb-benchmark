# Fork of the OpenMessaging Benchmark Framework for Popov R&D experiments

This repository is a research-oriented *fork* of the [**OpenMessaging Benchmark (OMB)*** framework](https://github.com/openmessaging/benchmark), used in Popov R&D to study real-world performance and architectural trade-offs of streaming and messaging systems (RabbitMQ, ActiveMQ Artemis, Kafka-like semantics, etc.).

## What is OMB?

OMB is an open-source, JVM-based benchmarking framework designed to measure end-to-end behavior across different brokers and protocols.

## Packaging

OMB is built with Maven and produces a runnable distribution.

```bash
mvn clean package -DskipTests
```

Artifacts are generated under:

```text
openmessaging-benchmark/package/target/
```

This folder contains an archive:

```text
openmessaging-benchmark-0.0.1-SNAPSHOT-bin.tar.gz
```

Extract the archive into any working directory.

Once extracted, the `bin/` folder contains several scripts:

- **benchmark**: launches the benchmark coordinator process
- **benchmark-worker**: launches a benchmark worker process

## Launching

The benchmark is launched as a JVM coordinator process with two YAML configuration files passed as startup parameters:

- **Driver**: defines the target system (system-specific settings such as addresses and credentials)
- **Workload**: describes the benchmark scenario (system-agnostic parameters such as producers, topics, rates, and duration)

Based on these configurations, OMB orchestrates producers and consumers, controls publish rates and payload sizes, and collects metrics such as throughput and latency throughout the run.

A generalized benchmark launch command looks like this:

```bash
./bin/benchmark \
  --drivers <driver-path>[,<driver-path>...] \
  [--workers <worker-url>[,<worker-url>...]] \
  <workload-path> [<workload-path>...]
```

where:

- `--drivers` defines a comma-separated list of systems benchmarked sequentially
- `--workers` (optional) defines a comma-separated list of externally deployed worker URLs
- `<workload-path>` defines one or more workload configuration files

## Results analysis

OMB computes metrics in fixed **10-second** reporting windows. At the end of a benchmark run, these windowed statistics are aggregated and emitted as a JSON result file (by default), stored in the directory from which the benchmark was launched.

See the next section for a more detailed explanation of how to interpret the results.

## Further reading

- **Andrii Popov**: [*OpenMessaging Benchmark framework*: overview](https://blog.popov-rnd.com/posts/omb-framework)
- **Official documentation***: [*OpenMessaging Benchmark framework*](https://openmessaging.cloud/docs/benchmarks/)

