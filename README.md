# PHP Systems Lab

> A collection of small educational PHP projects for exploring memory, operating systems, concurrency, processes, event-driven architecture, networking, storage engines, and backend infrastructure.

The goal is not to build production-ready replacements.

The goal is to understand **how backend systems actually work** by building simplified versions of their core mechanisms.

Every project is designed to be:

* small enough to understand
* real enough to experiment with
* simple enough to modify
* complex enough to expose real engineering problems

> **Learn backend infrastructure by building executable mental models.**

---

# 🗺️ Architecture

The projects form a progressive systems stack.

```text
                         🧪 PHP SYSTEMS LAB
                                  │
          ┌───────────────────────┼───────────────────────┐
          │                       │                       │
          ▼                       ▼                       ▼
   🧠 php-memory-lab      ⚡ php-concurrency        🌐 Networking
          │                       │                       │
          │               ┌───────┴───────┐               │
          │               ▼               ▼               ▼
          │        ⚙️ php-worker-pool  📬 php-job-queue  💾 php-mini-cache
          │               │               │               │
          │               └───────┬───────┘               ▼
          │                       │              🌐 php-mini-http-server
          │                       │                       │
          └───────────────┐       │                       │
                          ▼       ▼                       │
                 🗄️ php-mini-database     💡 Shared Concepts
                          │               ▲               │
                          └───────┬───────┘               │
                                  ▼                       │
                       🏗️ php-systems-platform ◄──────────┘
                                  │
                                  ▼
                         🚀 FINAL PLATFORM
```

The progression is intentional:

```text
Mechanisms
    ↓
Components
    ↓
Subsystems
    ↓
Infrastructure
    ↓
Integration
    ↓
Complete Platform
```

---

# 📚 Projects

## 🧠 PHP Memory Lab

### `php-memory-lab`

Exploring memory and operating-system fundamentals from PHP.

Topics include:

* virtual memory
* process memory
* RSS
* copy-on-write
* `fork()`
* `mmap()`
* shared memory
* memory mappings
* process isolation
* allocation behavior
* FFI and native memory
* interaction between PHP and the operating system

### Core questions

* What actually happens to memory after `fork()`?
* How does copy-on-write work?
* Why does RSS change?
* What memory is shared between processes?
* What does `mmap()` provide?
* How can PHP interact with native memory?

This project provides the low-level foundation for the rest of the lab.

---

## ⚡ PHP Concurrency

### `php-concurrency`

Exploring the fundamental building blocks of concurrency in PHP.

Topics include:

* processes
* `pcntl_fork()`
* IPC
* pipes
* Unix sockets
* `socket_pair()`
* worker processes
* process supervision
* event loops
* Fibers
* asynchronous programming
* Amp / Revolt
* ReactPHP
* concurrency patterns
* at-least-once execution
* idempotency

The project deliberately contains small independent experiments.

Each experiment should be easy to run, inspect, modify, and break.

### Core questions

* What is concurrency?
* How is concurrency different from parallelism?
* How can PHP processes communicate?
* How do event loops work?
* What problem do Fibers solve?
* How do asynchronous runtimes avoid blocking?
* How can multiple operations execute concurrently?

---

## ⚙️ PHP Worker Pool

### `php-worker-pool`

An educational implementation of a persistent multi-process worker pool.

The project explores how a pool of long-lived PHP processes can execute work concurrently.

Topics include:

* master/worker architecture
* worker lifecycle
* process supervision
* IPC
* Unix sockets
* `stream_select()`
* request dispatching
* worker states
* graceful shutdown
* draining
* timeouts
* backpressure
* failure handling

### Worker lifecycle

```text
STARTING
    ↓
IDLE
    ↓
BUSY
    ↓
DRAINING
    ↓
STOPPING
    ↓
DEAD
```

The pool also explores transitions between:

```text
IDLE ⇄ BUSY
```

### Core questions

