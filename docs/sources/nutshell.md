# Computational Catalysis in a Nutshell

It takes several steps to go from a structure to a publishable result.
This page contextualizes common steps and highlights the infrastructure required to execute those steps.

1. **Obtain a structure file.**

    Structure files can be:

    - generated within various Python packages (e.g., [ASE][ase], [Pymatgen][pymatgen])
    - obtained from the supplementary information of publications
    - retrieved from databases (e.g., [Materials Project][mp])

    Common structure file formats:

    - `.traj`: [ASE][ase] trajectory file
    - `.cif`: [Crystallographic Information File][cif]
    - `.xyz`: [XYZ File Format][xyz]
    - software-specific formats (e.g., `CONTCAR`, `Gaussian.log`, etc.)

2. **Perform structure manipulations.**

    Depending on the project, this can any combination of the following:

    - modifying the structure (e.g., creating defects, substituting atoms, creating surfaces)
    - adding adsorbates
    - and more...

3. **Determine necessary calculations and parameters.**

    This decision is informed by literature, experience, and the scientific questions that
    one wants to answer. The following papers provide a good starting point for making this
    decision.

     - [Best-Practice DFT Protocols for Basic Molecular Computational Chemistry][dft-best-practices]
     - [Density functional theory methods applied to homogeneous and heterogeneous catalysis: a short review and a practical user guide][dft-practical-guide]

4. **Create input files.**

    The required input files for a calculation generally include:

    - a structure file
    - an input parameter file for the DFT code
    - a Python script for interfacing with DFT code or generating results
    - a submission script
    - additional files for the DFT code (e.g., pseudopotentials, wavefunction files, etc.)

    Python and submission script templates for several different job types are described
    [here](./samples/python.md) and [here](./samples/slurm.md). The format for parameter
    files and additional files required by DFT codes are generally specified in the
    documentation for the corresponding computational code. However, for many DFT codes,
    there are utilities that can help with the the creation of input parameter files:

    - [ASE][ase]: provides Python interface to input file generation for a number of
      computational chemistry codes
    - [MaterialsCloud][materials-cloud]: provides a graphical interface for generating
      QuantumEspresso input files
    - [VSCode VASP INCAR Pilot Extension][vasp-support-ext]: VS Code extension providing
      help and support for writing VASP `INCAR`, `CONTCAR`, and `POSCAR` files

5. **Submit calculations.**

    Calculations are submitted to remote clusters managed by the Digital Research
    Alliance (formerly Compute Canada). This is typically done by logging into
    one of the clusters using `ssh` and executing something like

    ```bash
    sbatch run.sh
    ```

    from the command line.

6. **Retrieve Results.**

    Although the output files produced by the job contain the results of the calculation,
    these files are often quite large (~GB), so it is often useful to extract only
    the specific required outputs. These outputs may include:

    - the final positions of and forces on each atom in the final structure
    - the final energy for the system
    - orbital occupancies
    - trajectories for an optimization
    - vibrational modes

    While you can write a script to extract these data from the output files of your job,
    there is almost definitely a routine to extract this data in either [ASE][ase],
    [Pymatgen][pymatgen], or [cclib][cclib], so search the documentation of existing software
    for your use case.

7. **Analyze Results.**

    This almost inevitably involves importing data into Excel and performing further analyses
    there. For some data (e.g., density of states, valence charge density difference diagram,
    molecular orbital visualization), you may need another software. In such a case, check
    the tutorials for your use case.

[ase]: https://wiki.fysik.dtu.dk/ase/index.html
[pymatgen]: https://pymatgen.org
[mp]: https://next-gen.materialsproject.org
[cif]: https://en.wikipedia.org/wiki/Crystallographic_Information_File
[xyz]: https://en.wikipedia.org/wiki/XYZ_file_format
[vasp-support-ext]: https://marketplace.visualstudio.com/items?itemName=Mystery.vasp-support
[materials-cloud]: https://www.materialscloud.org/work/tools/qeinputgenerator
[dft-best-practices]: https://onlinelibrary.wiley.com/doi/abs/10.1002/anie.202205735
[dft-practical-guide]: https://pubs.rsc.org/en/content/articlelanding/2024/cp/d4cp00266k
[cclib]: https://cclib.github.io
