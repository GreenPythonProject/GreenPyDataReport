### Iteration of Rust code to read CSV into memory

# 1.    Use of native Rust code without the use of third party CSV reader libaries

```
use pyo3::prelude::*;
use std::fs::File;
use std::io::{self, BufRead, BufReader};
use std::path::Path;

// Data structure of dataframe attributes and expose to Python
#[pyclass]
pub struct DataFrameAttributes {
    #[pyo3(get)]
    number_of_rows: usize,
    #[pyo3(get)]
    number_of_cols: usize,
    #[pyo3(get)]
    has_header: bool,
    #[pyo3(get)]
    datum: Vec<Vec<String>>,
}

#[pyfunction]
pub fn read_file(filename: &str, header_status: bool) -> PyResult<DataFrameAttributes> {
    // check for file error
    if std::fs::metadata(filename).is_err() {
        return Err(PyErr::new::<pyo3::exceptions::PyFileNotFoundError, _>(
            format!("File '{}' does not exist", filename),
        ));
    }

    let mut datum: Vec<Vec<String>> = Vec::new();

    if let Ok(lines) = read_lines(filename) {
        for (_i, line) in lines.flatten().enumerate() {
            let fields: Vec<String> = line.split(',').map(|s| s.trim().to_string()).collect();
            datum.push(fields);
        }
    }

    let number_of_rows = datum.len();
    let number_of_cols = if number_of_rows > 0 { datum[0].len() } else { 0 };


    Ok(DataFrameAttributes {
        number_of_rows,
        number_of_cols,
        has_header: header_status,
        datum,
    })
}

fn read_lines<P>(filename: P) -> io::Result<io::Lines<BufReader<File>>>
where
    P: AsRef<Path>,
{
    let file = File::open(filename)?;
    Ok(BufReader::new(file).lines())
}
```
#    2.    Use of the CSV Crate https://crates.io/crates/csv


```

use pyo3::prelude::*;
use once_cell::sync::Lazy;
use std::sync::Mutex;
use std::path::Path;
use csv::{ReaderBuilder, ByteRecord};

// Global storage for loaded CSV rows
pub static FRAME: Lazy<Mutex<Vec<Vec<String>>>> = Lazy::new(|| Mutex::new(Vec::new()));

#[pyfunction]
pub fn readfile(filename: &str) -> PyResult<usize> {
    if !Path::new(filename).exists() {
        return Err(PyErr::new::<pyo3::exceptions::PyFileNotFoundError, _>(
            format!("File '{}' does not exist", filename),
        ));
    }

    let mut rdr = ReaderBuilder::new()
        .has_headers(true)
        .from_path(filename)
        .map_err(|e| PyErr::new::<pyo3::exceptions::PyIOError, _>(format!("{}", e)))?;

    let mut record = ByteRecord::new();
    let mut rows: Vec<Vec<String>> = Vec::new();

    // Read each ByteRecord
    while rdr.read_byte_record(&mut record)
        .map_err(|e| PyErr::new::<pyo3::exceptions::PyIOError, _>(format!("{}", e)))?
    {
        // Deserialize dynamically into Vec<String>
        let row: Vec<String> = record
            .deserialize(None)
            .map_err(|e| PyErr::new::<pyo3::exceptions::PyValueError, _>(format!("{}", e)))?;

        rows.push(row);
    }

    let row_count = rows.len();

    *FRAME.lock().unwrap() = rows;

    Ok(row_count)
}

```
#    3.    Use of Rust Polars library to read CSV file into a data frame

```

use pyo3::prelude::*;
use pyo3::wrap_pyfunction;
use polars::prelude::*;
use std::fs::File;
use std::io::BufReader;


#[pyfunction]
pub fn readfile(path: &str) -> PyResult<String> {
    // Open file
    let file = File::open(path)
        .map_err(|e| PyErr::new::<pyo3::exceptions::PyIOError, _>(e.to_string()))?;
    let reader = BufReader::new(file);

    // Read CSV into a DataFrame
    let df = CsvReader::new(reader)
        .finish()
        .map_err(|e| PyErr::new::<pyo3::exceptions::PyRuntimeError, _>(e.to_string()))?;

    Ok(format!("{:?}", df))
}

```