* How does a master process communicate with workers?
* How should workers be supervised?
* What happens when a worker is shutting down?
* How can a pool drain gracefully?
* How should request and execution timeouts differ?
* How can a pool avoid losing work?

The project is intentionally educational rather than a production process manager.

---

## 📬 PHP Job Queue

### `php-job-queue`

An educational implementation of a reliable background job processing system.

Topics include:

* job producers
* workers
* queues
* reservations
* acknowledgements
* retries
* failure handling
* visibility timeouts
* idempotency
* at-least-once delivery
* dead-letter handling
* worker lifecycle
* graceful shutdown

### Core questions

* What does "reliable delivery" actually mean?
* What happens when a worker crashes?
* When should a job become visible again?
* Why is at-least-once delivery common?
* Why must jobs be idempotent?
* What happens between receiving and acknowledging a job?

The project connects the process/concurrency concepts with real backend infrastructure patterns.

---

## 💾 PHP Mini Cache

### `php-mini-cache`

An educational event-driven in-memory cache/database server.

The project explores the architecture behind systems such as Redis without trying to reproduce Redis itself.

Topics include:

* TCP networking
* sockets
* event loops
* non-blocking I/O
* command parsing
* request processing
* response buffering
* pipelining
* TTL
* expiration
* pub/sub
* backpressure
* batching
* event-loop fairness

### Core questions

* How does an event-driven server process thousands of connections?
* How does non-blocking I/O work?
* How should an event loop schedule work?
* What happens when clients send large pipelines?
* How should responses be buffered?
* How can TTL expiration be implemented?
* What happens when one client becomes too expensive?

The goal is to understand the mechanics behind event-driven network servers.

---

## 🌐 PHP Mini HTTP Server

### `php-mini-http-server`

An educational event-driven HTTP server written in PHP.

Topics include:

* TCP connections
* HTTP parsing
* request lifecycle
* routing
* middleware
* keep-alive
* HTTP pipelining
* response generation
* connection management
* timeouts
* backpressure
* non-blocking sockets
* event loops

### Core questions

* How does an HTTP server actually work?
* How is an HTTP request parsed?
* How does keep-alive work?
* What happens when several requests arrive on one connection?
* How does a server manage thousands of sockets?
* Where should timeouts be enforced?
* What happens when a client is slower than the server?

This project builds the networking layer that sits between low-level event-driven I/O and application-level HTTP processing.

---

## 🗄️ PHP Mini Database

### `php-mini-database`

A small, readable relational database written in PHP.

The project explores database internals from the ground up.

Topics include:

* pages
* records
* storage
* tables
* schemas
* indexes
* B-trees
* SQL parsing
* query execution
* transactions
* WAL
* crash recovery
* locking
* durability
* client/server architecture
* authentication

### Database layers

```text
SQL
 ↓
Parser
 ↓
Query Planner / Executor
 ↓
Transactions
 ↓
Storage Engine
 ↓
Pages / Records
 ↓
Disk
```

### Core questions

* How are database records stored?
* Why are indexes needed?
* How does a B-tree work?
* How does SQL become executable operations?
* What does a transaction actually provide?
* Why does a database need a WAL?
* How can a database recover after a crash?
* How do durability and atomicity interact?

The project is intentionally small enough to understand while still exposing the major architectural ideas behind relational databases.

---

## 🏗️ PHP Systems Platform

### `php-systems-platform`

The final integration layer of PHP Systems Lab.

Instead of implementing another isolated system, this project brings the mechanisms and components from the previous projects together into one backend platform.

The platform is where the individual experiments become a complete system.

Potential integrated components include:

* HTTP server
* application runtime
* worker pool
* job queue
* cache
* database
* background workers
* IPC
* event-driven networking
* observability
* configuration
* graceful shutdown
* failure handling

### Architectural goal

```text
                    🌐 HTTP
                      │
                      ▼
              🏗️ Application
                 Runtime
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
       💾 Cache     🗄️ DB      📬 Jobs
          │           │           │
          │           │           ▼
          │           │      ⚙️ Workers
          │           │           │
          └───────────┼───────────┘
                      │
                      ▼
                🧠 System Runtime
```

