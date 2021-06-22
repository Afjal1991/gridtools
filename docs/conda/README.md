# conda

This explains some things about how to utilize conda to
manage python environments.  To learn much more details about
conda, we suggest visiting the 
[conda documentation](https://docs.conda.io/projects/conda/en/latest/index.html) website.

A YAML specification file is to configure a python environment.  We have prepared
a few specification files, please see the conda directory.

These instructions assume you have conda/miniconda installed.  If conda is not installed,
please consult this
[webpage](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).

If **conda cannot be used**, please consult the
[required software](../development/Requirements.md) for packages to install.

Several basic YAML files are provided to install an appropriate python environment.
To expidite the conda environment solver, a YAML\_export or YAML\_explicit file is
also provided for quicker recovery of python environment.

Initialization:
```
$ cat conda/gridTools.yml
$ conda env create -f=conda/gridTools.yml
$ conda env export > conda/gridTools_export.yml
$ conda list --explicit > conda/gridTools_explicit.txt
```

NOTE: The exported yml file will specify a directory path for installation.  It is
recommended to delete the last line in the yml file before publication.

NOTE: The explicit dumps will contain a specific OS platform target (e.g. linux-64) and
may not be compatible with the intended target platform for installation of the library.

For a quicker recovery of a conda environment, use the exported YAML file:
```
$ conda env remove --name gridTools
$ conda env create -f=conda/gridTools_export.yaml
```

NOTE: Read any post-installation comments found in the yml files.  Sometimes
multiple steps are required to help conda resolve dependencies.

Example:
```
$ conda env create -f conda/gridTools.yml
$ conda activate gridTools
(gridTools) $ conda install -c conda-forge geopandas matplotlib ipympl cartopy netcdf4 conda
(gridTools) $ conda env export -n gridTools > conda/gridTools_export.yml
```

You can also restore a conda environment using the explicit dump of packages.
```
$ conda create --name gridTools --file conda/gridTools_explicit.txt
```

## Performance

Sometimes the conda resolution can take a very long time to run.
Setting an automatic timeout is recommended if building a custom
conda enviroment.  This can be done using the `timeout` feature
of UNIX `time` command.

In this example, the timeout is set to 5 minutes to allow resolution
of the environment.
```
$ time timeout 5m conda env create -f conda/gridTools.yml
```

# Environments

Current operational environment for the grid toolset: ***gridTools***

## gridTools

This is the main enviroment for utilizing the grid generation libraries.

NOTE: Avoid version 0.5.2 of xesmf.  If you need to use the xgcm library,
      install it as a separate environment.
## legacyTools

NOTE: This is a very limited environment with netcdf4 and matplotlib's basemap
to review former functionality of older libraries and software.

This environment supported investigation into the following libraries:
  * [leaflet](../development/python/libraries/leaflet.md)
  * [pyroms](../development/python/libraries/pyroms.md)
