# Python package with Rust

Python package with some functions in Rust using [PyO3](https://pyo3.rs/) and [Maturin](https://www.maturin.rs/).
Maturin was made for all Rust python packages.
How mixed packages work is mentioned [in the README](https://github.com/PyO3/maturin/tree/main#mixed-rustpython-projects).
Some fiddling has to be done with the project structure, [Cargo.toml](./Cargo.toml), [pyproject.toml](./pyproject.toml)
to get everything right.
It should be possible to:
1. test the package locally (after building the Rust part)
2. buid the package with both Rust and Python

## Development Setup

Install all dependencies including [Maturin](https://www.maturin.rs/).

```
# using conda environments here
conda env create -f environment.yml
conda activate lutz
```

There is a python package [python/lutz/](./python/lutz/)
and a Rust library [rust/lib.rs](./rust/lib.rs).
In [lib.rs](./rust/lib.rs) a python module is implemented which is referenced in
[Cargo.toml](./Cargo.toml) in `lib.name`.
[Cargo.toml](./Cargo.toml) also defines package name and version number.

```
# build rust library
maturin develop
```

The library is build and a resulting `_lib.*.so` file is placed in [python/lutz](./python/lutz/).
This is defined in [pyproject.toml](./pyproject.toml) under `[tool.maturin]`.
In [python/lutz/rust.py](./python/lutz/rust.py) the private `_lib` is used.

Note [.vscode/settings.json](./.vscode/settings.json) and [vscode.env](./vscode.env)
is setup for being able to import *lutz* from the integrated python terminal
and Jupyter notebooks, as well as tell pylint how to import *lutz*.

## Tests

Using [pytest](https://docs.pytest.org/) python test suite.
`_lib.*.so` in *python/lutz/* must have been built before
(`maturin develop`).

```
PYTHONPATH=./python pytest tests
```

## Build

Maturin builds the project as defined in [pyproject.toml](./pyproject.toml)
into a `.whl`.

```
# release for optimizations
maturin build  --release

# not tested
# kann man fürs publishen nehmen
maturin publish
```