### Core questions

* How do the individual systems interact?
* Where should process boundaries exist?
* When should work be synchronous or asynchronous?
* How should failures propagate?
* How should backpressure move through the system?
* How should shutdown work across multiple components?
* How do storage, networking, workers, and queues form one platform?

This is the final step of the lab:

> **From understanding individual mechanisms to understanding a complete backend system.**

---

# 🔀 Two Major Concurrency Models

The lab deliberately explores two fundamentally different approaches to concurrency.

## ⚙️ Multi-Process

Primarily explored through:

```text
php-memory-lab
        ↓
php-concurrency
        ↓
php-worker-pool
        ↓
php-job-queue
```

The model is based on:

```text
Process
   ↓
IPC
   ↓
Worker
   ↓
Task
```

This provides a practical understanding of:

* process isolation
* parallel execution
* IPC
* supervision
* worker lifecycle
* graceful shutdown

---

## ⚡ Event-Driven

Primarily explored through:

```text
php-concurrency
        ↓
php-mini-cache
        ↓
php-mini-http-server
```

The model is based on:

```text
Socket
   ↓
Event Loop
   ↓
Ready Event
   ↓
Handler
   ↓
Response
```

This provides a practical understanding of:

* non-blocking I/O
* event loops
* connection management
* high concurrency
* backpressure
* asynchronous execution

---

# 🔗 How the Projects Connect

The projects are not independent tutorials.

Each one introduces mechanisms that become useful in later projects.

```text
🧠 Memory
   │
   ├── processes
   ├── virtual memory
   ├── COW
   └── IPC foundations
          │
          ▼
⚡ Concurrency
   │
   ├── fork()
   ├── IPC
   ├── event loops
   └── Fibers
          │
          ├──────────────────┐
          ▼                  ▼
⚙️ Worker Pool          💾 Mini Cache
          │                  │
          ▼                  ▼
📬 Job Queue          🌐 Mini HTTP Server
          │                  │
          └────────┬─────────┘
                   │
                   ▼
             🗄️ Mini Database
                   │
                   └──────────────┐
                                  │
                                  ▼
                       🏗️ Systems Platform
                                  │
                                  ▼
                            🚀 Final System
```

The same concepts appear repeatedly in different contexts.

For example:

```text
IPC
 │
 ├── Worker Pool
 │
 └── Job Queue

Event Loop
 │
 ├── Mini Cache
 │
 └── Mini HTTP Server

Storage
 │
 └── Mini Database

All of them
 │
 └── Systems Platform
```

This repetition is intentional.

The objective is to recognize the same systems principles when they appear in different architectures.

---

# 🎓 Suggested Learning Path

The recommended progression is:

## Level 1 — 🧠 Memory & OS Fundamentals

### `php-memory-lab`

Learn:

* memory
* processes
* virtual memory
* COW
* `mmap()`
* RSS
* shared memory

---

## Level 2 — ⚡ Concurrency Fundamentals

### `php-concurrency`

Learn:

* processes
* IPC
* event loops
* Fibers
* asynchronous programming
* concurrency patterns

---

## Level 3 — ⚙️ Process Runtime

### `php-worker-pool`

Learn:

* master/worker architecture
* persistent workers
* supervision
* lifecycle management
* graceful shutdown
* timeouts
* backpressure

---

## Level 4 — 📬 Background Processing

### `php-job-queue`

Learn:

* asynchronous jobs
* reservations
* ACK
* retries
* idempotency
* at-least-once processing
* failure recovery

---

## Level 5 — 💾 Event-Driven Server

### `php-mini-cache`

Learn:

* TCP
* non-blocking I/O
* event loops
* protocol processing
* pipelining
* TTL
* response buffering
* backpressure

---

## Level 6 — 🌐 HTTP Server Runtime

### `php-mini-http-server`

Learn:

