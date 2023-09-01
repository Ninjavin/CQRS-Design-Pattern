# CQRS Example with Spring Boot

This project serves as a practical demonstration of Command Query Responsibility Segregation (CQRS) principles using Spring Boot. It comprises two distinct services: `product-query-service` and `product-command-service`. These services communicate via Apache Kafka, enabling an event-driven architecture for seamless synchronization between their respective databases.

## Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [How it Works](#how-it-works)
- [Getting Started](#getting-started)
- [Kafka Integration](#kafka-integration)
- [Contribute](#contribute)
- [License](#license)

## Overview

### `product-query-service`
This service is responsible for handling read operations, specifically supporting GET requests. It serves as the query side of the application.

### `product-command-service`
The command service supports POST and PUT operations, functioning as the command side. It processes commands to create and update products.

## Project Structure

The project structure is organized as follows:

```
├── product-command-service
│   ├── src
│   ├── ...
├── product-query-service
│   ├── src
│   ├── ...
```

## How it Works

1. **Command Side - `product-command-service`**:
   - When a client sends a POST request to create a new product or a PUT request to update an existing product, the `product-command-service` processes the request and generates an event, such as `CreateProduct` or `UpdateProduct`, and publishes it to the Kafka topic dedicated to these events.

2. **Kafka Integration**:
   - Kafka acts as the central messaging service for this CQRS application.
   - It maintains topics for events like `CreateProduct` and `UpdateProduct`.
   - The `product-query-service` listens to these Kafka topics to consume the events.

3. **Query Side - `product-query-service`**:
   - The `product-query-service` subscribes to the Kafka topics where events are published.
   - When it receives an event, it updates its own database to ensure data consistency between the query and command sides.

4. **Client Requests**:
   - Clients can send GET requests to the `product-query-service` to retrieve product information. These requests are read-only operations.
   - The `product-query-service` responds to these requests with data from its synchronized database.

## Getting Started

To run the project locally, follow these steps:

1. **Prerequisites**:
   - Ensure you have Java and Apache Kafka installed on your system.

2. **Clone the Repository**:
   ```bash
   git clone https://github.com/Ninjavin/CQRS-Design-Pattern.git
   ```

3. **Build and Run Services**:
   - Navigate to the `product-query-service` and `product-command-service` directories.
   - Build and run each service using Maven or your preferred IDE.

4. **Configuration**:
   - Configure Kafka properties in the `application.properties` files of both services to specify the Kafka broker address.

5. **Test the Application**:
   - Send GET requests to `product-query-service` for read operations.
   - Use POST and PUT requests to `product-command-service` to create and update products.
   - Observe how the services interact via Kafka to maintain database consistency.

## Kafka Integration

This project heavily relies on Kafka for event-driven communication. Ensure you have Kafka set up with the following components:

- **Kafka Topics**: Create Kafka topics for events like `CreateProduct` and `UpdateProduct`. Configure these topics in both services.

- **Kafka Brokers**: Update Kafka broker properties in the `application.properties` files of both services to connect to your Kafka cluster.
