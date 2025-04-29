CPU SCHEDULING ALGORITHMS ANALYSIS
=================================

1. FIRST COME FIRST SERVE (FCFS)
--------------------------------
Theoretical Concepts:
- Simplest scheduling algorithm where processes are executed in order of arrival
- Non-preemptive: Once a process starts executing, it runs to completion
- Time Complexity: O(n log n) for sorting processes by arrival time
- Space Complexity: O(n) for storing processes

Implementation Details:
- Processes are sorted by arrival time
- Each process runs to completion without interruption
- Waiting time = Turnaround time - Burst time
- Turnaround time = End time - Arrival time
- Energy consumption = Burst time * 2 (base energy consumption)

2. ROUND ROBIN (RR)
------------------
Theoretical Concepts:
- Preemptive scheduling algorithm where each process gets a fixed time quantum
- Ensures fair CPU time distribution among processes
- Time Complexity: O(n * q) where q is number of time slices
- Space Complexity: O(n) for ready queue

Implementation Details:
- Uses a ready queue to maintain process order
- Each process gets a fixed quantum time slice
- If process doesn't complete within quantum, it goes back to queue
- If process completes within quantum, CPU is immediately given to next process
- Energy consumption = Burst time * 1.5 (slightly optimized energy usage)
- Handles idle time when no process is available

3. ENERGY-AWARE SCHEDULING
-------------------------
Theoretical Concepts:
- Advanced scheduling algorithm focusing on power efficiency
- Uses dynamic speed scaling based on process characteristics
- Time Complexity: O(n log n) for sorting and process selection
- Space Complexity: O(n) for process management

Implementation Details:
- Dynamic speed scaling based on process burst time:
  * Short processes (≤5 units): 70% speed
  * Medium processes (≤10 units): 85% speed
  * Long processes (>10 units): 100% speed
- Energy consumption follows P ∝ V² × f relationship
- Optimizes for both performance and power consumption

EXAMPLE CALCULATION
------------------
Given Processes:
P1: arrival_time=1, burst_time=5
P2: arrival_time=2, burst_time=6
P3: arrival_time=3, burst_time=11

1. FCFS Analysis:
   Timeline:
   0-1: Idle
   1-6: P1
   6-12: P2
   12-23: P3
   
   Metrics:
   P1: Turnaround=5, Waiting=0 (5-5)
   P2: Turnaround=10, Waiting=4 (10-6)
   P3: Turnaround=20, Waiting=9 (20-11)
   Average Waiting Time: 4.33
   Average Turnaround Time: 11.67

2. Round Robin Analysis (quantum=2):
   Timeline:
   1-3: P1(2)
   3-5: P2(2)
   5-7: P3(2)
   7-9: P1(2)
   9-11: P2(2)
   11-13: P3(2)
   13-14: P1(1)  // P1 completes in 1 second, CPU immediately given to P2
   14-16: P2(2)  // P2 completes at 16 seconds, CPU immediately given to P3
   16-23: P3(7)  // P3 executes for remaining time
   
   Metrics:
   P1: Turnaround=13, Waiting=8 (13-5)
   P2: Turnaround=14, Waiting=8 (14-6)
   P3: Turnaround=20, Waiting=9 (20-11)
   Average Waiting Time: 8.33
   Average Turnaround Time: 15.67

3. Energy-Aware Analysis:
   Timeline:
   1-8: P1 (70% speed)
   8-15: P2 (85% speed)
   15-26: P3 (100% speed)
   
   Metrics:
   P1: Turnaround=8, Waiting=0 (8-8)
   P2: Turnaround=13, Waiting=7 (15-8)
   P3: Turnaround=23, Waiting=14 (25-11)
   Average Waiting Time: 7.0
   Average Turnaround Time: 16


-------------
1. Q: What makes the Energy-Aware algorithm different from traditional scheduling?
   A: The Energy-Aware algorithm implements dynamic speed scaling based on process characteristics, 
      reducing power consumption while maintaining performance. It uses the relationship P ∝ V² × f 
      to optimize energy usage.

2. Q: How does the Energy-Aware algorithm handle different process types?
   A: It categorizes processes based on burst time and adjusts CPU speed accordingly:
      - Short processes run at 70% speed to save energy
      - Medium processes run at 85% speed for balance
      - Long processes run at full speed to maintain performance

3. Q: What are the advantages of the Energy-Aware approach over traditional algorithms?
   A: 
   - Reduced power consumption through dynamic speed scaling
   - Better resource utilization
   - Balanced performance and energy efficiency
   - Adaptive to different process characteristics

4. Q: How does the algorithm handle process priorities?
   A: The current implementation considers process length as the primary factor for speed scaling.
      Priority could be incorporated by adjusting the speed factor based on both burst time and priority.

