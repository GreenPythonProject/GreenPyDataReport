# Energy and Time Measurement Scripts

## This section includes the scripts used to measure the following data analytics for NumPy, Pandas, and Polars libary. Scripts for GreenPy will be included at the end

### NumPy Get Average for small data set

```
import time
import subprocess
import signal
import os
import numpy as np


for x in range (1,100):

    powerjoular_filename = f"./numpy_avg_large_output/numpy_avg_small_power_{x}.csv"
    timing_filename =f"./timings/numpy_avg_small_timing.csv"

    pid = os.getpid()

    print("iteration ", x)	

    powerjoular_process = subprocess.Popen(
        ["sudo", "powerjoular", "-f", powerjoular_filename, "-p", str(pid)],
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE,
        preexec_fn=lambda: signal.signal(signal.SIGINT, signal.SIG_IGN)
    )

    time.sleep(20)

    try:
        start = time.time()

        data = np.genfromtxt("acs2015_census_tract_data.csv", delimiter=',', skip_header=1)

        pop_column = data[:,3]

        std = np.mean(pop_column)

        end = time.time()
        duration = end - start
        

        # Save timing result
        with open(timing_filename, "a") as f:
            f.write(f"{duration:.6f}\n")

        time.sleep(20)

    finally:
        powerjoular_process.terminate()
        powerjoular_process.wait()


exit()
```
### NumPy Get Average for large data set

```
import time
import subprocess
import signal
import os
import numpy as np


for x in range (1,100):

    powerjoular_filename = f"./numpy_avg_small_output/numpy_avg_large_power_{x}.csv"
    timing_filename =f"./timings/numpy_avg_large_timing.csv"

    pid = os.getpid()

    print("iteration ", x)	

    powerjoular_process = subprocess.Popen(
        ["sudo", "powerjoular", "-f", powerjoular_filename, "-p", str(pid)],
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE,
        preexec_fn=lambda: signal.signal(signal.SIGINT, signal.SIG_IGN)
    )

    time.sleep(20)

    try:
        start = time.time()

        data = np.genfromtxt("population_by_zip_2000.csv", delimiter=',', skip_header=1)

        pop_column = data[:,3]

        std = np.mean(pop_column)
        

        end = time.time()
        duration = end - start
        

        # Save timing result
        with open(timing_filename, "a") as f:
            f.write(f"{duration:.6f}\n")

        time.sleep(20)

    finally:
        powerjoular_process.terminate()
        powerjoular_process.wait()


exit()
```
### NumPy Get Standard Deviation Small dataset

```
import time
import subprocess
import signal
import os
import numpy as np


import pandas as pd

for x in range (1,100):

    powerjoular_filename = f"numpy_avg_small_power_{x}.csv"
    timing_filename =f"numpy_avg_small_timing.csv"

    pid = os.getpid()

    print("iteration ", x)	

    powerjoular_process = subprocess.Popen(
        ["sudo", "powerjoular", "-f", powerjoular_filename, "-p", str(pid)],
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE,
        preexec_fn=lambda: signal.signal(signal.SIGINT, signal.SIG_IGN)
    )

    time.sleep(20)

    try:
        start = time.time()

        data = pd.read_csv("acs2015_census_tract_data.csv")

        pop_column = data[:,3]

        std = np.std(pop_column)
        
        end = time.time()
        duration = end - start
        

        # Save timing result
        with open(timing_filename, "a") as f:
            f.write(f"{duration:.6f}\n")

        time.sleep(20)

    finally:
        powerjoular_process.terminate()
        powerjoular_process.wait()


exit()

```
### NumPy Get Standard Deviation Large dataset

```

import time
import subprocess
import signal
import os
import numpy as np


for x in range (1,100):

    powerjoular_filename = f"./numpy_std_small_output/numpy_std_small_output/numpy_std_small_power_{x}.csv"
    timing_filename =f"./timings/numpy_std_small_timing.csv"

    pid = os.getpid()

    print("iteration ", x)	

    powerjoular_process = subprocess.Popen(
        ["sudo", "powerjoular", "-f", powerjoular_filename, "-p", str(pid)],
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE,
        preexec_fn=lambda: signal.signal(signal.SIGINT, signal.SIG_IGN)
    )

    time.sleep(20)

    try:
        start = time.time()

        data = np.genfromtxt("acs2015_census_tract_data.csv", delimiter=',', skip_header=1)

        pop_column = data[:,3]

        std = np.std(pop_column)
        
        end = time.time()
        duration = end - start
        

        # Save timing result
        with open(timing_filename, "a") as f:
            f.write(f"{duration:.6f}\n")

        time.sleep(20)

    finally:
        powerjoular_process.terminate()
        powerjoular_process.wait()


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

