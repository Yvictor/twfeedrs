# TWFeedRS Key Information

## Project Goal

To create a high-performance Rust application that:
1.  Receives market data from exchanges (primarily via UDP Multicast, potentially WebSocket/API).
2.  Parses the incoming raw data efficiently.
3.  Transforms the parsed data into a predefined target format.
4.  Publishes the formatted data to a message broker (e.g., Kafka, NATS, RabbitMQ).

## Core Requirements & Constraints

*   **Language**: Rust (stable toolchain).
*   **Performance**: Focus on low latency and high throughput.
*   **Input Methods**: Must support UDP Multicast. WebSocket and API polling are secondary considerations.
*   **Parsing**: Use the `nom` crate within a modular parser architecture. A central dispatcher identifies the message format code from the header and routes data to specific parser modules (one per format). This approach is crucial for handling the numerous TWSE formats while ensuring maintainability, testability, and enabling per-format benchmarking.
*   **Multicast I/O**: Prefer `monoio` for handling UDP multicast networking for potential performance benefits.
*   **String Efficiency**: Utilize `compact_str` for string representations where appropriate (e.g., symbols, IDs) to reduce heap allocations and improve performance.
*   **Asynchronous**: Leverage the `monoio` async runtime for managing concurrent I/O operations, prioritizing performance.
*   **Channels**: Use `kanal` for high-performance inter-task communication.
*   **Output**: Target a message broker system.

## Key Technology Choices

*   **Runtime**: `monoio` (selected for I/O performance)
*   **Channels**: `kanal` (selected for channel performance)
*   **Multicast**: `monoio`
*   **Parsing**: `nom` (within modular parsers)
*   **Strings**: `compact_str::CompactString`
*   **Configuration**: `config-rs`, `serde`
*   **Logging**: `fastrace` (selected for tracing performance)
*   **Error Handling**: `thiserror` / `anyhow`
*   **CLI Args**: `clap`
*   **Message Broker Client**: TBD (e.g., `rdkafka`, `lapin`, `async-nats`)

## Naming Conventions

To ensure clarity, consistency, and easy mapping back to the official TWSE documentation, the following naming conventions are adopted:

*   **`UnifiedEvent` Enum Variants**: `FormatNN` + `DescriptiveName` (PascalCase)
    *   **Example**: `UnifiedEvent::Format06RealtimeQuote`, `UnifiedEvent::Format21IndexDefinition`
    *   **Rationale**: Emphasizes the **source format** first, which optimizes readability in `match` statements used for dispatching and classifying incoming messages based on their origin format.

*   **Payload Structs**: `DescriptiveName` + `FmtNN` (PascalCase)
    *   **Example**: `struct RealtimeQuoteFmt6 { ... }`, `struct IndexDefinitionFmt21 { ... }`
    *   **Rationale**: Emphasizes the **data content/type** first, which optimizes readability when working with the actual parsed data struct. The `FmtNN` suffix provides unambiguous linkage back to the specific format definition.

*   **Parser Modules (Files/Dirs)**: `format_nn` (snake_case)
    *   **Example**: `src/parsers/format_06.rs`, `src/parsers/format_21.rs`
    *   **Rationale**: Standard Rust module naming, clearly grouping parsers by format number.

*   **Parser Functions**: `parse_format_nn` (snake_case)
    *   **Example**: `fn parse_format_06(input: &[u8]) -> ...`
    *   **Rationale**: Standard Rust function naming, indicating the action and target format.

This slightly differentiated approach for Enum variants versus Structs aims to maximize readability in their respective primary contexts (dispatching vs. data usage) while maintaining overall consistency and traceability.

## Potential Challenges

*   Handling the large number of diverse TWSE data formats (addressed by modular parser design).
*   Implementing efficient and correct `nom` parsers for each specific format, including PACK BCD.
*   Managing UDP packet loss or out-of-order delivery in multicast feeds.
*   Optimizing the entire pipeline for minimal latency.
*   Ensuring seamless integration between `monoio`, `kanal`, and potentially other crates.
*   Error handling and recovery in a distributed, real-time system.
*   Achieving comprehensive test coverage and meaningful benchmarks for all parser modules.
