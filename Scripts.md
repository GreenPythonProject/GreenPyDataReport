# Energy and Time Measurement Scripts

## This section includes the scripts used to measure the following data analytics for Python native code, NumPy, Pandas, and Polars libary

The US 2015 Census data set is used for all data analytic tasks across all libraries for consistency. It is availble freely on keagle at (https://www.kaggle.com/datasets/muonneutrino/us-census-demographic-data) . It has a total of 74,002 rows and 37 columns with some missing data

1.    Get the average population from the US 2015 Census data set using NumPy
2.    Get the average population from the US 2015 Census data set using Pandas
3.    Get the average population from the US 2015 Census data set using Polars

Each script is wrapped around a process that runs Power Jourlar to measure the energy consumption.  Power Joular will only measure the energy of the PID attached to the script. Energy measurement data will be provided in a .csv file
Each script is run 10 times to miminise the risk of extrenous data skewing the mean overall power consumption measurement.
An appropriate amount of sleep will occur before the data analytic task begins to allow Power Joular to capture the energy consumption. The faster the execution of the data task, the longer the period of sleep needs to be.
A timer will start before the data analytic task begings and ends when the process stops.  Time data wlll be outputted to a csv file.


### NumPy Get Average

```
import time
import subprocess
import signal
import sys
import os
import numpy as np
from io import StringIO 

output = sys.argv[1]
dataset = sys.argv[2]
iteration = int(sys.argv[3])

powerjoular_filename = f"{output}_power_{iteration}.csv"
timing_filename =f"{output}_timing.csv"

pid = os.getpid()

powerjoular_process = subprocess.Popen(
    ["sudo", "powerjoular", "-f", powerjoular_filename, "-p", str(pid)],
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE,
    preexec_fn=lambda: signal.signal(signal.SIGINT, signal.SIG_IGN)
)

time.sleep(20)

try:
    start = time.time()

    data = np.genfromtxt(dataset, delimiter=',', skip_header=1)

    pop_column = data[:,3]

    avg = np.mean(pop_column)
    print(f"Average population = {avg:.2f}")

    end = time.time()
    duration = end - start
    print(f"Computation time = {duration:.2f} seconds")

    # Save timing result
    with open(timing_filename, "a") as f:
        f.write(f"{duration:.6f}\n")

finally:
    powerjoular_process.terminate()
    powerjoular_process.wait()
    time.sleep(20)

exit()

```

### Pandas Get Average

```
import time
import subprocess
import signal
import sys
import csv
import os
import pandas as pd


output = sys.argv[1]
dataset = sys.argv[2]
iteration = int(sys.argv[3])

powerjoular_filename = f"{output}_power_{iteration}.csv"
timing_filename =f"{output}_timing.csv"

pid = os.getpid()

powerjoular_process = subprocess.Popen(
    ["sudo", "powerjoular", "-f", powerjoular_filename, "-p", str(pid)],
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE,
    preexec_fn=lambda: signal.signal(signal.SIGINT, signal.SIG_IGN)
)

time.sleep(20)

try:
    start = time.time()

    data = pd.read_csv(dataset)
    avg = data["TotalPop"].mean()

    print(f"Average population = {avg:.2f}")

    end = time.time()
    duration = end - start
    print(f"Computation time = {duration:.2f} seconds")

    # Save timing result
    with open(timing_filename, "a") as f:
        f.write(f"{duration:.6f}\n")
    time.sleep(20)

finally:
    powerjoular_process.terminate()
    powerjoular_process.wait()


exit()
```


### Polars Get Average

```
import time
import subprocess
import signal
import sys
import csv
import os
import polars as pl


output = sys.argv[1]
dataset = sys.argv[2]
iteration = int(sys.argv[3])

powerjoular_filename = f"{output}_power_{iteration}.csv"
timing_filename =f"{output}_timing.csv"

pid = os.getpid()

powerjoular_process = subprocess.Popen(
    ["sudo", "powerjoular", "-f", powerjoular_filename, "-p", str(pid)],
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE,
    preexec_fn=lambda: signal.signal(signal.SIGINT, signal.SIG_IGN)
)

time.sleep(20)

try:
    start = time.time()

    data = pl.read_csv(dataset)
    avg = data["TotalPop"].mean()

    print(f"Average population = {avg:.2f}")

    end = time.time()
    duration = end - start
    print(f"Computation time = {duration:.2f} seconds")

    # Save timing result
    with open(timing_filename, "a") as f:
        f.write(f"{duration:.6f}\n")
    time.sleep(20)

finally:
    powerjoular_process.terminate()
    powerjoular_process.wait()


exit()

```

