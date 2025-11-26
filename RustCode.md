~~
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
~~
