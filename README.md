# PHP Systems Lab

> A collection of small educational PHP projects for exploring concurrency, processes, event-driven architecture, networking, and backend infrastructure.

**PHP Systems Lab** is a collection of educational projects built to understand how backend and systems programming concepts work internally.

The goal is not to build production-ready replacements for existing technologies.

The goal is to build small, understandable implementations that can be:

* read;
* run;
* modified;
* broken;
* debugged;
* experimented with.

> **Learn backend infrastructure by building executable mental models.**

---

# 🧠 The Philosophy

Modern backend infrastructure can be difficult to understand.

Production systems often contain:

```text
Thousands of files

↓

Multiple abstraction layers

↓

Plugins

↓

Configuration

↓

Observability

↓

Production concerns

↓

Complex infrastructure
```

Eventually, the original idea can become difficult to see.

This collection takes a different approach:

```text
Idea

↓

Minimal Architecture

↓

Runnable Implementation

↓

Experiments

↓

Understanding
```

Every project focuses on one fundamental question.

For example:

```text
How does concurrency work?

How do worker processes work?

How does a job queue work?

How does an event-driven server work?

How does an HTTP server work?
```

The projects are intentionally designed to be:

```text
Small enough to understand

↓

Real enough to experiment with

↓

Simple enough to modify

↓

Complex enough to demonstrate real engineering problems
```

---

# 🗺️ The Ecosystem

```text
                              🧠
                      php-concurrency
                  Concurrency Fundamentals
                              │
              ┌───────────────┼────────────────┐
              │               │                │
              ▼               ▼                ▼
             ⚙️              🟥               🔬
      php-worker-pool    php-mini-redis    Experiments
       Process Runtime   Event-Driven Server
              │               │
              │               │
              ▼               ▼
             📬              🌐
       php-job-queue   php-mini-http-server
     Background Jobs      HTTP Server
```

The projects explore different layers of backend and systems programming.

---

# 📚 Projects

## 🧠 PHP Concurrency

### [`php-concurrency`](https://github.com/Researcher86/php-concurrency)

> Exploring the fundamental building blocks of concurrency in PHP.

This project explores:

* processes;
* `pcntl_fork()`;
* IPC;
* process communication;
* synchronization;
* concurrency patterns;
* producer-consumer;
* event loops;
* Fibers;
* asynchronous I/O.

### Core Question

> **How does concurrency work?**

### Concepts

```text
Processes
    │
    ▼
Fork
    │
    ▼
IPC
    │
    ▼
Synchronization
    │
    ▼
Concurrency Patterns
    │
    ▼
Event Loops
    │
    ▼
Fibers
```

This repository provides many of the fundamental concepts used by the rest of the ecosystem.

---

# ⚙️ PHP Worker Pool

### [`php-worker-pool`](https://github.com/Researcher86/php-worker-pool)

> An educational implementation of a persistent multi-process Worker Pool.

This project explores how a master process manages persistent worker processes.

Architecture:

```text
Clients
    │
    ▼
Master Process
    │
    ├── Request Queue
    │
    ├── Dispatcher
    │
    └── Worker Supervisor
             │
             ▼
       Worker Processes
```

### Core Question

> **How do persistent worker processes work?**

### Concepts

* master process;
* worker processes;
* IPC;
* Unix Domain Sockets;
* request dispatching;
* worker lifecycle;
* worker recycling;
* supervision;
* graceful shutdown.

### Worker Lifecycle

```text
                ┌───────────────┐
                │               │
                ▼               │
START → IDLE ⇄ BUSY             │
                │               │
                ▼               │
             DRAINING           │
                │               │
                ▼               │
             STOPPING           │
                │               │
                ▼               │
               DEAD ────────────┘
```

---

# 📬 PHP Job Queue

### [`php-job-queue`](https://github.com/Researcher86/php-job-queue)

> An educational implementation of a background job processing system.

This project explores how background work can be represented, stored, processed and retried.

Architecture:

```text
Producer
    │
    ▼
Job
    │
    ▼
Queue
    │
    ▼
Reservation
    │
    ▼
Worker
    │
    ▼
Processing
    │
    ├── ACK
    │
    ├── Retry
    │
    └── Failed
```

### Core Question

> **How does reliable background processing work?**

### Concepts

* jobs;
* queues;
* producers;
* consumers;
* workers;
* reservation;
* acknowledgements;
* retries;
* retry backoff;
* delayed jobs;
* visibility timeout;
* failed jobs;
* idempotency.

