# slurm-stats
**SLURM Stats** are scripts for gathering SLURM statistics

Currently the scripts are 
 - **sacct_stats.R** which generates simple user/monthly stats from sacct output
  - Requires also the helper.R script

## sacct_stats.R ##
### Fetching data
To generate the data, you should use the following type of **sacct** command. You probably want to vary the start (and end) dates (-S and -E flags).  

    sacct --format JobID,JobIDRaw,JobName,User,Group,Partition,MaxRSS,MaxPages,AveCPU,MaxDiskWrite,MaxDiskRead,MaxVMSize,NTasks,AllocCPUS,Submit,Start,Elapsed,End,State,ExitCode,ReqMem,Timelimit -s BF,CA,CD,F,NF,PR,TO -P -a -S 08/15 > sisu

The example contains some extra fields which are not processed yet by the script but will likely be useful

### Processing the data
 - Ensure that you have the [data.table](https://github.com/Rdatatable/data.table) library installed in R
```
install.packages("data.table")
library(data.table)
```
 - Run the command and give the input file as an argument
```
R --no-save --args "taito-gpu" < sacct_stats.R
```
 - After the script completes you should have CSVs containing aggregations of per-month and per-user data
```
sisu_stats_per_user.csv
sisu_stats_per_month.csv
```

 - There are also some commented out lines at the end that generate other plots and statisics and can be used as basis for playing around with the data interactively

### Interpreting the data

The resulting CSV files contain the following fields
 - **User** name or Date (Month/Year)
 - **Count** Number of jobs for the user or during the time period

For the following statistics, minimum, mean, maximum and standard deviation (min,mean,max,stddev) is calculated
 - **AllocCPUS** Allocated CPUs
 - **QueueTime** Time spent queued (in seconds)
 - **Elapsed** Time spent running (in seconds)
 - **Timelimitaccuracy** Difference of timelimit vs. actual runtime (Elapsed/Timelimit)
