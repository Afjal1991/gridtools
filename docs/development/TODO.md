# Planned work

## Milestones

 - [ ] Release 0.3
   - [ ] Port ROMS mask editor as-is to gridtools under ipython/pylab
         so it works with MOM6/ROMS grids
 - [ ] Release 0.x
   - [ ] Application improvements
   - [ ] Improve mask editor grid location on click
   - [ ] Enable mask editor to work under Jupyter
   - [ ] Boundery condition grid creation and support (OBCs)
     - [ ] Save only the points we need instead of the whole supergrid
   - [ ] Sponge data preparation
   - [ ] Subset existing grids and infrastructure
   - [ ] Leverage dask (for users that lack access to large memory nodes)
   - [ ] Explore the extent problem for lon defined as +0,+360 vs -180,+180
   - [ ] Enhanced grid/plot projection options (non-map based;
         e.g. dumbbell; double gyre grids)
   - [ ] Enhanced plotting support
   - [ ] Allow export of MOM6 grid to ROMS
     - [ ] implement ROMS.extend_ROMS_grid()
   - [ ] Grid filling options (flooding) (ice9)
   - [ ] Enable gridtools library to be installable via conda

# BUGS
 - [ ] latitude is not reliably reproduced between platforms; other
       fields show reproducibility.
 - [ ] A nested dictionary will clobber other nested elements instead
       of updating elements.  Recode `setPlotParameters` and
       `setGridParameters` to recursively update dictionary elements.
 - [ ] Regular filenames should be usable everywhere that takes file or
       data source arguments.

