# Performance Analysis of Matrix Multiplication

![Course](https://img.shields.io/badge/Course-Parallel%20%26%20GPU%20Computing-blue.svg)
![Workload](https://img.shields.io/badge/Workload-4000x4000%20Matrix%20Multiplication-orange.svg)
![Models](https://img.shields.io/badge/Models-Sequential%20%7C%20OpenMP%20%7C%20MPI%20%7C%20CUDA-green.svg)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen.svg)

---

## Executive Summary

This project presents a performance analysis of **4000 × 4000 matrix multiplication** using four different computing approaches:

* Sequential CPU
* OpenMP
* MPI
* CUDA

The same input matrices are used in all implementations. Every element of matrices `A` and `B` is initialized to `1.0`.

Therefore, the expected result is:

```text
C[i][j] = 4000.00
```

The measured execution times are:

| Model          | Execution Time |  Speedup |
| -------------- | -------------: | -------: |
| Sequential CPU |       244.12 s |    1.00× |
| OpenMP         |        30.83 s |    7.92× |
| MPI            |        92.98 s |    2.63× |
| CUDA           |        0.165 s | 1479.48× |

---

## 1. Experiment Objectives

The objectives of this experiment are:

1. Implement matrix multiplication using Sequential, OpenMP, MPI, and CUDA.
2. Use the same `4000 × 4000` matrix workload for all implementations.
3. Measure the execution time of each implementation.
4. Calculate the speedup achieved by parallel implementations.
5. Verify the correctness of the output.
6. Compare shared-memory, distributed-memory, and GPU-based parallel computing.

---

## 2. Workload Specification

### Matrix Size

```text
N = 4000
```

The matrices are:

```text
A = 4000 × 4000
B = 4000 × 4000
C = 4000 × 4000
```

### Input Initialization

```text
A[i][j] = 1.0
B[i][j] = 1.0
```

### Matrix Multiplication

The matrix multiplication is performed using:

```text
C[i][j] = Σ A[i][k] × B[k][j]
```

Since every element of `A` and `B` is `1.0`:

```text
C[i][j] = 4000.0
```

### Verification

```text
C[0][0] = 4000.00
```

This value is used to verify the correctness of the output.

---

## 3. Computing Models

### 3.1 Sequential CPU

The Sequential implementation uses a traditional triple nested loop:

```text
for i
    for j
        for k
            C[i][j] += A[i][k] × B[k][j]
```

Only one CPU thread performs the computation.

**Characteristics:**

* Single CPU thread
* No parallel processing
* Simple implementation
* Used as the performance baseline

---

### 3.2 OpenMP

OpenMP uses multiple CPU threads to perform the matrix multiplication in parallel.

The main parallel loop uses:

```c
#pragma omp parallel for
```

The experiment uses **8 CPU threads**.

**Characteristics:**

* Shared-memory parallelism
* Multiple CPU threads
* Easy to implement
* Suitable for multi-core processors

---

### 3.3 MPI

MPI stands for **Message Passing Interface**.

The MPI implementation distributes the matrix multiplication among multiple processes.

The experiment uses:

```text
4 MPI ranks
```

The major MPI operations are:

```text
MPI_Scatter
MPI_Bcast
MPI_Gather
```

**MPI_Scatter:** Distributes portions of matrix `A`.

**MPI_Bcast:** Sends matrix `B` to all MPI processes.

**MPI_Gather:** Collects the calculated results.

**Characteristics:**

* Distributed-memory parallelism
* Multiple processes
* Can run across multiple machines
* Communication occurs between processes

---

### 3.4 CUDA

CUDA is used to perform matrix multiplication on an NVIDIA GPU.

The GPU uses a large number of threads to calculate the output matrix in parallel.

### CUDA Configuration

```text
Block size = 16 × 16
Threads per block = 256
Grid size = 250 × 250
```

Total logical GPU threads:

```text
250 × 250 × 16 × 16

= 16,000,000 threads
```

Each GPU thread is responsible for calculating an output matrix element.

---

## 4. Repository Structure

```text
project/
│
├── README.md
│
├── src/
│   │
│   ├── sequential/
│   │   └── matrix_sequential.c
│   │
│   ├── openmp/
│   │   └── matrix_openmp.c
│   │
│   ├── mpi/
│   │   ├── matrix_mpi.c
│   │   └── mpi_send_recv.c
│   │
│   └── cuda/
│       └── matrix_cuda.cu
│
└── images/
    ├── sequential_result.png
    ├── openmp_htop.png
    ├── mpi_ping.png
    ├── mpi_send_recv.png
    ├── mpi_result.png
    ├── performance_comparison_charts.png
    ├── execution_time_chart.png
    └── speedup_chart.png
```

---

# 5. Source Code Files

| Computing Model | Source File                          | Description                       |
| --------------- | ------------------------------------ | --------------------------------- |
| Sequential      | `src/sequential/matrix_sequential.c` | Sequential matrix multiplication  |
| OpenMP          | `src/openmp/matrix_openmp.c`         | Multi-threaded CPU implementation |
| MPI             | `src/mpi/matrix_mpi.c`               | Distributed matrix multiplication |
| MPI Test        | `src/mpi/mpi_send_recv.c`            | MPI communication test            |
| CUDA            | `src/cuda/matrix_cuda.cu`            | GPU-based matrix multiplication   |

---

# 6. Compilation and Execution

## 6.1 Sequential

Compile:

```bash
gcc src/sequential/matrix_sequential.c -o sequential
```

Run:

```bash
./sequential
```

---

## 6.2 OpenMP

Compile:

```bash
gcc -fopenmp src/openmp/matrix_openmp.c -o openmp
```

Run:

```bash
./openmp
```

To use 8 threads:

### Linux

```bash
export OMP_NUM_THREADS=8
./openmp
```

### Windows PowerShell

```powershell
$env:OMP_NUM_THREADS=8
.\openmp.exe
```

---

## 6.3 MPI

Compile:

```bash
mpicc src/mpi/matrix_mpi.c -o matrix_mpi
```

Run using 4 processes:

```bash
mpirun -np 4 ./matrix_mpi
```

For Windows:

```powershell
mpiexec -n 4 .\matrix_mpi.exe
```

---

## 6.4 MPI Send/Receive Test

Compile:

```bash
mpicc src/mpi/mpi_send_recv.c -o mpi_send_recv
```

Run:

```bash
mpirun -np 4 ./mpi_send_recv
```

This program verifies communication using:

```c
MPI_Send()
MPI_Recv()
```

---

## 6.5 CUDA

Compile:

```bash
nvcc src/cuda/matrix_cuda.cu -o matrix_cuda
```

Run:

```bash
./matrix_cuda
```

For Windows:

```powershell
nvcc src\cuda\matrix_cuda.cu -o matrix_cuda.exe
.\matrix_cuda.exe
```

---

# 7. Performance Results

The following results were obtained for the `4000 × 4000` matrix multiplication.

| Model      | Architecture       | Active Resources | Execution Time |  Speedup | Verification |
| ---------- | ------------------ | ---------------- | -------------: | -------: | -----------: |
| Sequential | Single CPU         | 1 thread         |       244.12 s |    1.00× |      4000.00 |
| OpenMP     | Shared-memory CPU  | 8 threads        |        30.83 s |    7.92× |      4000.00 |
| MPI        | Distributed memory | 4 ranks          |        92.98 s |    2.63× |      4000.00 |
| CUDA       | GPU                | NVIDIA GPU       |        0.165 s | 1479.48× |      4000.00 |

---

# 8. Speedup Calculation

Speedup is calculated using:

```text
Speedup = Sequential Execution Time / Parallel Execution Time
```

### OpenMP

```text
Speedup = 244.12 / 30.83

        ≈ 7.92×
```

### MPI

```text
Speedup = 244.12 / 92.98

        ≈ 2.63×
```

### CUDA

```text
Speedup = 244.12 / 0.165

        ≈ 1479.48×
```

---

# 9. Performance Analysis

## Sequential CPU

The Sequential implementation takes the longest time because only one CPU thread performs all matrix multiplication operations.

It provides the baseline for comparing the parallel implementations.

---

## OpenMP

OpenMP divides the computation among 8 CPU threads.

The execution time decreases from:

```text
244.12 seconds → 30.83 seconds
```

The measured speedup is:

```text
7.92×
```

OpenMP is suitable for systems with multiple CPU cores and shared memory.

---

## MPI

MPI distributes the computation among 4 processes.

The processes communicate using:

```text
MPI_Scatter
MPI_Bcast
MPI_Gather
```

The measured execution time is:

```text
92.98 seconds
```

The speedup is:

```text
2.63×
```

MPI communication introduces additional overhead because processes may need to exchange data.

---

## CUDA

CUDA performs the matrix multiplication on the GPU.

Matrix multiplication contains a large number of independent calculations, making it suitable for GPU parallelism.

The measured execution time is:

```text
0.165 seconds
```

The speedup is:

```text
1479.48×
```

---

# 10. Comparison of Computing Models

| Feature                | Sequential | OpenMP         | MPI                   | CUDA             |
| ---------------------- | ---------- | -------------- | --------------------- | ---------------- |
| Processing Unit        | CPU        | CPU            | Multiple CPUs/VMs     | GPU              |
| Memory                 | Single     | Shared         | Distributed           | GPU Memory       |
| Parallelism            | No         | Yes            | Yes                   | Yes              |
| Processes/Threads      | 1 thread   | 8 threads      | 4 ranks               | GPU threads      |
| Communication          | None       | Low            | Network/Process       | CPU-GPU transfer |
| Programming Complexity | Low        | Medium         | High                  | High             |
| Main Purpose           | Baseline   | Multi-core CPU | Distributed computing | GPU acceleration |

---

# 11. Advantages and Limitations

## Sequential

### Advantages

* Simple to implement
* Easy to understand
* Easy to debug
* No parallel communication overhead

### Limitations

* Slow for large matrices
* Uses only one CPU thread

---

## OpenMP

### Advantages

* Easy to parallelize
* Uses multiple CPU cores
* Shared memory simplifies programming
* Good for multi-core systems

### Limitations

* Limited by available CPU cores
* Mainly suitable for shared-memory systems

---

## MPI

### Advantages

* Supports distributed systems
* Can run across multiple machines
* Suitable for cluster computing
* Supports large-scale applications

### Limitations

* Communication overhead
* More complex programming
* Network performance affects execution time

---

## CUDA

### Advantages

* Massive parallelism
* Very fast for suitable workloads
* Suitable for numerical computations
* Uses thousands of GPU threads

### Limitations

* Requires a compatible NVIDIA GPU
* More complex programming
* CPU-GPU data transfer introduces overhead

---

# 12. Correctness Verification

All implementations use the same input initialization:

```text
A[i][j] = 1.0
B[i][j] = 1.0
```

Therefore:

```text
C[i][j] = 4000.0
```

The verification result is:

```text
Expected Result : 4000.00
Obtained Result : 4000.00
Status          : PASS
```

This confirms that the matrix multiplication produces the expected result.

---

# 13. Performance Comparison

### Execution Time

```text
Sequential : 244.12 seconds
OpenMP     : 30.83 seconds
MPI        : 92.98 seconds
CUDA       : 0.165 seconds
```

### Speedup

```text
Sequential : 1.00×
OpenMP     : 7.92×
MPI        : 2.63×
CUDA       : 1479.48×
```

The experiment shows that parallel processing can significantly reduce execution time for computationally intensive workloads.

---

# 14. Screenshots

## Sequential Execution

![Sequential Execution Result](images/sequential_result.png)

---

## OpenMP Execution

![OpenMP htop Execution](images/openmp_htop.png)

---

## MPI Network Verification

![MPI Ping Test](images/mpi_ping.png)

---

## MPI Send/Receive Verification

![MPI Send Recv Verification](images/mpi_send_recv.png)

---

## MPI Matrix Multiplication

![MPI Matrix Multiplication Result](images/mpi_result.png)

---

## Performance Comparison

![Performance Comparison Charts](images/performance_comparison_charts.png)

---

## Execution Time Chart

![Execution Time Chart](images/execution_time_chart.png)

---

## Speedup Chart

![Speedup Chart](images/speedup_chart.png)

---

# 15. Key Observations

1. Sequential execution uses only one CPU thread.
2. OpenMP improves CPU performance by using multiple threads.
3. MPI distributes the workload among multiple processes.
4. MPI introduces communication overhead between processes.
5. CUDA uses GPU parallelism to perform many calculations simultaneously.
6. Matrix multiplication is highly suitable for parallel processing.
7. The same verification value is obtained from all implementations.
8. The CUDA implementation has the shortest measured execution time for this experiment.

---

# 16. Conclusion

This project implemented and analyzed `4000 × 4000` matrix multiplication using four different computing models: Sequential CPU, OpenMP, MPI, and CUDA.

The execution times obtained were:

```text
Sequential : 244.12 s
OpenMP     : 30.83 s
MPI        : 92.98 s
CUDA       : 0.165 s
```

The corresponding speedups were:

```text
Sequential : 1.00×
OpenMP     : 7.92×
MPI        : 2.63×
CUDA       : 1479.48×
```

All implementations produced the expected verification result:

```text
C[0][0] = 4000.00
```

The experiment demonstrates the importance of parallel computing for large computational workloads. OpenMP provides CPU-based parallelism, MPI provides distributed-memory parallelism, and CUDA provides GPU-based massive parallelism.

---

