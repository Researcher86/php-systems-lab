# PHP Systems Lab

> A collection of small educational PHP projects for exploring memory, operating systems, concurrency, processes, event-driven architecture, networking, storage engines, and backend infrastructure.

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

How does PHP use memory and interact with the operating system?

How do worker processes work?

How does a job queue work?

How does an event-driven server work?

How does an HTTP server work?

How does a database engine store data on disk?
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
                  PHP Systems Lab
             Systems Programming in PHP
                         │
          ┌──────────────┴──────────────┐
          │                             │
          ▼                             ▼
          🧠                            🧠
 php-concurrency                 php-memory-lab
 Concurrency Fundamentals       Memory & OS Fundamentals
          │                             │
          └──────────────┬──────────────┘
                         │
          ┌──────────────┼────────────────┬──────────────────────┐
          │              │                │                      │
          ▼              ▼                ▼                      ▼
          ⚙️             🟥               🌐                     🗄️
 php-worker-pool   php-mini-cache   php-mini-http-server  php-mini-database
 Process Runtime   Event-Driven     HTTP Server           Storage Engine
                   Server
          │
          ▼
          📬
  php-job-queue
 Background Jobs
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

## 🧠 PHP Memory Lab

### [`php-memory-lab`](https://github.com/Researcher86/php-memory-lab)

> Exploring memory and operating-system fundamentals in PHP.

This project complements `php-concurrency` by focusing on the runtime and operating-system concepts that influence how PHP programs use memory.

Every topic becomes the smallest experiment that proves it, the result is measured at both levels that matter, and the number is explained rather than asserted.

### Core Question

> **How does PHP use memory and interact with the operating system?**

### Concepts

```text
memory_get_usage() vs RSS
    │
    ▼
/proc: VmRSS, RssAnon, PSS, Private_Dirty
    │
    ▼
zvals, refcounting, packed vs associative arrays
    │
    ▼
fork() and Copy-on-Write
    │
    ▼
Unix sockets, SysV shared memory, semaphores
    │
    ▼
Shared-memory ring buffer
    │
    ▼
mmap: lazy loading, MAP_SHARED vs MAP_PRIVATE
    │
    ▼
FFI and native memory
```

### The One Distinction

Everything in this project rests on the fact that two honest answers disagree:

```text
memory_get_usage()          RSS (VmRSS)

engine-managed bytes        resident physical pages
goes down on unset()        rarely goes back down
knows about zvals           knows nothing about zvals
blind to mmap/FFI/shm       counts every touched page
```

A process can show flat PHP memory while its RSS climbs, and the reverse. So every measurement here records both, always.

### Boundary with `php-concurrency`

The two projects overlap on `fork()`, process lifecycle, IPC, producer-consumer and backpressure, and the overlap is deliberate. What differs is the question:

```text
php-concurrency      how is work coordinated across processes?
                     → answers with patterns

php-memory-lab       what does that mechanism cost in pages and copies?
                     → answers with RssShmem, Pss, Private_Dirty
```

Which is how `php-memory-lab` reaches a conclusion its sibling never measures: a Unix socket beats shared memory, because the semaphore shared memory needs costs more than the copy it saves.

Read `php-concurrency` to learn the pattern; read `php-memory-lab` to learn what it costs.

---

## ⚙️ PHP Worker Pool

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

## 📬 PHP Job Queue

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

## 🟥 PHP Mini Cache

### [`php-mini-cache`](https://github.com/Researcher86/php-mini-cache)

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

## 🌐 PHP Mini HTTP Server

### [`php-mini-http-server`](https://github.com/Researcher86/php-mini-http-server)

> An educational event-driven HTTP server written in PHP.

This project builds on many concepts explored in `php-mini-cache`.

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

## 🗄️ PHP Mini Database

### [`php-mini-database`](https://github.com/Researcher86/php-mini-database)

> A small, readable relational database written in PHP.

This project explores what happens when data has to survive a restart.

`php-mini-cache` keeps everything in memory:

```text
Process dies

↓

Data disappears
```

`php-mini-database` explores the opposite question:

```text
Process dies

↓

Data survives
```

Architecture:

```text
SQL
    │
    ▼
Parser
    │
    ▼
Query Planner
    │
    ▼
Executor
    │
    ├── Index
    │
    └── Storage Engine
             │
             ├── Records
             │
             ├── Pages
             │
             ├── File Format
             │
             └── WAL
                  │
                  ▼
               Recovery
```

### Core Question

> **How does a database engine work internally?**

### Concepts

* on-disk storage;
* file formats;
* records;
* pages;
* indexes;
* SQL parsing;
* query execution;
* transactions;
* WAL;
* crash recovery;
* locking;
* concurrency.