### Important Idea

A Worker Pool manages:

> **Workers**

A Job Queue manages:

> **Work**

```text
Worker Pool

How do we manage workers?


Job Queue

How do we manage jobs?
```

---

# 🟥 PHP Mini Redis

### [`php-mini-redis`](https://github.com/Researcher86/php-mini-redis)

> An educational event-driven in-memory database server.

This project explores a completely different concurrency model from the Worker Pool.

Instead of:

```text
Many Processes
```

it explores:

```text
One Process

+

Event Loop

+

Many Connections
```

Architecture:

```text
Clients
    │
    ▼
TCP Server
    │
    ▼
Event Loop
    │
    ├── Read Events
    ├── Write Events
    └── Timers
            │
            ▼
      Protocol Parser
            │
            ▼
    Command Dispatcher
            │
            ▼
      Command Handler
            │
            ▼
     In-Memory Storage
```

### Core Question

> **How does an event-driven server work?**

### Concepts

* TCP servers;
* client connections;
* event loops;
* non-blocking I/O;
* buffers;
* protocol parsing;
* command dispatching;
* in-memory storage;
* TTL;
* Pub/Sub.

---

# 🌐 PHP Mini HTTP Server

### [`php-mini-http-server`](https://github.com/Researcher86/php-mini-http-server)

> An educational event-driven HTTP server written in PHP.

This project builds on many concepts explored in `php-mini-redis`.

The difference is that instead of implementing a database protocol, it explores:

> **HTTP.**

Architecture:

```text
Clients
    │
    ▼
TCP Server
    │
    ▼
Event Loop
    │
    ▼
Connection
    │
    ▼
Read Buffer
    │
    ▼
HTTP Parser
    │
    ▼
HttpRequest
    │
    ▼
Router
    │
    ▼
Middleware
    │
    ▼
Request Handler
    │
    ▼
HttpResponse
    │
    ▼
HTTP Encoder
    │
    ▼
Write Buffer
    │
    ▼
Client
```

### Core Question

> **How does an HTTP server work internally?**

### Concepts

* TCP;
* event loops;
* HTTP parsing;
* partial reads;
* partial writes;
* request buffering;
* response buffering;
* routing;
* route parameters;
* middleware;
* request handlers;
* keep-alive;
* HTTP pipelining;
* timeouts;
* backpressure;
* graceful shutdown.

---

# 🔀 Two Major Concurrency Models

One of the most interesting parts of this ecosystem is that it explores different approaches to concurrency.

---

## ⚙️ Multi-Process Model

Used in:

```text
php-worker-pool
```

Architecture:

```text
                 Master
                   │
        ┌──────────┼──────────┐
        │          │          │
        ▼          ▼          ▼
     Worker 1   Worker 2   Worker N
```

The master manages multiple operating system processes.

This model explores:

* `fork()`;
* process lifecycle;
* IPC;
* supervision;
* worker recycling.

---

## 🟥 Event-Driven Model

Used in:

```text
php-mini-redis

php-mini-http-server
```

Architecture:

```text
                 Event Loop
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
      Client A     Client B     Client N
```

One process manages many connections.

This model explores:

* non-blocking I/O;
* event loops;
* connection state;
* buffers;
* timers.

---

# 🧬 How the Projects Connect

The projects are not isolated.

They build on related concepts.

```text
php-concurrency
       │
       │ Processes
       │ IPC
       │ Event Loops
       │ Fibers
       │
       ├─────────────────────┐
       │                     │
       ▼                     ▼
php-worker-pool       php-mini-redis
       │                     │
       │ Workers             │ TCP
       │ Lifecycle           │ Event Loop
       │ IPC                 │ Connections
       │                     │
       ▼                     ▼
php-job-queue      php-mini-http-server
       │                     │
       │ Background Jobs     │ HTTP
       │ Retries             │ Routing
       │ ACK                 │ Middleware
       │                     │
       ▼                     ▼
Application Infrastructure   Server Runtime
```

---

# 🎓 Suggested Learning Path

The projects can be explored independently.

However, the recommended order is:

## Level 1 — Concurrency Fundamentals

### 🧠 `php-concurrency`

Learn:

```text
Processes

↓

IPC

↓

Synchronization

↓

Concurrency Patterns

↓

Event Loops

↓

Fibers
```

---

## Level 2 — Process Runtime

