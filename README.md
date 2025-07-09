# High Performance Computing (HPC) Benchmarking Project

This project benchmarks the performance, scalability, and energy efficiency of parallel applications on a high-performance computing cluster. By combining MPI, OpenMP, and GPU workloads (TensorFlow), the project explores the impact of different parallelization strategies on execution time, resource usage, and energy consumption.

---

## 📄 Overview

We evaluate three NAS Parallel Benchmarks (NPB) multi-zone (MZ) kernels:

- **BT-MZ** (Block-Tridiagonal)
- **LU-MZ** (Lower-Upper symmetric Gauss-Seidel)
- **SP-MZ** (Scalar Pentadiagonal)

Each kernel is executed under four parallel paradigms:

- **MPI only:** Multiple MPI processes per node, one thread per process.
- **OpenMP only:** Single MPI process per node, multiple threads per process.
- **Hybrid configurations:**
  - **INT1:** Many MPI processes per node, 8 threads per process.
  - **INT2:** Fewer MPI processes per node, 16 threads per process.

Experiments span **4, 6, and 8 nodes** to measure:

- Execution time
- Scalability (speedup and parallel efficiency)
- CPI (Cycles Per Instruction)
- Memory bandwidth
- Power and energy consumption

---

## ⚙️ Cluster and Environment

- **Partitions:** 
  - GPP (CPU nodes)
  - ACC (GPU nodes)
- **Nodes:** Up to 8 nodes (totaling 896 CPU cores)
- **Monitoring Tools:** 
  - EAR (Energy Aware Runtime) for power and energy tracking
  - Custom Python utilities for CSV merging and metric computation
- **GPU Workloads:** TensorFlow models (DenseNet121, ResNet50, VGG19), tested with different precision settings (basic and mixed)

---

## 🧪 Experimental Highlights

### Scalability & Performance

- **Hybrid (INT1)** configurations consistently achieved the best trade-off between runtime and efficiency.
- **MPI-only** runs showed strong CPU scaling (lower CPI), but memory bandwidth decreased as node count grew.
- **OpenMP-only** configurations scaled poorly, with the highest energy consumption due to thread synchronization overhead.

#### Example: BT-MZ Kernel on 8 Nodes

| Configuration | Elapsed Time (s) | Speedup | Efficiency (%) |
|---------------|------------------|---------|----------------|
| MPI           | 39.94            | 1.00    | 12.50          |
| OMP           | 26.19            | 1.53    | 19.12          |
| INT1          | 12.29            | 3.26    | 40.74          |
| INT2          | 14.91            | 2.69    | 33.60          |

### Energy Efficiency

- **INT1** consistently delivered the lowest energy consumption and highest efficiency.
- OpenMP-only was the least energy efficient in all scenarios.
- **GPU experiments:** Mixed precision in TensorFlow models significantly reduced energy consumption compared to standard precision.

---

## 🔍 Metrics Tracked

- **Execution Time & Speedup**
- **Parallel Efficiency**
- **CPI (Cycles Per Instruction):** Lower in MPI-dominant settings.
- **Memory Bandwidth:** Higher in MPI, but drops at scale.
- **Average Power Draw**
- **Energy Cost Estimation**

---

## 📝 How to Run

This repository provides:

- SLURM scripts for launching benchmarks (`run_bt_4nodes.sh`, etc.)
- Python utilities for merging EAR CSV logs and computing derived metrics
- TensorFlow scripts for GPU benchmarking (with precision toggling)

**Example workflow:**

1. **Launch a kernel benchmark:**
   ```bash
   sbatch run_bt_4nodes.sh
   ```
2. **Collect EAR energy metrics:**
   ```bash
   scp mn5:/path/to/results/*.csv .
   ```
3. **Merge and analyze metrics:**
   ```bash
   python merge_csv.py
   python calculate_energy.py
   ```
4. **Generate plots and summary tables:**
   ```bash
   python generate_plots.py
   ```

---

## 📂 Repository Structure

```
/scripts             → SLURM scripts to launch and manage jobs
/Proyecto HPC        → Project report, plots, and summary tables
```

---

## 📚 References

- [NAS Parallel Benchmarks (NPB)](https://www.nas.nasa.gov/software/npb.html)
- [EAR: Energy Aware Runtime](https://www.bsc.es/research-and-development/software-and-apps/software-list/ear-energy-aware-runtime)
- [TensorFlow Documentation](https://www.tensorflow.org/)

---

## 📝 License

This project is licensed under the MIT License.
