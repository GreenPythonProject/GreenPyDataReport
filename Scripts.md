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


### Pandas Get Average Small Dataset

```
import time
import subprocess
import signal
import os
import pandas as pd


for x in range (1,100):

    powerjoular_filename = f"./pandas_avg_small_output/numpy_avg_small_power_{x}.csv"
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

        data = pd.read_csv("acs2015_census_tract_data.csv")

        avg = data["TotalPop"].mean()

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


### Pandas Get Average Large Dataset

```
import time
import subprocess
import signal
import os
import pandas as pd


for x in range (1,100):

    powerjoular_filename = f"./pandas_avg_large_output/pandas_avg_large_power_{x}.csv"
    timing_filename =f"./timings/pandas_avg_large_timing.csv"

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

        data = pd.read_csv("population_by_zip_2000.csv")

        avg = data["population"].mean()

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

### Pandas Get Standard Deviation Small Dataset

```

import time
import subprocess
import signal
import os
import pandas as pd


for x in range (1,100):

    powerjoular_filename = f"./pandas_std_small_output/pandas_std_small_power_{x}.csv"
    timing_filename =f"./timings/pandas_std_small_timing.csv"

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

        avg = data["TotalPop"].std()

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

### Pandas Get Standard Deviation Large Dataset

```
import time
import subprocess
import signal
import os
import pandas as pd


for x in range (1,100):

    powerjoular_filename = f"./pandas_std_large_output/pandas_std_large_power_{x}.csv"
    timing_filename =f"./timings/pandas_std_large_timing.csv"

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

        data = pd.read_csv("population_by_zip_2000.csv")

        avg = data["population"].std()

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

### Polars Get Average Small Dataset

```
import time
import subprocess
import signal
import os
import polars as pl


for x in range (1,100):

    powerjoular_filename = f"./polars_avg_small_output/polars_avg_small_power_{x}.csv"
    timing_filename =f"./timings/polars_avg_small_timing.csv"

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

        data = pl.read_csv("acs2015_census_tract_data.csv")

        avg = data["TotalPop"].mean()

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

### Polars Get Average Large Dataset

```
import time
import subprocess
import signal
import os
import polars as pl


for x in range (1,100):

    powerjoular_filename = f"./polars_avg_large_output/polars_avg_large_power_{x}.csv"
    timing_filename =f"./timings/polars_avg_large_timing.csv"

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

        data = pl.read_csv("population_by_zip_2000.csv")

        avg = data["population"].mean()

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

### Polars Get Standard Deviation Small Dataset

```
import time
import subprocess
import signal
import os
import polars as pl


for x in range (1,100):

    powerjoular_filename = f"./polars_std_small_output/polars_std_small_power_{x}.csv"
    timing_filename =f"./timings/polars_std_small_timing.csv"

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

        data = pl.read_csv("acs2015_census_tract_data.csv")

        avg = data["TotalPop"].std()

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

### Polars Get Standard Deviation Large Dataset

```
import time
import subprocess
import signal
import os
import polars as pl


for x in range (1,100):

    powerjoular_filename = f"./polars_std_large_output/polars_std_large_power_{x}.csv"
    timing_filename =f"./timings/polars_std_large_timing.csv"

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

        data = pl.read_csv("population_by_zip_2000.csv")

        avg = data["population"].std()

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


### GreenPy Get Average Small Dataset

```
import time
import subprocess
import signal
import os

import greenpydata
import polars as pl

for x in range (1,100):

    powerjoular_filename = f"./greenpy_avg_small_output/greenpy_avg_small_power_{x}.csv"
    timing_filename =f"./timings/greenpy_avg_small_timing.csv"

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

        data = pl.read_csv("acs2015_census_tract_data.csv")

        avg = greenpydata.aggregation("get_avg", data, "TotalPop")

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

### GreenPy Get Average Large Dataset

```
import time
import subprocess
import signal
import os

import greenpydata
import polars as pl


for x in range (1,100):

    powerjoular_filename = f"./greenpy_avg_large_output/greenpy_avg_large_power_{x}.csv"
    timing_filename =f"./timings/greenpy_avg_large_timing.csv"

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

        data = pl.read_csv("population_by_zip_2000.csv")

        avg = greenpydata.aggregation("get_avg", data, "population")

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


### GreenPy Get Standard Deviation Small Dataset

```
import time
import subprocess
import signal
import os

import greenpydata
import polars as pl
import greenpydata as gp
from io import StringIO 

for x in range (1,100):

    powerjoular_filename = f"./greenpy_std_small_output/greenpy_std_small_power_{x}.csv"
    timing_filename =f"./timings/greenpy_std_small_timing.csv"

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

        data = pl.read_csv("acs2015_census_tract_data.csv")

        avg = greenpydata.aggregation("get_std", data, "TotalPop")

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

### GreenPy Get Standard Deviation Large Dataset

```
import time
import subprocess
import signal
import os

import greenpydata
import polars as pl


for x in range (1,100):

    powerjoular_filename = f"./greenpy_std_large_output/greenpy_std_large_power_{x}.csv"
    timing_filename =f"./timings/greenpy_std_large_timing.csv"

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

        data = pl.read_csv("population_by_zip_2000.csv")

        avg = greenpydata.aggregation("get_std", data, "population")

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