5. Q: What improvements could be made to the Energy-Aware algorithm?
   A:
   - Add priority-based scheduling
   - Implement more sophisticated speed scaling
   - Consider system load in speed decisions
   - Add temperature-aware scheduling
   - Implement process grouping for better resource utilization 

## Energy-Aware Scheduling

### Theoretical Concepts
Energy-Aware scheduling is a power-efficient CPU scheduling algorithm that reduces energy consumption by dynamically adjusting CPU speed based on process characteristics. It categorizes processes into short, medium, and long based on their burst time and assigns different speed factors to each category.

### Implementation Details
- Short processes (burst time ≤ 5): Run at 70% speed
- Medium processes (5 < burst time ≤ 10): Run at 85% speed
- Long processes (burst time > 10): Run at 100% speed
- Execution time = Math.ceil(burst time / speed factor)
- Energy consumption = execution  time × (speed factor)²

### Example Calculations
Consider three processes with the following characteristics:
- P1: Arrival Time = 1, Burst Time = 5
- P2: Arrival Time = 2, Burst Time = 6
- P3: Arrival Time = 3, Burst Time = 11

#### Process Categorization and Speed Scaling
- P1: Short process, Speed Factor = 0.7 (70%)
- P2: Medium process, Speed Factor = 0.85 (85%)
- P3: Long process, Speed Factor = 1.0 (100%)

#### Execution Time Calculation
- P1: Execution Time = Math.ceil(5 / 0.7) = Math.ceil(7.14) = 8 time units
- P2: Execution Time = Math.ceil(6 / 0.85) = Math.ceil(7.06) = 8 time units
- P3: Execution Time = Math.ceil(11 / 1.0) = Math.ceil(11) = 11 time units

#### Timeline Construction
```
Time:    0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26
Process: -  P1 P1 P1 P1 P1 P1 P1 P1 P2 P2 P2 P2 P2 P2 P2 P2 P3 P3 P3 P3 P3 P3 P3 P3 P3 P3 P3 P3 P3
```

- P1: Arrives at time 1, runs from time 1-9 (8 units)
- P2: Arrives at time 2, runs from time 9-17 (8 units)
- P3: Arrives at time 3, runs from time 17-28 (11 units)

#### Turnaround Time Calculation
- P1: 9 - 1 = 8 time units
- P2: 17 - 2 = 15 time units
- P3: 28 - 3 = 25 time units
- Average Turnaround Time: (8 + 15 + 25) / 3 = 16.0 time units

#### Waiting Time Calculation
- P1: 8 - 8 = 0 time units
- P2: 15 - 8 = 7 time units
- P3: 25 - 11 = 14 time units
- Average Waiting Time: (0 + 7 + 14) / 3 = 7.0 time units

#### Energy Consumption Calculation
- P1: 5 × (0.7)² = 5 × 0.49 = 2.45 energy units
- P2: 6 × (0.85)² = 6 × 0.7225 = 4.34 energy units
- P3: 11 × (1.0)² = 11 × 1.0 = 11.0 energy units
- Total Energy Used: 2.45 + 4.34 + 11.0 = 17.79 energy units

### Comparison with Other Algorithms
| Algorithm      | Avg Waiting Time | Avg Turnaround Time | Total Energy |
|----------------|------------------|---------------------|--------------|
| FCFS           | 4.33             | 11.67               | 44.0         |
| Round Robin    | 8.33             | 15.67               | 33.0         |
| Energy-Aware   | 7.0              | 16.0                | 17.79        |


1. How does the Energy-Aware algorithm reduce power consumption?
   - By categorizing processes and running them at reduced speeds
   - Short processes run at 70% speed, medium at 85%, and long at 100%
   - Energy consumption is proportional to the square of the speed factor

2. What is the trade-off in the Energy-Aware algorithm?
   - Energy savings vs. increased execution time
   - Running at reduced speed increases execution time but reduces energy consumption
   - The algorithm balances these factors based on process characteristics

3. How is execution time calculated in the Energy-Aware algorithm?
   - Execution Time = Math.ceil(Burst Time / Speed Factor)
   - Math.ceil ensures complete execution of processes
   - The execution time becomes the modified burst time for scheduling

4. How is energy consumption calculated in the Energy-Aware algorithm?
   - Energy = Burst Time × (Speed Factor)²
   - Uses the original burst time, not the execution time
   - Reflects the theoretical energy consumption if the process ran at full speed

5. How does the Energy-Aware algorithm compare to FCFS and Round Robin in terms of energy efficiency?
   - Energy-Aware: 17.79 energy units
   - FCFS: 44.0 energy units
   - Round Robin: 33.0 energy units
   - Energy-Aware achieves significant energy savings (59.6% compared to FCFS) 