* HTTP parsing
* routing
* middleware
* keep-alive
* pipelining
* connection lifecycle
* HTTP timeouts

---

## Level 7 — 🗄️ Database Engine

### `php-mini-database`

Learn:

* storage
* records
* indexes
* B-trees
* SQL
* transactions
* WAL
* crash recovery
* database networking

---

## Level 8 — 🏗️ Final Platform

### `php-systems-platform`

Bring everything together.

Learn:

* system integration
* component boundaries
* runtime architecture
* synchronous vs asynchronous execution
* queues
* workers
* caching
* persistence
* networking
* failure handling
* graceful shutdown
* observability

The final objective is not another isolated component.

It is understanding how the components form a **complete backend platform**.

---

# 🔬 Learn by Experimenting

The projects are designed to be actively experimented with.

Do not only read the code.

Change it.

Break it.

Measure it.

Try things such as:

```text
What happens if...
```

* a worker crashes?
* a socket closes unexpectedly?
* a client sends a huge pipeline?
* a job is processed twice?
* an ACK is lost?
* a database process crashes during a transaction?
* a worker stops while processing a request?
* the event loop is blocked?
* memory usage grows continuously?
* a client is much slower than the server?
* the queue becomes full?

The interesting part is often not the normal path.

The interesting part is what happens when things go wrong.

---

# 🧪 Production-Inspired, Not Production-Ready

These projects intentionally resemble real backend infrastructure.

They may contain concepts found in:

* Redis
* PostgreSQL
* RabbitMQ
* PHP-FPM
* Swoole
* RoadRunner
* FrankenPHP
* asynchronous runtimes
* distributed systems

But they are **not intended to replace those systems**.

They intentionally sacrifice:

* feature completeness
* performance
* operational maturity
* security hardening
* production compatibility
* ecosystem integration

in favor of:

* readability
* experimentation
* explicit mechanisms
* small implementations
* understandable architecture

The goal is not:

> "Build a better Redis."

The goal is:

> "Understand why Redis needs an event loop."

The goal is not:

> "Build a production database."

The goal is:

> "Understand how storage, indexes, transactions, and recovery fit together."

---

# 🧰 Shared Conventions

The projects generally follow the same development conventions.

Typical tooling includes:

* Docker
* Makefile
* Composer
* PHPUnit
* PHPStan
* PHP-CS-Fixer
* GitHub Actions

Typical commands:

```bash
make install
make test
make analyse
make format-check
make format
make shell
```

Project-specific commands may differ.

---

# 📖 Documentation

Projects use documentation to explain the mechanisms rather than simply documenting APIs.

Typical documentation includes:

```text
README.md
docs/
├── DECISIONS.md
├── PHASES.md
└── ...
```

Important architectural decisions should be documented.

The purpose is to preserve the reasoning behind the implementation.

---

# 📦 These Are Not Libraries

The repositories are intentionally built as educational systems.

They should be:

```text
Read
 ↓
Run
 ↓
Measure
 ↓
Modify
 ↓
Break
 ↓
Debug
 ↓
Understand
```

They are not intended to become dependencies for production applications.

If a production-ready library is needed, use an established project.

If the goal is to understand how the mechanism works, build the simplified version.

---

# 🧠 The Core Philosophy

The lab follows a simple idea:

> **Small systems are easier to understand than large abstractions.**

Instead of hiding complexity behind frameworks, the projects expose it.

Instead of:

```text
Framework
    ↓
Magic
    ↓
Application
```

the lab tries to show:

```text
Operating System
        ↓
Processes
        ↓
Memory / IPC
        ↓
Event Loop / Sockets
        ↓
Workers
        ↓
Storage
        ↓
Protocols
        ↓
Application
        ↓
Complete System
```

The objective is to understand the layers underneath modern backend frameworks.

---

# 🏗️ The Main Principle

The lab follows a progression:

```text
Understand the mechanism
        ↓
Implement the mechanism
        ↓
Combine mechanisms
        ↓
Observe emergent problems
        ↓
Solve the problems
        ↓
Build a subsystem
        ↓
Integrate subsystems
        ↓
Build a complete system
```

