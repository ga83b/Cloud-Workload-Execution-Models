# Cloud Workload Execution Models: Container vs FaaS

An OMNeT++ discrete-event simulation comparing Container-based and Function-as-a-Service (FaaS) workload scheduling in cloud environments. Validated analytically against an M/M/c (Erlang-C) queuing model.

Developed as part of the Performance Evaluation of Computer Systems and Networks course at the University of Pisa (M.Sc. AI & Data Engineering).

## Core Architecture
The system abstracts a hardware cluster into a Single Virtual Server with continuous fluid capacity C. It handles requests using a strict First-Come-First-Served (FCFS) infinite queue:
*   **Container model:** Tasks run inside a long-term container with fixed setup overhead (2 * I_cont).
*   **FaaS model:** Workloads are split into K sequential functions, each triggering a lightweight container boot (2 * K * I_FaaS).

To optimize execution, user populations (N) are mathematically modeled via a single Aggregated User Source Poisson stream.

## Verification & Testing Suite
The simulator passed five rigorous verification stages to ensure stability:
1.  **Degeneracy:** System stability confirmed at boundary limits (N=0, C=0, R>C).
2.  **Consistency:** Time-scaling inputs by a factor of k correctly scaled time metrics while keeping utilization invariant.
3.  **Continuity/Monotonicity:** Paired T-Tests proved strict positive delay growth when scaling from N=64 to N=65 users.
4.  **Theoretical Validation:** Simulation vs Erlang-C formulas matched with a 0.08% relative error margin.
5.  **Memory Profile:** Zero leaks detected under Valgrind profiling.

## Simulation Parameters (Scaled Model alpha=1000)
*   **Total Capacity (C):** 40 GHz (20 vCPUs)
*   **Workload Size (R):** U[0.5, 1.5] GHz
*   **Task Duration (D):** Exponential (E[D] = 5.0s)
*   **Inter-arrival (T):** Exponential (E[T] = 10.0s)
*   **Overheads:** I_cont = 0.8s (Docker-like), I_FaaS = 0.125s (Firecracker MicroVM)
*   **Sweep Range:** N in [20, 90] users, K in [2, 8] functions
*   **Warm-up:** 500 seconds

## Key Takeaway
FaaS scheduling is highly sensitive to fragmentation (K). As K grows, cumulative initialization overheads accelerate system saturation, making monolithic containers more efficient under high concurrency.

## Getting Started

### Prerequisites
The project requires an operational installation of the **OMNeT++** discrete event simulator environment.
