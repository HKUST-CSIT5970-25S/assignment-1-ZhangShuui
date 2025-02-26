[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Shurui Zhang
### Student Id: 21073000
### Email: szhangfa@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

**Tool:** Phoronix Test Suite (v10.8.4)

**CPU Test:**  
- **Test:** `pts/compress-gzip` (3 runs)  
- **Why:** Gzip compression mimics real-world CPU workloads.  
- **Result:** Execution time in seconds (lower is better).

**Memory Test:**  
- **Test:** `pts/ramspeed` with `{run-type: Average, benchmark: Integer}` (3 runs)  
- **Why:** Chosen for its popularity and relevance in measuring average memory performance using integer operations.  
- **Result:** Memory speed in MB/s (higher is better).

---

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance (Seconds) | Memory performance (MB/s) |
    | ----------- | ------------------------- | ------------------------- |
    | `t2.micro`  | 130.119                   | 10618.79                  |
    | `t2.medium` | 51.701                    | 19127.09                  |
    | `c5d.large` | 44.565                    | 14977.72                  |


Based on the results above:
- CPU Performance: Increasing vCPUs leads to lower execution times—t2.micro is the slowest, t2.medium shows a marked improvement, and c5d.large performs the best.
- Memory Performance: Although t2.medium has the highest memory speed, c5d.large (despite having more vCPUs) does not scale its memory throughput proportionately.
**Conclusion:** While CPU performance improves with additional vCPUs, memory performance does not increase linearly with memory resources; other architectural factors also play a significant role.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

| Type                      | TCP b/w (Mbps) | RTT (ms) |
| ------------------------- | -------------- | -------- |
| `t3.medium` - `t3.medium` | 3754           | 0.182    |
| `m5.large` - `m5.large`   | 5087           | 0.143    |
| `c5n.large` - `c5n.large` | 9371           | 0.112    |
| `t3.medium` - `c5n.large` | 2387           | 0.674    |
| `m5.large` - `c5n.large`  | 3192           | 0.587    |
| `m5.large` - `t3.medium`  | 1874           | 1.047    |

Based on the results above:
Network performance between instances of the same type is notably better than between different types. For homogeneous pairs (e.g., `t3.medium`–`t3.medium`, `m5.large`–`m5.large`, `c5n.large`–`c5n.large`), TCP bandwidth is higher and RTT is lower. In contrast, mixed pairs such as `t3.medium`–`c5n.large` and `m5.large`–`t3.medium` experience reduced bandwidth and increased latency, reflecting differences in underlying hardware and network configurations between instance types.


2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

| Connection                  | TCP b/w (Mbps) | RTT (ms) |
| --------------------------- | -------------- | -------- |
| N. Virginia - Oregon        | 45.3           | 79.1     |
| N. Virginia - N. Virginia   | 4672           | 0.178    |
| Oregon - Oregon             | 4825           | 0.171    |

Based on the results above:
Instances within the same region (N. Virginia–N. Virginia and Oregon–Oregon) exhibit extremely high TCP bandwidth (over 4600 Mbps) and very low latency (around 0.17–0.18 ms). In contrast, instances deployed in different regions (N. Virginia–Oregon) experience a dramatic drop in bandwidth (only 45.3 Mbps) and a significant increase in RTT (79.1 ms). This clearly shows that cross-region communications are limited by long-distance network delays and lower throughput compared to same-region connections.

