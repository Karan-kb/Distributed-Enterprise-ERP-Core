# 🏢 Multi-Tenant Database Framework with Dynamic Schema Routing

This repository contains the architecture and implementation code for a PHP-based database routing engine. The project serves as a practical testbed to analyze query performance, latency shifts, and structural limits when isolating tenant data across individual database schemas under concurrent workloads.

---

### 📐 Request Routing & Architecture

```mermaid
graph TD
    A[Load Testing Client] --> B[Nginx Reverse Proxy]
    B --> C[Tenant Identification Middleware]
    C --> D{Database Router Core}
    D -->|Tenant Alpha| E[(Isolated Target DB Schema A)]
    D -->|Tenant Beta| F[(Isolated Target DB Schema B)]
    E --> G[Query Optimization Layer]
    F --> G
    G --> H[Redis Queue / Cache]
    G --> I[Performance Metrics Log]
```

---

## 🔬 Core System Architecture Features

Building software for multi-tenant environments requires careful balancing between strict database isolation requirements and hardware resource consumption under load.

### 1. Dynamic Data Isolation (Database-Per-Tenant Model)
This framework maps and separates user information into dedicated database schemas at runtime instead of grouping all tenant records into a single global data pool:
* **Runtime Workspace Mapping:** Incoming API requests pass through a middleware layer that detects the client context properties dynamically, keeping individual data spaces safely separated.
* **Data Security Baseline:** Separating data paths at the database layer entirely prevents accidental cross-tenant data leaks, offering a reliable model for zero-trust local testing environments.

### 2. Slow Query Minimization \& Task Buffering
To handle heavy multi-user traffic spikes without locking database tables, the system uses targeted relational design patterns and in-memory key-value queues:
* **Relational Performance Tuning:** Implements MySQL table partitioning layouts and custom indexing layouts to optimize query lookups during heavy write loads.
* **Background Task Offloading:** Uses **Redis task queues** to sequence data operations orderly, preserving target database thread pools during intense background processing loads.

---

## 📊 Performance Benchmarks & Testing Results

The system routing core was stress-tested using automated traffic simulation scripts to document execution trends and pinpoint hardware limit margins.

### 1. Performance Limits
* **Routing Overhead:** System tenant schema detection and connection mapping calculations run efficiently, resolving at an O(1) time complexity cost.
* **Workload Capacity:** Tested to securely process steady workloads of **10,000+ daily requests** without causing concurrent database deadlocks or query context drops.

### 2. Planned Development Extensions
* Incorporate reliable cluster management simulations (e.g., experimenting with **Raft** consensus mechanisms) to evaluate database state recovery patterns across multi-node environments.
* Test database sharding strategies across distinct network nodes to study connection packet latency over wide area configurations.

---

## 🛠️ Repository Directory Layout

The codebase separates infrastructure setup, request handling pipelines, and indexing configurations into clean layers:
*   📁 **`src/Core/`** — Core classes handling the dynamic query context routing logic.
*   📁 **`src/Database/`** — Index layout migration profiles, schema triggers, and sandbox data seeders.
*   📁 **`config/`** — Global environmental variables, Redis worker parameters, and proxy cache paths.

---

## ⚙️ Development Stack Primitives

* **Language Engine:** PHP 8.x (Custom performance optimization settings)
* **In-Memory Cache:** Redis (Asynchronous background task processing queue)
* **Relational Database:** MySQL (Table partitioning layout setups)
* **Web Server Proxy:** Nginx (Virtual Private Server deployment proxy rules)

---

## 🚀 Setting Up the Local Testing Sandbox

### Step 1: Clone the Repository
```bash
git clone https://github.com
cd Distributed-Enterprise-ERP-Core
```

### Step 2: Configure Environment Settings
Create a standard configuration profile (`.env`) in your root folder and set your local database access paths and server credential blocks.

### Step 3: Run Database Migrations
Start your relational database services, spin up your Redis server instance, and execute the automated data table migrations:
```bash
composer install
php artisan migrate --seed
```

---

## 👨‍💻 Maintainer

* **Karan B.C.** - Backend Software Engineer | [GitHub Profile](https://github.com) | [LinkedIn Profile](https://linkedin.com)
