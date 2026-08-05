# Uberish Eats Platform

Uberish Eats is an on-demand food delivery platform inspired by Uber Eats. It connects customers, restaurants, and couriers in one system.

This is a hands-on project for building a real-world distributed system. I am using it to get practical experience with event-driven architecture, domain-driven design, and reliable communication between services.

## What the platform does

The platform is planned to support these main actions:

- Customers search for restaurants and menu items.
- Customers place food orders and follow their order status.
- Restaurants accept orders and prepare them.
- Couriers receive delivery jobs and update their location.
- Customers track their courier and receive the delivered order.

The detailed product requirements are available in [docs/prd.md](docs/prd.md).

## Architecture focus

The project is built around small services that communicate through events. Each service owns its business logic and data as much as possible.

The main architecture topics are:

- Event-driven communication with RabbitMQ
- Domain-driven design and clear business boundaries
- Saga pattern for long-running order workflows
- Transactional Outbox pattern for reliable event publishing
- Eventual consistency between services
- Redis for courier location and fast data access
- Elasticsearch for restaurant and menu search
- PostgreSQL for transactional business data
- Terraform for infrastructure as code

## Main services

The current repository contains the first service boundaries:

| Service | Responsibility |
| --- | --- |
| `order-service` | Manages the order lifecycle and order events |
| `search-service` | Provides restaurant and menu search |
| `courier-service` | Manages couriers, delivery jobs, and location updates |

More services may be added when the platform grows, for example payment, restaurant, notification, and dispatch services.

## Repository structure

```text
.
├── docs/                 # Product and architecture documentation
├── services/             # Application services
├── terraform/            # Infrastructure as code and Terraform modules
├── docker-compose.yml    # Local infrastructure and service setup
└── Makefile              # Common project commands
```

## Infrastructure

Terraform modules are prepared for the main infrastructure components:

- PostgreSQL
- RabbitMQ
- Redis
- Elasticsearch

The infrastructure files are in the [`terraform/`](terraform/) directory. Local development will also use Docker Compose where possible.

## Project status

This project is in the early development stage. The repository currently contains the initial documentation, service structure, Dockerfiles, and Terraform modules. Features and implementation details will be added step by step.

## Roadmap

- [ ] Set up the local development environment
- [ ] Define service contracts and domain models
- [ ] Implement the first order flow
- [ ] Add the Transactional Outbox pattern
- [ ] Add asynchronous order events
- [ ] Implement the Saga workflow for order processing
- [ ] Add courier assignment and location tracking
- [ ] Add automated tests and observability
- [ ] Improve the Terraform environment

## A note about the project

Uberish Eats is an independent practice project inspired by common food delivery platforms. It is not connected to Uber or Uber Eats.