### Important Idea

A cache answers:

> **How do we keep data fast?**

A database answers:

> **How do we keep data safe?**

```text
php-mini-cache

In-Memory State


php-mini-database

Durable State
```

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
php-mini-cache

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
php-concurrency                 php-memory-lab
       │                                 │
       │ Concurrency                     │ Memory & OS
       │ Processes                       │ Fundamentals
       │ IPC                             │
       │ Event Loops                     │
       │ Fibers                          │
       └───────────────┬─────────────────┘
                       │
       ┌───────────────┼──────────────────────────────┐
       │               │                              │
       ▼               ▼                              ▼
php-worker-pool   php-mini-cache               php-mini-database
       │               │                              │
       ▼               ▼                              ▼
php-job-queue   php-mini-http-server          Storage & Recovery
       │               │                              │
       ▼               ▼                              ▼
Application        Server Runtime                Durable State
Infrastructure
```

---

# 🎓 Suggested Learning Path

The projects can be explored independently.

However, the recommended order is:

## Level 1 — Memory & OS Fundamentals

### 🧠 `php-memory-lab`

Start here to explore how PHP memory behavior relates to operating-system fundamentals.

---

## Level 2 — Concurrency Fundamentals

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

## Level 3 — Process Runtime

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

## Level 4 — Background Processing

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

## Level 5 — Event-Driven Servers

### 🟥 `php-mini-cache`

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

## Level 6 — HTTP Server Runtime

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

## Level 7 — Database Engine

### 🗄️ `php-mini-database`

Learn:

```text
Files

↓

Pages

↓

Records

↓

Indexes

↓

SQL

↓

Query Execution

↓

Transactions

↓

WAL

↓

Crash Recovery
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

## Memory & OS Experiments

```text
How does memory usage change as a PHP program runs?

↓

What happens to memory when a process is created?

↓

Which behaviors are managed by PHP, and which by the operating system?
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

## Storage & Durability Experiments

```text
What happens when the process is killed in the middle of a write?

↓

What does WAL replay restore after a crash?

↓

How does an index change the cost of a query?
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

SQLite

MySQL

PostgreSQL
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

# 🧰 Shared Conventions

The projects are independent repositories, but they are built the same way, so that moving between them costs nothing.

```text
Docker              nothing is installed on your machine
Makefile            the same verbs everywhere
Composer            PSR-4, PHP 8.5, platform extensions declared
PHPUnit             tests/ next to src/
PHPStan             level 8 where the code allows it
PHP-CS-Fixer        one shared .php-cs-fixer.dist.php
GitHub Actions      test, analyse, format:check on every push
docs/DECISIONS.md   what was decided, and why
docs/PHASES.md      how it was built, phase by phase
```

The verbs:

```bash
make install         # build the image and install dependencies
make test            # PHPUnit
make analyse         # PHPStan
make format-check    # PHP-CS-Fixer, dry run
make format          # apply PHP-CS-Fixer
make shell           # a shell in the container
```

`php-memory-lab` carries the most complete version of this and is the reference for it - including `tests/DocumentationTest.php`, which checks that every link, path and command in the documentation still resolves.

`php-concurrency` is the deliberate exception: it is a lesson course rather than an engineered project, so each lesson is a directory with a `README.md`, a `diagram.txt` and a runnable `main.php`, with no Composer package around it.

---

# 📦 These Are Not Libraries

None of these projects is published, and none depends on another as a package.

```text
What travels between them:

   the mechanism        read in one, reimplemented in the next
   the measurement      a number you can reproduce
   the conventions      the same Makefile, the same formatter

What does not travel:

   the code             deliberately
```

So `php-worker-pool` has its own shared-memory telemetry and `php-job-queue` its own queue, even though `php-memory-lab` implements both. That is the point: rebuilding a mechanism is how you learn it, and a shared dependency would remove exactly the work that teaches.

Every project declares `"license": "MIT"` in its `composer.json` and says so in its README. There is no `LICENSE` file, by choice.

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
php-mini-cache
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

## Storage & Persistence

```text
php-memory-lab
       │
       ▼
php-mini-cache
       │
       ▼
php-mini-database
```

Possible future experiments:

```text
Alternative Index Structures

Query Plan Visualization

Replication
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
├── MEMORY & OS
│   └── php-memory-lab
│
├── CONCURRENCY
│   └── php-concurrency
│
├── PROCESSES
│   ├── php-worker-pool
│   └── php-job-queue
│
├── STORAGE
│   └── php-mini-database
│
└── NETWORKING
    ├── php-mini-cache
    └── php-mini-http-server

All projects
    │
    ▼
Experiments
    │
    ▼
Understanding
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
