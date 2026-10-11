# Parallel Sum and Average of Dataset Using OpenMP

A parallel computing project that calculates the sum and average of a dataset using sequential execution and OpenMP parallelization, and evaluates performance for different dataset sizes and thread counts.

---

## Team Members

| Name | USN |
|---|---|
|Anupriya S  |01FE24BCI046 |
|Pavitra J |01FE24BCI049 |
|Anushree A  |01FE24BCI050  |
|Sahana A |01FE24BCI030  |

## 1. Objective

To implement sequential and parallel sum and average calculations using OpenMP and analyze the effect of different dataset sizes and thread counts on execution time, speedup, and parallel efficiency.

## 2. Project Description

This project demonstrates the use of OpenMP to parallelize the summation of dataset elements.

The program:
- Generates a dataset containing consecutive integers from 1 to N.
- Calculates the sum and average sequentially.
- Calculates the sum using OpenMP parallel execution.
- Tests multiple thread counts.
- Measures execution time.
- Calculates speedup and efficiency.
- Compares sequential and parallel performance.

## 3. Technologies Used

- **Programming language:** C
- **Parallel programming:** OpenMP
- **Compiler:** GCC
- **Operating system:** Ubuntu
- **Virtual environment:** VMware
- **Graphs:** Python and Matplotlib
- **Version control:** Git and GitHub

## 4. Repository Structure

```text
openmp-sum-average/
├── README.md
├── src/
│   └── sum_average.c
├── data/
│   └── generate_dataset.py
├── results/
│   ├── results.csv
│   └── results.txt
├── graphs/
│   ├── execution_time.png
│   ├── speedup.png
│   └── efficiency.png
├── screenshots/
│   ├── run_100000.png
│   ├── run_1000000.png
│   └── run_10000000.png
├── report/
│   └── experiment_report.md
└── presentation/
    └── Parallel_Sum_Average_OpenMP_Project.pptx
```

## 5. Experimental Configuration

### Dataset Sizes

- 100,000 elements
- 1,000,000 elements
- 10,000,000 elements

### Thread Counts

- 1 thread
- 2 threads
- 4 threads
- 8 threads
- 16 threads
- 32 threads

## 6. Compilation and Execution

### Install Required Packages

```bash
sudo apt update
sudo apt install build-essential python3-matplotlib -y
```

### Compile the C Program

Run this command from the repository root:

```bash
gcc -O2 -fopenmp src/sum_average.c -o sum_average
```

### Execute the Program

```bash
./sum_average
```

When prompted, enter one of the supported dataset sizes:

```text
100000
1000000
10000000
```

The program displays the sequential results followed by the parallel results for the selected dataset size.

## 7. Experimental Results and Graphs

The following graphs compare execution time, speedup, and parallel efficiency for the three dataset sizes.

### 7.1 Execution Time vs Number of Threads

This graph compares the measured OpenMP execution time for different thread counts.

![Execution Time vs Number of Threads](graphs/execution_time.png)

### 7.2 Speedup vs Number of Threads

Speedup measures how much faster the parallel implementation runs compared with the sequential baseline.

**Formula:**

`Speedup = Sequential Execution Time / Parallel Execution Time`

![Speedup vs Number of Threads](graphs/speedup.png)

### 7.3 Parallel Efficiency vs Number of Threads

Parallel efficiency measures speedup relative to the number of threads used.

**Formula:**

`Efficiency (%) = (Speedup / Number of Threads) × 100`

![Parallel Efficiency vs Number of Threads](graphs/efficiency.png)

## 8. Experimental Output Screenshots

The following screenshots document the program execution in the Ubuntu environment.

### Experiment 1: Dataset Size = 100,000

![Terminal output for 100000 elements](screenshots/run_100000.png)

### Experiment 2: Dataset Size = 1,000,000

![Terminal output for 1000000 elements](screenshots/run_1000000.png)

### Experiment 3: Dataset Size = 10,000,000

![Terminal output for 10000000 elements](screenshots/run_10000000.png)

## 9. Results Summary

The following table summarizes the sequential baseline and the four-thread result from each recorded experiment.

| Dataset Size | Threads | Sequential Time (s) | Parallel Time (s) | Speedup | Efficiency |
|---:|---:|---:|---:|---:|---:|
| 100,000 | 4 | 0.000086 | 0.002205 | 0.0390× | 0.98% |
| 1,000,000 | 4 | 0.001584 | 0.003832 | 0.4134× | 10.33% |
| 10,000,000 | 4 | 0.006908 | 0.002608 | 2.6485× | 66.21% |

### Best Observed Result

For the dataset containing **10,000,000 elements**, the best recorded result was obtained using four OpenMP threads.

- Sequential execution time: 0.006908 seconds
- Parallel execution time: 0.002608 seconds
- Speedup: approximately 2.65×
- Parallel efficiency: 66.21%

These values are from the individual runs recorded during the experiment.

## 10. Observations

1. The calculated sum and average matched the expected values for all recorded configurations.
2. Parallel execution did not always outperform sequential execution for smaller datasets.
3. The largest dataset showed the strongest observed improvement with four threads.
4. Increasing the number of threads did not always reduce execution time.
5. Thread-management overhead, available CPU resources, and timing variability can affect performance.

## 11. Limitations and Future Improvements

The recorded results are based on individual measurements. Since the execution times are very short, they can vary between runs.

Possible improvements include:

- Repeating each configuration several times.
- Reporting mean or median execution times.
- Testing on systems with different CPU core counts.
- Comparing additional dataset sizes.
- Automating the collection of timing results.

## 12. Conclusion

This project demonstrates how OpenMP can parallelize sum calculation for datasets of different sizes. Sequential and parallel execution were compared using execution time, speedup, and efficiency.

The experiment showed that parallelization can improve performance for sufficiently large workloads, but adding more threads does not guarantee faster execution. The observed performance depends on workload size, available resources, and parallelization overhead.
