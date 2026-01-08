## 1. Fault Tolerance

**Fault tolerance = keep working even when things break.**

It’s about **behavior during failure**.

If a server crashes, a network drops, or a dependency is down, the system **continues operating** (maybe in a reduced mode).


**Example**

* One Node.js service instance crashes
* Load balancer routes traffic to other instances
* Users don’t see an outage

**Key idea**
Failure happens → system survives.

---

## 2. Reliability

**Reliability = how consistently the system works over time.**

It’s a **probability and time-based concept**.

Usually measured as:

* Uptime percentage (99.9%, 99.99%)
* Mean Time Between Failures (MTBF)

**Example**

* A service that runs for months without crashing
* Few bugs, predictable behavior

**Key idea**
How often does the system fail at all?

---

## 3. Redundancy

**Redundancy = having backups.**

It’s a **design technique**, not a behavior.

You add extra components so one can replace another if it fails.


**Examples**

* Two or more servers instead of one
* Primary + replica database
* Multiple availability zones

**Key idea**
One component fails → another is available.

---

## Putting them together (this is the important part)

| Concept         | What it answers                |
| --------------- | ------------------------------ |
| Redundancy      | Do we have backups?            |
| Fault Tolerance | Can we survive failures?       |
| Reliability     | How often does failure happen? |

---

## Relationship (one builds on the other)

* **Redundancy** is often required to achieve **fault tolerance**
* **Fault tolerance** improves **reliability**
* But none of them automatically guarantee the others

### Example that makes it obvious

* System with **redundancy only**

  * Two servers
  * No failover logic
  * One dies → traffic still goes to dead server
    ❌ Redundant but not fault tolerant

* System with **fault tolerance**

  * Health checks
  * Load balancing
  * Automatic failover
    ✅ Survives failures

* System with **high reliability**

  * Rarely fails
  * But when it fails, it goes completely down
    ❌ Reliable but not fault tolerant

---

## One-line interview answers

* **Redundancy**: adding extra components to avoid single points of failure
* **Fault tolerance**: ability to continue operating despite component failures
* **Reliability**: likelihood that a system performs correctly over time

---

## Backend / Node.js mental model

* Redundancy → multiple Node instances, DB replicas
* Fault tolerance → retries, timeouts, circuit breakers, queues
* Reliability → good code, monitoring, low bug rate, stable infra

