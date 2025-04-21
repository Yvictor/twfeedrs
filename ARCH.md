# TWFeedRS Architecture Design

## 1. Overview

This project aims to receive market data feeds (primarily multicast) from exchanges, parse them using `nom`, transform them into a defined format, and publish them to a message broker. The core focus is on high performance, low latency, maintainability, and handling the diverse message formats specified by TWSE, leveraging Rust's capabilities and specific libraries like `monoio`, `kanal`, `fastrace`, and `compact_str`.

## 2. Core Components

```mermaid
graph LR
    subgraph Input Sources
        direction LR
        MC(Multicast Listener <br/> [monoio])
    end

    subgraph Processing Pipeline
        direction TB
        ChanRaw[(Raw Data Channel <br/> kanal<Bytes>)] --> Dispatcher(Parser Dispatcher <br/> [Identify Format Code]);
        Dispatcher --> P1(Parser Fmt 1 <br/> [nom, compact_str]);
        Dispatcher --> P2(Parser Fmt 2 <br/> [nom, compact_str]);
        Dispatcher --> Pn(Parser Fmt N <br/> [nom, compact_str]);
        P1 --> ChanParsed[(Parsed Event Channel <br/> kanal<UnifiedEvent>)]
        P2 --> ChanParsed
        Pn --> ChanParsed
        ChanParsed --> Transformer(Data Transformer)
        Transformer --> ChanFormatted[(Formatted Data Channel <br/> kanal<FormattedTradeEvent>)]
    end

    subgraph Output
        direction LR
        MBP(Message Broker Producer <br/> [e.g., rdkafka, lapin])
    end

    Config(Configuration <br/> [config-rs, serde])
    Orchestrator(Core Orchestrator <br/> [monoio])
    Logging(Logging/Metrics <br/> [fastrace])

    Input --> ChanRaw

    ChanFormatted --> MBP

    Config --> Orchestrator
    Orchestrator --> Input
    Orchestrator --> Dispatcher
    Orchestrator --> Transformer
    Orchestrator --> MBP
    Orchestrator --> Logging
```

## 3. Component Details

*   **Input Sources**:
    *   `Multicast Listener`: Uses `monoio` for efficient UDP multicast packet reception. Handles potential packet loss and reordering if necessary. Sends raw `Bytes` to `ChanRaw`.
*   **Raw Data Channel (`ChanRaw`)**: A high-performance `kanal` channel buffering raw byte payloads (`Bytes`) received from input sources before parsing.
*   **Parser Dispatcher (`Dispatcher`)**:
    *   Consumes `Bytes` from `ChanRaw`.
    *   Reads the standard message `HEADER` (specifically the `Transmission Format Code` field) to identify the message type.
    *   Routes the raw `Bytes` payload to the corresponding specialized parser module (`P1`...`Pn`).
*   **Modular Parsers (`P1`...`Pn`)**:
    *   A collection of modules, each dedicated to parsing a specific TWSE message format (e.g., Format 1, Format 6).
    *   Uses `nom` for zero-copy parsing of the specific byte stream structure, including handling `PACK BCD` encoding and variable fields.
    *   Utilizes `compact_str` for relevant string fields.
    *   Parses the specific format into a variant of the `UnifiedEvent` enum.
    *   Sends the resulting `UnifiedEvent` to `ChanParsed`.
    *   This modular design enhances maintainability, testability, and allows for individual parser benchmarking.
*   **Parsed Event Channel (`ChanParsed`)**: A `kanal` channel transmitting the standardized internal `UnifiedEvent` enum, representing successfully parsed data from any format.
*   **Transformer**:
    *   Consumes `UnifiedEvent`s from `ChanParsed`.
    *   Matches on the `UnifiedEvent` enum variant to apply format-specific transformations or data enrichment.
    *   Converts the internal representation into the final `FormattedTradeEvent` required by the downstream systems/message broker.
    *   Sends `FormattedTradeEvent`s to `ChanFormatted`.
*   **Formatted Data Channel (`ChanFormatted`)**: A `kanal` channel holding the final `FormattedTradeEvent` ready for publishing.
*   **Message Broker Producer (`MBP`)**: Connects to the configured message broker (e.g., Kafka, NATS, RabbitMQ) and publishes the `FormattedTradeEvent` messages consumed from `ChanFormatted`. Manages connection pooling and publishing semantics.
*   **Core Orchestrator**: The main application entry point.
    *   Initializes and manages the lifecycle of all other components (spawns tasks using `monoio`).
    *   Loads configuration.
    *   Sets up logging (`fastrace`) and potentially metrics.
    *   Handles graceful shutdown.
*   **Configuration**: Loads settings from files or environment variables (e.g., multicast group/port, broker addresses, topic names) using libraries like `config-rs` and `serde`.
*   **Logging/Metrics**: Uses `fastrace` for high-performance tracing/logging. Potentially integrates with metrics systems for monitoring performance and throughput.

## 4. Key Technologies

*   **Language**: Rust
*   **Async Runtime**: `monoio` (prioritized for I/O performance).
*   **Channels**: `kanal` (prioritized for inter-task communication performance).
*   **Multicast**: `monoio`.
*   **Parsing**: `nom` (within modular parsers).
*   **String Handling**: `compact_str`.
*   **Configuration**: `config-rs`, `serde`.
*   **Logging**: `fastrace`.
*   **Error Handling**: `anyhow` or `thiserror`.
*   **Message Broker Clients**: Depends on the chosen broker (e.g., `rdkafka`, `lapin`, `nats`).

## 5. Data Flow

1.  `Multicast Listener` receives raw UDP datagrams (`Bytes`).
2.  Raw `Bytes` are sent via `ChanRaw` to the `Parser Dispatcher`.
3.  `Parser Dispatcher` identifies the format code from the header and routes the `Bytes` to the appropriate `Modular Parser` (e.g., `P6` for Format 6).
4.  The specific `Modular Parser` (e.g., `P6`) parses the `Bytes` using `nom` into a corresponding `UnifiedEvent` variant (e.g., `UnifiedEvent::RealtimeQuote(quote_data)`).
5.  The `UnifiedEvent` is sent via `ChanParsed` to the `Transformer`.
6.  The `Transformer` matches on the `UnifiedEvent` variant, performs necessary transformations, and creates a `FormattedTradeEvent`.
7.  The `FormattedTradeEvent` is sent via `ChanFormatted` to the `Message Broker Producer`.
8.  The `Message Broker Producer` serializes and publishes the event to the external message broker.
9.  The `Orchestrator` manages all tasks, configuration, and application lifecycle.
