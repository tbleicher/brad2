## Design Goal ##

One of the main ideas behind the brad rewrite is to allow allow
other Python scripts the import and use of brad2 functionality.
This is achieved by introducing a new intermediate level
between the user front end code and the low level Radiance processes
and files. To keep these modules reusable by other scripts this level
is further divided in a functional part that only depends on Python
and Python modules and a Blender dependent part with modules for the
GUI and the interaction with 3D scene elements.

![http://tbleicher.googlepages.com/brad2_modules.png](http://tbleicher.googlepages.com/brad2_modules.png)

TODO: update module names in picture


#### Python dependent modules ####

  * **radlib**
> > This is a new Python library which will define a set of interface
> > classes to the Radiance render processes and files. In the future
> > this might be done as a C extension based on the Radiance sources
> > but for now we will have to do with system commands and plain Python.

  * **bradbase**
> > This module defines the framework for the higher level modules.
> > Classes defined here handle logging, session state, config options
> > etc. The non-gui dependent scripts could do without it but since
> > we need it for the GUI classes it should be used by the other
> > components as well.

  * **bradproc**
> > These classes provide control over complex rendering processes like
> > vector illuminance or DDS. They will use radlib to interface to the
> > Radiance command line tools. Their interface should be designed for
> > GUI wrappers as well as simple command line tools.

  * **bradaux**
> > This is the place for extension that are not essential for the
> > functionality of brad2 as an exporter. They will provide code for
> > visualisations etc. which uses advanced Python libraries. If these
> > libraries are not found on the system the functionality in these modules
> > will not be available on only to a small extend.

  * **scripts**
> > This is not a module but only a collection of command line scripts.
> > Some of them have a functionally equivalent GUI component in the
> > brad2 module while others provide background services the GUI may
> > rely on - like a rendering or database daemon.


#### Blender dependent modules ####

  * **bradscene** (was: bliff.scene)
> > Classes in bradscene provide a higher level interface to the Blender
> > scene objects. They will provide methods for the special requirements
> > of a Radiance exporter which will make the gui modules less complex.
> > Additionally it defines an abstraction layer to the ever changing
> > Blender PyAPI which provides a way of migrating from one PyAPI version
> > to the next.

  * **bradtk** (was: bliff.base)
> > This module defines the gui components that are used to build the
> > brad2 interface. It deals with the underlying Blender.Draw and Blender.BGL
> > modules. Most gui components can be created with the functions
> > defined here and do not need to access BGL or Draw directly.

  * **brad2**
> > This module defines the final exporter interface and functionality
> > by combining the Blender and Radiance modules into GUI components.



## File System Hierarchy ##

#### brad2 modules - the exporter components ####

This section outlines a file system hierarchy for the new modules.
The best solution seems to be flat hierarchy of module directories.
The top level directory has to be in PYTHONPATH which will include
it in the Blender modules search path as well.

```
brad2modules : [should be in PYTHONPATH]
  |
  +== brad2 : the main code base for the exporter
  |     |     [ this needs to be in the Blender Python search path ]
  |     | 
  |     +-- export : create Radiance project files
  |     +-- luminaire : make luminaire data available in Blender
  |     +-- material : assign or create materials
  |     +-- scene : combine all settings for Radiance export
  |   [ +-- plot : visualise numeric data (was rtrace) ]
  |   [ +-- monitor : display of render progress       ]
  |
  +== bradaux : non essential modules
  |     |
  |     +-- VTK
  |     +-- OpenDX
  |     +-- database code
  |     +-- pdfreport : create and preview PDF reports
  |
  +== bradbase : framework classes (was 'bliff.borg')
  |     |
  |     +-- config
  |     +-- i18n
  |     +-- logging
  |     +-- session
  |
  +== bradproc : interface to Radiance processes and files
  |     |
  |     +-- genHDRSky
  |     +-- bradd : network render daemon
  |     +-- lumdbd : luminaire data base
  |     +-- matdbd : material data base
  |     
  +== bradscene : wraps the Blender scene objects
  |     |
  |     +-- bradscene
  |     +-- bradmesh
  |     +-- bradlamp
  |     +-- bradcamera
  |     +-- bradmaterial
  |     +-- bradempty
  |
  +== bradtk - interface code (formerly bliff)
        |
        +-- base : frame superclasses and basic functional classes
        |          [BaseFrame, TextFrame, ImageFrame]
        | 
        +-- interface : controls events, user interaction and menus
        |               [Interface, Menu, settings, config, about, splash]
        |
        +-- layout : specialised frames for layout control
        |            [Layout, ColumnLayout, RowLayout]
        |
        +-- listframe : listbox related classes
        |
        +-- export_frames: render options and export summary
        +-- luminaire_frames : Radiance luminaire data
        +-- material_frames : Radiance material data
        +-- sky_frames: set sky options (gensky, gendaylit, HDR, weatherdata)    
```


#### radlib - Python API for Radiance ####

This library contains classes to interface to Radiance binaries
and Radiance scene files. These classes are useful for all scripts
that deal with Radiance related files. Ideally it would be a C-extension
based on the Radiance C-source code to avoid recoding of the existing
C functions. That might take a while, though.

The location of this directory should be in PYTHONPATH to allow
any script use of these classes and functions.


```
radlib : Python library related to Radiance apps and files
  |      [ this should be included in PYTHONPATH by the user        ]
  |      [ best way for this would be a Python C extension based on ]
  |      [ the Radiance sources - but that might take a while       ]
  |
  +-- replacement for rad in Python
  +-- parser for *.rif
  +-- parser for *.rad and *.mat
  +-- ies2rad in Python
  +-- ldt2rad
  +-- *.pic reading, pvalue/pfilt/pcond etc. in Python
  +-- unified command line handling (Pythons getopt is too limited)
  +-- sky : basic sky class for gensky and gendaylit
  +-- weatherDataFile
```


#### scripts - stand alone scripts and tools ####

This directory will contain utilities to simplify complex processes
for the end user. This directory should be included in the user's PATH.
Some of these scripts might even get a GUI front end in brad2 or provide
command line versions of GUI modules that can be used without Blender
running.

```
scripts
  |
  +-- brtrace : distribute numeric rendering to multiple processes
  +-- pdfplot : create PDF plots from numeric results
  +-- weatherdatapdf : create PDF plots from weather data files
  +-- [more]
```