### ⚙️ `php-worker-pool`

Learn:

```text
Master Process

↓

Workers

↓

IPC

↓

Dispatching

↓

Worker Lifecycle

↓

Graceful Shutdown
```

---

## Level 3 — Background Processing

### 📬 `php-job-queue`

Learn:

```text
Jobs

↓

Queues

↓

Workers

↓

Reservation

↓

Processing

↓

ACK

↓

Retries
```

---

## Level 4 — Event-Driven Servers

### 🟥 `php-mini-redis`

Learn:

```text
TCP

↓

Connections

↓

Event Loop

↓

Non-Blocking I/O

↓

Protocol Parsing

↓

Storage
```

---

## Level 5 — HTTP Server Runtime

### 🌐 `php-mini-http-server`

Learn:

```text
TCP

↓

Event Loop

↓

HTTP

↓

Request Parsing

↓

Routing

↓

Middleware

↓

Handlers

↓

Responses
```

---

# 🔬 Learn by Experimenting

These projects are designed to be modified.

The recommended learning loop is:

```text
Read
  │
  ▼
Run
  │
  ▼
Observe
  │
  ▼
Modify
  │
  ▼
Break
  │
  ▼
Debug
  │
  ▼
Understand
```

Examples of experiments:

---

## Process Experiments

```text
What happens when a Worker crashes?

↓

What happens when the Master restarts it?

↓

What happens during graceful shutdown?
```

---

## Queue Experiments

```text
What happens when a Job fails?

↓

What happens when the Worker crashes?

↓

What happens when ACK is never received?
```

---

## Event Loop Experiments

```text
What happens with 100 connections?

↓

What happens with a slow client?

↓

What happens when buffers grow?
```

---

## HTTP Experiments

```text
What happens when an HTTP request arrives in pieces?

↓

What happens when one connection sends multiple requests?

↓

What happens during Keep-Alive?

↓

What happens during graceful shutdown?
```

---

# 🧪 Production-Inspired, Not Production-Ready

These projects are inspired by real backend infrastructure.

They intentionally explore concepts used by technologies such as:

```text
PHP-FPM

RoadRunner

Swoole

FrankenPHP

Redis

RabbitMQ

Nginx

Apache
```

However, the goal is not to compete with them.

Production systems contain many additional concerns:

```text
Security

TLS

HTTP/2

HTTP/3

Observability

Metrics

Distributed Systems

High Availability

Load Balancing

Configuration

Plugins

Hot Reload

Production Hardening
```

Those features are important.

But they can hide the fundamental architecture.

PHP Systems Lab focuses on:

> **Understanding the core idea first.**

---

# 🧠 The Core Philosophy

Every project should answer:

```text
What problem does this solve?

↓

Why does this component exist?

↓

What happens if we remove it?

↓

What failure scenario does it prevent?
```

The goal is not memorization.

The goal is building a mental model.

---

# 🔭 Future Directions

The ecosystem can grow in several directions.

## Process-Based Infrastructure

```text
php-concurrency
       │
       ▼
php-worker-pool
       │
       ▼
php-job-queue
```

Possible future projects:

```text
php-process-supervisor

php-mini-runtime
```

---

## Event-Driven Networking

```text
php-mini-redis
       │
       ▼
php-mini-http-server
```

Possible future experiments:

```text
WebSocket Server

Custom TCP Protocol

Mini Proxy Server
```

---

# 🏗️ The Main Principle

This ecosystem does not try to build:

> Another production-ready framework.

It tries to build:

> **Executable mental models of backend infrastructure.**

Every project should remain:

```text
Small enough to understand.

Real enough to experiment with.

Simple enough to modify.

Complex enough to teach something important.
```

---

# 🗺️ The Mental Map

```text
                         BACKEND SYSTEMS
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
          ▼                     ▼                     ▼
     CONCURRENCY            PROCESSES             NETWORKING
          │                     │                     │
          ▼                     ▼                     ▼
  php-concurrency      php-worker-pool       php-mini-redis
          │                     │                     │
          │                     ▼                     ▼
          │                php-job-queue   php-mini-http-server
          │
          └───────────────┐
                          │
                          ▼
                   EXPERIMENTS
                          │
                          ▼
                    UNDERSTANDING
```

---

# Final Principle

> **Don't just use infrastructure.**

> **Build a small version of it.**

> **Run it.**

> **Modify it.**

> **Break it.**

> **Understand it.**

---

# License

MIT
