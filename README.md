### API Definitions

This repository contains the Protocol Buffer (protobuf) definitions for the `tinyurl` project. These files define the messages and services used by the various microservices within the project. The `common.proto` file contains shared message definitions to avoid code duplication and ensure consistency across the API.

### Services

#### `KeyGeneratorService`

The `KeyGeneratorService` provides methods for generating unique keys.

* `Ping`: A health check RPC to ensure the service is running. It uses the `PingRequest` and `PingResponse` messages.
* `GenerateKey`: An RPC that creates a new unique key.
    * **Request:** `GenerateKeyRequest` is an empty message.
    * **Response:** `GenerateKeyResponse` contains the newly generated key as a string.

#### `StatisticsService`

The `StatisticsService` defines the API for retrieving statistical data.

* `Ping`: A health check RPC to ensure the service is operational. It uses the shared `PingRequest` and `PingResponse` messages.
* `GetStatistics`: This RPC retrieves a stream of statistics points based on a query.
    * **Request:** `GetStatisticsRequest` allows filtering statistics by a `tag` and a time interval defined by `interval_start` and `interval_end`. It can also include an `interval_duration` and an optional `timezone`.
    * **Response:** The `GetStatisticsResponse` is a stream of messages that can contain either summary statistics (`GetStatisticsStats`) or a single statistic point (`StatisticPoint`).
        * `GetStatisticsStats`: Contains the total number of points.
        * `StatisticPoint`: Represents a single data point within a time interval, including the `interval_start`, `interval_end`, `interval_duration`, and `total_visits`.

### Messages

#### Common Messages (`common.proto`)

The common messages are shared definitions used across multiple services.

* `PingRequest`: An empty message used as a request for health check RPCs.
* `PingResponse`: A response message for health check RPCs, containing a status string.

#### Task Messages (`task.proto`)

The `task.proto` file defines messages for asynchronous tasks.

* `Task`: A message representing a single task to be processed. It contains a `oneof` field for different task types.
* `InsertRecord`: A task message for inserting a new record. It is one of the possible task types within the `Task` message. It contains a `tag` and a `time` field.
