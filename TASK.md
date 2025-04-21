# TWFeedRS Task List

This document outlines the development tasks required for the TWFeedRS project.

## Phase 1: Project Setup & Core Structures

- [x] **Initialize Project**: Create a new Rust binary project using `cargo new twfeedrs`.
- [ ] **Add Base Dependencies**: Include `monoio`, `kanal`, `fastrace`, `nom`, `compact_str`, `serde`, `config`, `clap`, `thiserror`/`anyhow` in `Cargo.toml`.
- [ ] **Define Core Data Structures**:
    - [ ] Define `UnifiedEvent` enum (to represent outputs from different parsers).
    - [ ] Define structs for each TWSE format payload (e.g., `BasicDataFmt1`, `QuoteFmt6`).
    - [ ] Define `FormattedTradeEvent` struct (representing the final output format).
    - [ ] Use `CompactString` from `compact_str` for relevant string fields (symbols, IDs, etc.).
    - [ ] Implement `serde::Serialize` for `FormattedTradeEvent`.
- [ ] **Setup Configuration**: Implement configuration loading using `config-rs` (e.g., from `config.toml` or environment variables). Define necessary configuration fields (multicast addresses, ports, broker details, etc.).
- [ ] **Setup Logging**: Initialize `fastrace` for high-performance tracing in `main.rs`.
- [ ] **Setup Basic Error Handling**: Choose and integrate an error handling strategy (`thiserror` or `anyhow`).
- [ ] **Command Line Arguments**: Use `clap` to define basic command-line arguments (e.g., path to config file).

## Phase 2: Input Modules

- [ ] **Implement Multicast Listener**:
    - [ ] Create a module for multicast handling (`src/input/multicast.rs`).
    - [ ] Use `monoio` to join the multicast group and receive UDP packets.
    - [ ] Send received raw byte buffers to the `Raw Data Channel` (`kanal::AsyncSender<Bytes>`).
    - [ ] Handle potential I/O errors.

## Phase 3: Processing Pipeline

- [ ] **Implement Parser Dispatcher** (`src/dispatcher.rs`):
    - [ ] Consume raw data (`Bytes`) from the `Raw Data Channel` (`kanal::AsyncReceiver<Bytes>`).
    - [ ] Implement logic to read the `HEADER` and extract the `Transmission Format Code` (handle PACK BCD).
    - [ ] Route the `Bytes` payload to the appropriate specific parser function/module based on the format code.
    - [ ] Handle unknown format codes (e.g., log, send `UnifiedEvent::UnsupportedFormat`).

- [ ] **Implement Modular Parsers** (`src/parsers/mod.rs`, `src/parsers/format_xx.rs` ...):
    - [ ] Create a central `parsers` module.
    - [ ] Implement helper functions for common tasks (e.g., PACK BCD decoding, header parsing).
    - [ ] **Implement Format 6 Parser** (`src/parsers/format_06.rs`) - *PRIORITY 1*
        - [ ] Define `nom` parsers for Format 6 (Real-time Auction Quotes).
        - [ ] Handle variable length based on `Disclosed Item Remarks` bitmap.
        - [ ] Handle PACK BCD fields.
        - [ ] Parse into `UnifiedEvent::RealtimeQuote(QuoteFmt6)`.
    - [ ] **Implement Format 21 Parser** (`src/parsers/format_21.rs`) - *PRIORITY 1*
        - [ ] Define `nom` parsers for Format 21 (Realtime Index Definition).
        - [ ] Handle PACK BCD fields.
        - [ ] Parse into `UnifiedEvent::IndexDefinition(IndexDefFmt21)`.
    - [ ] Implement other required TWSE format parsers (e.g., Fmt 1, Fmt 2, Fmt 4, Fmt 12, Fmt 13...) as needed:
        - [ ] Create a dedicated submodule (e.g., `src/parsers/format_01.rs`).
        - [ ] Define `nom` parsers specific to that format's fields and structure.
        - [ ] Parse the input `Bytes` into the corresponding struct (e.g., `BasicDataFmt1`).
        - [ ] Return a `Result<UnifiedEvent, ParseError>`.
        - [ ] Use `compact_str` where appropriate.
    - [ ] Send successfully parsed `UnifiedEvent`s to the `Parsed Event Channel` (`kanal::AsyncSender<UnifiedEvent>`).
    - [ ] Handle parsing errors within each module (return `Err(ParseError)`).