For example:

```text
fork()
 ↓
IPC
 ↓
Worker Pool
 ↓
Job Queue
 ↓
Background Processing
 ↓
Integrated Platform
```

Or:

```text
socket()
 ↓
non-blocking I/O
 ↓
event loop
 ↓
protocol parser
 ↓
HTTP server
 ↓
application runtime
 ↓
integrated platform
```

---

# 🔭 Future Directions

The lab can continue evolving toward more advanced systems topics.

Possible future experiments include:

* distributed coordination
* replication
* consensus concepts
* sharding
* distributed locks
* service discovery
* rate limiting
* circuit breakers
* observability
* metrics
* tracing
* load balancing
* connection pooling
* caching strategies
* persistent queues
* database replication
* fault injection
* benchmarking
* Linux networking
* kernel interaction
* native extensions

These should be added only when they provide a meaningful new systems concept.

The project should avoid growing simply for the sake of adding features.

---

# 🗺️ The Mental Map

The entire lab can be reduced to one mental model:

```text
                         🧠 MEMORY
                            │
                            ▼
                       ⚡ PROCESSES
                            │
                            ▼
                       🔀 CONCURRENCY
                       /            \
                      /              \
                     ▼                ▼
              ⚙️ WORKERS          ⚡ EVENT LOOP
                  │                    │
                  ▼                    ▼
             📬 JOB QUEUE        🌐 NETWORKING
                                       │
                              ┌────────┴────────┐
                              ▼                 ▼
                          💾 CACHE           🌐 HTTP
                              │                 │
                              └────────┬────────┘
                                       │
                                       ▼
                                  🗄️ DATABASE
                                       │
                                       ▼
                              🏗️ INTEGRATION
                                       │
                                       ▼
                              🚀 FINAL PLATFORM
```

This is the conceptual map of the repository.

The projects are different implementations of the same underlying systems ideas.

---

# 🚀 From Mechanisms to Systems

The most important transition happens near the end of the learning path.

At first, the questions are local:

```text
How does fork() work?
How does mmap() work?
How does IPC work?
How does an event loop work?
How does a B-tree work?
```

Later, the questions become architectural:

```text
Where should processes live?

What should be asynchronous?

Where should backpressure be applied?

How should failures propagate?

How should components communicate?

How should the system shut down?

What happens when one subsystem becomes overloaded?

How do persistence and concurrency interact?
```

Finally:

```text
How do all of these mechanisms form one coherent system?
```

That is the purpose of `php-systems-platform`.

---

# 🎯 Final Principle

> **Do not learn backend infrastructure only by using it. Learn it by rebuilding simplified versions of it.**

The purpose of PHP Systems Lab is to make invisible mechanisms visible.

Memory becomes measurable.

Processes become observable.

Concurrency becomes executable.

Queues become understandable.

Event loops become inspectable.

Protocols become parsable.

Storage becomes tangible.

Databases become buildable.

And eventually:

```text
                         🧪 PHP SYSTEMS LAB
                                  │
        ┌─────────────────────────┼─────────────────────────┐
        │                         │                         │
     🧠 Memory                ⚡ Concurrency             🌐 Network
        │                         │                         │
        │              ┌──────────┴──────────┐              │
        │              │                     │              │
        ▼              ▼                     ▼              ▼
     Processes      ⚙️ Workers          📬 Jobs          💾 Cache
        │              │                     │              │
        │              └──────────┬──────────┘              │
        │                         │                         │
        └─────────────────────────┼─────────────────────────┘
                                  │
                                  ▼
                           🗄️ Database
                                  │
                                  ▼
                         🏗️ Systems Platform
                                  │
                                  ▼
                            🚀 Final System
```

**Understand the mechanism.
Build the mechanism.
Connect the mechanisms.
Understand the system.**

---

# 📄 License

All projects in PHP Systems Lab are released under the **MIT License**.
