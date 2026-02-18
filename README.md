# PYXAID MPI Liouville-Space Extension

This repository extends PYXAID with Liouville-space surface-hopping dynamics.

## Layout
- `pyxaid-code-mpi/` – main source tree.
  - `PYXAID/` – Python API wrapping the compiled C++ core `pyxaid_core.so` plus utilities such as `runMD.py`, `average.py`, and spectrum/PDOS helpers.
  - `code_level3/` – helper scripts (plotting, submission template `submit_templ.pbs`).
  - `tutorials/` – runnable tutorials (`Tut1_basics`, `Tut2_small`, `Tut3_code_level2_NAC`, `Tut4_coding_with_Pyxaid`).
  - `test.small` & `test.large` – example MPI runs containing `para-pyxaid.py` drivers.
  - `README.mpi4py` – developer notes describing the MPI changes (splitting iconds across ranks, tar/gzip handling of `res` and spectral density outputs).
  - `how_to_use.txt` – example environment setup from the original cluster (Python 2.7, Boost 1.68 library path, PYTHONPATH entries).

## Requirements
- Python 2.7 with `mpi4py` installed.
- MPI runtime (e.g., OpenMPI/MPICH) and standard `tar`/`gzip` utilities.
- The bundled `pyxaid_core.so` is prebuilt; rebuilding the C++ core requires Boost (see the library path hint in `how_to_use.txt`).

## Quick Start (running the small test)
1) Add the code to `PYTHONPATH`:
   ```bash
   export PYTHONPATH="$PYTHONPATH:$(pwd)/pyxaid-code-mpi"
   ```
2) Place or link your `res.tar.gz` Hamiltonian archive where the driver expects it, or edit `ham_dir` in `pyxaid-code-mpi/test.small/para-pyxaid.py` to point to your path.
3) Run with MPI (adjust ranks as needed):
   ```bash
   mpirun -np 4 python pyxaid-code-mpi/test.small/para-pyxaid.py
   ```

The driver untars `res.tar.gz` to a scratch directory on each node, distributes initial conditions across ranks, and aggregates spectra at the end. For larger calculations, start from `test.large/para-pyxaid.py` and tune the parameters inside the script.

## More Info
- For MPI implementation details and file-management behavior, read `pyxaid-code-mpi/README.mpi4py`.
- Cluster module examples and library paths live in `pyxaid-code-mpi/how_to_use.txt`.
- Tutorials in `pyxaid-code-mpi/tutorials/` provide progressively more complex input setups.