- [ ] **Implement Transformer** (`src/transformer.rs`):
    - [ ] Create a transformer module (`src/transformer.rs`).
    - [ ] Consume `UnifiedEvent`s from the `Parsed Event Channel` (`kanal::AsyncReceiver<UnifiedEvent>`).
    - [ ] Implement `match` logic to handle different variants of the `UnifiedEvent` enum.
    - [ ] Implement logic to convert specific `UnifiedEvent` variants (e.g., `UnifiedEvent::BasicStockData`) to `FormattedTradeEvent`.
    - [ ] Send `FormattedTradeEvent`s to the `Formatted Data Channel` (`kanal::AsyncSender<FormattedTradeEvent>`).

## Phase 4: Output Module

- [ ] **Implement Message Broker Producer**:
    - [ ] Choose a message broker (e.g., Kafka, NATS, RabbitMQ) and add the corresponding Rust client library (e.g., `rdkafka`, `async-nats`, `lapin`).
    - [ ] Create a producer module (`src/output/broker.rs`).
    - [ ] Consume `FormattedTradeEvent`s from the `Formatted Data Channel` (`kanal::AsyncReceiver<FormattedTradeEvent>`).
    - [ ] Serialize `FormattedTradeEvent` (e.g., to JSON using `serde_json`).
    - [ ] Connect to the message broker based on configuration.
    - [ ] Publish serialized messages to the configured topic/queue.
    - [ ] Handle connection errors and potential publishing failures/acknowledgments.

## Phase 5: Integration & Orchestration

- [ ] **Implement Core Orchestrator** (`src/main.rs`):
    - [ ] Read configuration.
    - [ ] Initialize logging.
    - [ ] Create the necessary `kanal` channels (async versions).
    - [ ] Spawn separate `monoio` tasks for each component (Input Listener, Parser Dispatcher, Transformer, Producer).
    - [ ] Pass channel senders/receivers to the respective tasks.
    - [ ] Implement graceful shutdown logic (e.g., on SIGINT/SIGTERM).
    - [ ] Wait for tasks to complete.

## Phase 6: Testing & Documentation

- [ ] **Unit Tests**:
    - [ ] Write unit tests for the `Parser Dispatcher` logic.
    - [ ] Write unit tests for the **Format 6 Parser** using sample Format 6 byte data.
    - [ ] Write unit tests for the **Format 21 Parser** using sample Format 21 byte data.
    - [ ] Write unit tests for other implemented parsers (`format_xx.rs`) using sample byte data.
    - [ ] Write unit tests for common helpers (e.g., PACK BCD decoding).
    - [ ] Write unit tests for the `Transformer` logic (handling different `UnifiedEvent` variants).
- [ ] **Integration Tests**:
    - [ ] Write integration tests for the data flow (e.g., mock input source -> dispatcher -> specific parser -> transformer -> mock producer).
- [ ] **Documentation**:
    - [ ] Write a comprehensive `README.md` explaining setup, configuration, and usage.
    - [ ] Add code comments (doc comments) for public APIs and complex logic.
- [ ] **Containerization** (Optional):
    - [ ] Create a `Dockerfile` to build and run the application in a container.

## Phase 7: Refinement & Monitoring

- [ ] **Performance Profiling**: Profile the application under load to identify bottlenecks.
- [ ] **Optimization**: Optimize critical code paths based on profiling results.
- [ ] **Metrics**: Integrate basic metrics (e.g., messages processed, errors, channel backlog) using `metrics` crate and potentially export to Prometheus.
- [ ] **Benchmarking**:
    - [ ] Create benchmarks for the **Format 6 Parser** function.
    - [ ] Create benchmarks for the **Format 21 Parser** function.
    - [ ] Create benchmarks for other implemented parser functions.
    - [ ] Potentially benchmark the `Parser Dispatcher` itself.
