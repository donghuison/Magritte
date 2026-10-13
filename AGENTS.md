# Repository Guidelines

Magritte is a radiation transport library with a C++17 core and a Python frontend.

## Project Structure & Module Organization

- `src/model/`, `src/solver/`, and `src/io/` implement physical models, solvers, and I/O; `src/bindings/pybindings.cpp` exposes the core to Python.
- `magritte/` contains Python setup, meshing, plotting, and utility modules; `magritte/templates/` holds mesh templates.
- `tests/benchmarks/analytic/` and `tests/benchmarks/numeric/` contain Python benchmarks. Fixtures live in `tests/data/`; generated models go in `tests/models/`.
- `docs/src/` contains Sphinx documentation, example notebooks, and image assets. `dependencies/` holds submodules and environment setup; `.github/workflows/` and `CI/` define automation.

## Build, Test, and Development Commands

Run from the repository root unless noted. Use CMake 3.18+ and a C++17 compiler with OpenMP support.

- `git submodule update --init --recursive`: initialize Eigen, pybind11, Paracabs, and GoogleTest.
- `conda env create -n magritte -f dependencies/conda_env.yml`, then `conda activate magritte`: prepare the development and benchmark dependencies.
- `python -m pip install .`: compile and install the Python package, including `magritte.core`. Repeat after C++ changes.
- `bash compile.sh`: rebuild into `build/` and `bin/` with OpenMP enabled and MPI/GPU disabled. Override compilers with `CC` and `CXX`.
- `jupyter lab`: run examples under `docs/src/1_examples/`.

## Coding Style & Naming Conventions

Use four spaces for C++ and Python indentation. Follow `.clang-format` for C++: attached braces, no tabs, and a 100-column limit. Format changed files with `clang-format -i src/path/file.cpp`. CI formats `src/`; Python formatting checks are currently disabled. Match nearby naming: `Model`/`Geometry` types, descriptive underscore-separated functions, and `.hpp`/`.cpp`/`.tpp` files. Keep binding changes consistent with the Python API.

## Testing Guidelines

Run the same pytest suites as CI:

```sh
(cd tests/benchmarks/analytic && python -m pytest --verbose)
(cd tests/benchmarks/numeric && python -m pytest --verbose)
```

Use `test_*.py` files and `test_*` methods. Preserve setup-before-run ordering in incremental classes. Add numerical regression checks for solver changes; some benchmarks download external data. Legacy C++ tests and GoogleTest wiring exist, but root CMake disables that directory. No coverage threshold is configured.

## Commit & Pull Request Guidelines

History favors descriptive subjects such as `Fix code style issues with clang_format`. CI defaults to patch version increments; `(increase version minor)` or `(increase version major)` requests larger increments.

Follow `CONTRIBUTING.md`: fork, create a branch, and run relevant benchmarks/examples before submitting. Discuss new functionality in an issue first. Describe behavior changes, link relevant issues, and report validation results. Contribute examples as notebooks; keep generated binaries, HDF5 models, and built documentation out of commits.