# TASKS

 - [ ] Sponge data preparation
   - [ ] Current scripts generate u,v fields on h-points; this needs
         to be changed to C-grid u/v-points instead
 - [ ] general documentation
   - [ ] include local markdown files
     - [ ] Use m2r2 to create a one way connection and compile local
           .md files into .rst files for use in the sphinx documentation.
 - [ ] grid creation
   - [ ] grid metrics
     - [ ] Copy cartesian solution for Niki''s ROMS to MOM6 converter
   - [ ] make Lambert Conformal Conic Grids; needs testing
     - [ ] LCC cannot take custom lat_1 and lat_2; it generates 
           lat_1 and lat_2 based on grid inputs.
     - [X] Update new lat_1 and lat_2 for application after makeGrid() is run
     - [ ] changing plot parameters lat_1 and lat_2 do not seem to impact the view
   - [ ] make Mercator grids; needs testing
     - [ ] issue a warning if tilt is non-zero - disabled
     - [ ] having tilt may not produce conformal grids
     - [X] Niki''s example added; but it may not be correct
     - [ ] Niki might have solved lat lon tilt?
   - [ ] make Stereographic grids
     - [ ] user testing
   - [ ] grid generation in other projections (tri-polar, etc)
   - [ ] on saveGrid():
     - [X] convert lon [+0,+360] to [-180,+180]
     - [ ] Unify code that adjusts lon (PR#1)
   - [ ] Verify unification of radius (R) throughout code
 - [ ] grid mask editor (land, etc)
     - [ ] add routines for mask checking
     - [ ] add routines for updating the exchange grid masks
     - [ ] Obey `MASKING_DEPTH`, `MINIMUM_DEPTH`, `ALLOW_LANDMASK_CHANGES`,
           `MAXIMUM_DEPTH`, `TOPO_EDITS_FILE` MOM6/src/initialization parameters
     - [ ] needs upgrade from basemap()
 - [ ] integration of data sources
   - [ ] generic regridder for creating boundary files (OBCs) from data sources
   - [ ] xesmf regridder for bathymetry sources
   - [X] option to create land mask fraction
   - [ ] option to use source grid as a supergrid for coarsening
   - [ ] refactor function arguments into kwargs
   - [ ] refactor print statements to use gridtools logging facility
 - [ ] integration of bathymetric sources and apply to grids
   - [X] https://github.com/nikizadehgfdl/ocean\_model\_topog\_generator
   - [ ] fix native zero band columns in partitions
   - [ ] flexible partitioning / rework
   - [ ] Investigate `get_indices1D()` function
   - [ ] Rework detection of grid bounds
   - [ ] Rework calculation of input grid points available vs grid
         points utilized
   - [ ] Rework for use with periodic grids
   - [ ] Include metadata for each partition: number of refinements, etc
   - [ ] implement useFixByOverlapQHGridShift so a regular grid can be
         used without a shift
   - [ ] Implement `TOPO_EDITS_FILE` in bathytools.applyExistingLandMask()
 - [ ] improve reproducibility
   - [X] include a dump of conda environment in the grid file (nc)
   - [X] add sha256 to grid and variable arrays
   - [ ] if conda environment does not exist, do some other snooping
 - [ ] Add option to use numpypi package (Alistair) as a configurable
       option in gridtools
 - [X] add datashader and numpypi from github sources; see postBuild script
   - [ ] implement and document in application
   - [ ] implement and document for programming use
 - [ ] on load of a grid into gridtool library
   - [ ] calculate R
   - [ ] calculate tilt (may not be possible)
   - [ ] update any tool metadata that is appropriate for that grid
   - [ ] parse and utilize any available proj string; must be a global
         or variable attribute
 - [ ] Using xesmf regridder and other tools to create bathymetry and
       other forcing and boundary files
 - [ ] Develop a field "flood" routine similar to pyroms
 - [ ] Perform checks for ensureEvenI and ensureEvenJ everywhere.
       This applies only to the grid not the supergrid.

# TODO

 - [ ] Grid plotting
   - [X] Grid
   - [X] Gridboxes
   - [ ] Supergrid
 - [ ] Generic plotting of figures
   - [ ] Scour MOM6-examples/tools for useful code; analysis/m6plot.py
     - [ ] May hold solution to plotting grid vertices and center values
   - [ ] Grid, data or other values (from data sources or script generated)
   - [ ] Support other colorbar options: orientation, et al
   - [ ] Support for custom colorbar ranges
   - [ ] Allow for one plot
   - [ ] Allow for two or more plots; side by side (sbs)
         or (stack)ed top to bottom
   - [ ] Allow for four plots
   - [ ] Allow paper mode: portrait
   - [ ] Allow paper mode: landscape
   - [ ] Allow paper mode: custom/poster
 - [ ] Add "Refresh Plot" buttons to other Plot tabs or figure out how
       to squeeze a single plot button into the layout
 - [ ] Do we have to declare everything in __init__ first or can be
       push all that to respective reset/clear functions?
 - [ ] refactor messaging/logging of GridUtils into its own package
       so we can import printMsg/debugMsg as standalone calls
 - [ ] Add a formal logging/message mechanism.
   - [X] Allow display of important messages and warnings in panel
         application: widget=TextAreaInput
   - [X] Create options in application and other tools for user
         configuration of logging and output.
   - [X] Create a message buffer/system for information.
   - [ ] Create a way to monitor a log file;
         https://discourse.holoviz.org/t/scrollable-log-text-viewer/317
   - [ ] log/display github revision of gridtools used by mybinder.org instances
 - [ ] For now, the gridParameters are always in reference to a center
       point in a grid in the future, one may fix a side or point of
       the grid and grow out from that point instead of the center.
 - [ ] application
   - [ ] enable user configurable plot and widget sizes (hardcoded in __init__)
   - [ ] enable user to change ellipsoid, R, `x_0` and `y_0` grid and
         plot parameters
   - [ ] plotting: adjust `satellite_height`, for now it is fixed to the default
   - [ ] title is misleading; it should show the projections in use if different
 - [ ] Develop a GridUtils() function
   - [ ] Run `proj -le` and return the names or display the details
   - [ ] Populates the ellps field for the application
 - [ ] `x_0` and `y_0` are hard coded to be zero offsets.  The user can
        modify these values.
 - [ ] Deploy use of self.gridMade (robTest:PR#1)
   - [ ] After success in makeGrid()
   - [ ] Successful load of grid from a file
   - [ ] Reset appropriately when clearGrid() is called
 - [ ] More contemplation of longitude range with respect 0, +/-180, 360.
   - [ ] How does this library respond for grids draped over 0 degree
         longitude vs +/-180 degrees longitude
 - [ ] numpypi
   - [ ] a test fails in `test_trunc.py`
 - [ ] Add testing harnesses.
   - [X] pytest: This will allow testing of core code via command line
         and iterative methods.
   - [ ] pytest: Setup some simple projection tests: IBCAO, ....
   - [ ] pytest: Refactor numpypi into structured tests under pytest
   - [ ] pytest: allow certain tests to fail if a module is not
         available (issue warnings instead)
   - [ ] selenium: Testing interactive methods may be harder.

# WISH

 - [ ] Write example program(s) that utilizes dask
   - [ ] Example 4: mkGridsEample4a.ipynb is incomplete
 - [ ] Support for multiple tiles for a model grid
 - [ ] Harmonize filename operations in functions
 - [ ] Teach grid tools to use "input.nml" to find grid related things
       for model runs.
 - [ ] Investigate the differences between FRE-NCtools vs gridutils.  Are
       there things that we could use there instead of recreating many wheels.
       There are lot of FRE-NCtool references in the ROMS to MOM6
       conversion tool.
 - [ ] Migrate to use of file:// or http://, https:// for file specifications.
 - [ ] Allow gridtools to continue to operate with some disabled
       routines that use xesmf.
 - [ ] app:Save remote files; additional sanity checks
 - [ ] app:Add an activity spinner to indicate the notebook is busy
 - [ ] Compute `angle_dy` for testing of grid conformality.  Theoretically,
       we can do this check for all grid and supergrid cells.
 - [ ] tripolar grids: use FRE-NCtools via cython?
 - [ ] Bring in code that converts ROMS grids to MOM6 grids
   - [ ] Allow conversion of MOM6 grids to ROMS grids
 - [ ] grid reading and plot parameter defaults should be dynamic with
       grid type declaration and potentially split out into separate
       library modules? lib/gridTools/grids/{MOM6,ROMS,WRF}
 - [ ] Place additional metadata into MOM6 grid files
   - [X] Grid parameters
   - [X] Software stack, git information
     - [ ] Update to utils.get_git_repo_version_info()
   - [ ] Alternate version/software capture if conda and/or git is not available
   - [X] Added proj string to netCDF file
   - [ ] Tri polar grid description
   - [X] Update conda capture code so a temporary file is not necessary
 - [ ] Work with generic non-mapping reference systems for use with
       some of the sample MOM6 problems
   - [ ] MOM6-examples: double_gyre
     - [ ] https://github.com/NOAA-GFDL/MOM6-examples/blob/dev/gfdl/ocean_only/double_gyre/Visualizing%20and%20animating%20sea-surface%20height.ipynb
     - [ ] https://gist.github.com/adcroft/2a2b91d66625fd534372
   - [ ] MOM6 dumbbell: https://github.com/NOAA-GFDL/MOM6/search?q=dumbbell
     - [ ] Support for grid variable and plotting
     - [ ] Learn about OBC preparation and sponges
 - [ ] Refactor any grid math into a gridmath library. Any grid
       computation that can stand on its own should be moved into a
       separate gridmath library.
 - [ ] gridtools.makeGrid() will need a refactor to work with other grid types
 - [ ] write out all MOM6 ancillary files when writing a grid
 - [ ] refactor expansion/clipping of grid points when fitting grid
 - [ ] Add a notebook or two that demonstrates some of the esoteric API
       features of the library: help, debugging, etc.
 - [ ] Dask optimizations
   - [ ] creating the native IBCAO grid is too big for mybinder.org
 - [ ] Subset any grid for running with MOM6
   - [ ] https://github.com/ESMG/regionalMOM6_notebooks/tree/master/creating_obc_input_files
   - [ ] May be especially useful for debugging situations; Arctic6
 - [ ] Allow gridtools to be used without xesmf and xgcm; enable module
       detection for available capabilities
 - [ ] Update setup.py and other files with package dependencies
   - [ ] Create a configuration script that would perform autosetup of
         gridtools library
 - [ ] Pull in BC and forcing fields from various sources
   - [ ] Delta method: "We extract 20-30 years of a future projection
         from several models, build an average of each forcing variable
         which we superpose on modern day climate.  It’s the so-called
         delta method.  It debiases climate projections relative to
         modern day (reanalysis constrained) dynamics, but adds the
         climate change signal on top of it (as a secular change/delta)."
   - [ ] CMIP/ESM
     - [ ] Browser catalog: https://esgf-node.llnl.gov/search/cmip6/
     - [ ] Programmatic access: https://github.com/intake/intake-esm
     - [ ] http://gallery.pangeo.io/repos/pangeo-gallery/cmip6/
     - [ ] https://github.com/aradhakrishnanGFDL/gfdl-aws-analysis/tree/master/examples
     - [ ] https://github.com/aradhakrishnanGFDL/gfdl-aws-analysis/tree/master/esm-collection-spec-examples
     - [ ] https://github.com/MackenzieBlanusa/OHC_CMIP6
     - [ ] https://github.com/xarray-contrib/cf-xarray
     - [ ] https://github.com/jbusecke/cmip6_preprocessing
 - [ ] triton node issue: python netCDF4 large file reading seems to hang nodes
 - [ ] Add an Example 7a to demonstrate using existing files from Example 7.
 - [ ] Update all references to field to either variable or grid
       depending on context.

# QUESTIONS

 - [ ] ntiles,1 is written in write_MOM6_topography_file, is this
       required for MOM6?