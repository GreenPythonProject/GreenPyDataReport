# Energy and Time Measurement Scripts

## This section includes the scripts used to measure the following data analytics for NumPy, Pandas, and Polars libary

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

