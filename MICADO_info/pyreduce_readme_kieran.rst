PyReduce Handover
=================

Installing PyReduce
-------------------
See Werner's email for installing PyReduce
Basically just clone the github repository into a fresh folder:

    $ git clone https://github.com/AWehrhahn/PyReduce.git


Creating a MICADO package
-------------------------
There are 6 files (data/scripts) that need to be created to use PyReduce for MICADO:

- A Table (numpy recarray) with initial guess for wavelength calibration (wavecal/micado.npz)
- A config file to map header keywords to Pyreduce keywords (instruments/micado.json)
- The instrument description, which contains code for subclassing the Instrument object (instruments/micado.py)
- A config file for the PyReduce reduction steps (settings/settings_micado.json)
- The running script, which creates an instance of the MICADO subclass and calls the reduce method. (examples/micado_example.py)
- A bad pixel mask FITS image. For MICADO this can be a single empty array the size of the MICADO detectors (masks/mask_micado.fits.gz)


Work plan
#########

- [Nadeen, Wolfgang] - 3. Wavecal initial guess (micado.npz)
- [Wolfgang] - 2. Header keyword map (micado.json)
- [Hugo, Kieran?] A. Subclassing Instrument for MICADO (micado.py)
- [Nadeen] - 1. PyReduce runtime config file (settings_micado.json)
- [Nadeen] - B. PyReduce runtime script (example_micado.py)
- [Nadeen] - 4. Bad pixel mask FITS file (mask_micado.fits.gz)


Where to place files
####################

- micado.npz              --> pyreduce/wavecal/micado.npz
- settings_micado.json    --> pyreduce/settings/settings_micado.json
- mask_micado.fits.gz     --> pyreduce/mask/mask_micado.fits.gz
- micado.json             --> pyreduce/instruments/micado.json
- micado.py               --> pyreduce/instruments/micado.py
- micado_example.py       --> examples/micado_example.py


1 Reduction steps settings file
###############################
FILE TO CREATE: settings/settings_micado.json

Contains all the run-time settings for a MICADO pyreduce workflow

- The settings_schema file does a good job at describing what each parameter means. However I have no idea what "normal" values would be. I guess just leave the defaults for the moment.
- see files in folder: /PyReduce/pyreduce/settings/
- default parameters are in: /PyReduce/pyreduce/settings/settings_pyreduce.json
- description of parameters: /PyReduce/pyreduce/settings/settings_schema.json


2 File sorting settings file
############################
FILE TO CREATE: instruments/micado.json

- Basically just a json file that maps the unreduced FITS header keywords to the keywords that PyReduce requires to run
- A json file following the formats given in: /PyReduce/pyreduce/instruments/instrument_schema.json


3 Wavelength calibration file
#############################
FILE TO CREATE: wavecal/micado.npz

A Table (numpy recarray) with initial guess for wavelength calibration

- npz file (pickled numpy recarray). See docs for contents.
- see files in folder: /PyReduce/pyreduce/wavecal
- The online documentation describes what needs to be included in this file.
    - I don't know what all the columns mean, but hopefully Nadeen does.


4 Mask file
###########
FILE TO CREATE: masks/mask_micado.fits.gz

Bad pixel mask as a FITS file

- Can just be an FITS file with a zeros-array in each of the extensions for the MICADO detectors.
- see files in folder: /PyReduce/pyreduce/masks


A Code subclass based on Instrument
###################################
FILE TO CREATE: instruments/micado.py

A python script that contains a subclassed version of the Instrument class, tailored to MICADO

- see the instrument specific python files in folder: /PyReduce/pyreduce/instruments/<>.py

Needs to contain the following methods:

- load_info():
  Reads in the MICADO json settings file. Returns a dict
- sort_files():
  Sorts the bias, flat, wavecal, orderdef, spec files so that they can be passed to each reduction step. Returns a list of dicts.
- add_header_info():
  reformat the MICADO speific header keywords into format needed by the REDUCE algorithm (i.e. header keywords starting with "e_****")
- get_wavecal_filename():
  returns the file path to the wavelength calibration first guess file. A .npz file, see abover
- get_mask_filename():
  returns a file path to the file containing the bad pixel mask. A FITS HDUList file.
		

B The running script
####################
FILE TO CREATE: examples/micado_example.py

A python script that contains the running commands
- see the files in : /PyReduce/examples/<>.py






