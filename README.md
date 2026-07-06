# Cloud Workload Execution Models: Container vs FaaS

An OMNeT++ discrete-event simulation comparing Container-based and Function-as-a-Service (FaaS) workload scheduling in cloud environments. Validated analytically against an M/M/c (Erlang-C) queuing model[cite: 1].

Developed as part of the Performance Evaluation of Computer Systems and Networks course at the University of Pisa (M.Sc. AI & Data Engineering)[cite: 1].

## Core Architecture
The system abstracts a hardware cluster into a Single Virtual Server with continuous fluid capacity C[cite: 1]. It handles requests using a strict First-Come-First-Served (FCFS) infinite queue[cite: 1]:
*   **Container model:** Tasks run inside a long-term container with fixed setup overhead (2 * I_cont)[cite: 1].
*   **FaaS model:** Workloads are split into K sequential functions, each triggering a lightweight container boot (2 * K * I_FaaS)[cite: 1].

To optimize execution, user populations (N) are mathematically modeled via a single Aggregated User Source Poisson stream[cite: 1].

## Verification & Testing Suite
The simulator passed five rigorous verification stages to ensure stability[cite: 1]:
1.  **Degeneracy:** System stability confirmed at boundary limits (N=0, C=0, R>C)[cite: 1].
2.  **Consistency:** Time-scaling inputs by a factor of k correctly scaled time metrics while keeping utilization invariant[cite: 1].
3.  **Continuity/Monotonicity:** Paired T-Tests proved strict positive delay growth when scaling from N=64 to N=65 users[cite: 1].
4.  **Theoretical Validation:** Simulation vs Erlang-C formulas matched with a 0.08% relative error margin[cite: 1].
5.  **Memory Profile:** Zero leaks detected under Valgrind profiling[cite: 1].

## Simulation Parameters (Scaled Model alpha=1000)
*   **Total Capacity (C):** 40 GHz (20 vCPUs)[cite: 1]
*   **Workload Size (R):** U[0.5, 1.5] GHz[cite: 1]
*   **Task Duration (D):** Exponential (E[D] = 5.0s)[cite: 1]
*   **Inter-arrival (T):** Exponential (E[T] = 10.0s)[cite: 1]
*   **Overheads:** I_cont = 0.8s (Docker-like), I_FaaS = 0.125s (Firecracker MicroVM)[cite: 1]
*   **Sweep Range:** N in [20, 90] users, K in [2, 8] functions[cite: 1]
*   **Warm-up:** 500 seconds[cite: 1]

## Key Takeaway
FaaS scheduling is highly sensitive to fragmentation (K)[cite: 1]. As K grows, cumulative initialization overheads accelerate system saturation, making monolithic containers more efficient under high concurrency[cite: 1].

## Getting Started

### Prerequisites
The project requires an operational installation of the **OMNeT++** discrete event simulator environment.
