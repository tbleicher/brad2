## Features planned for the first release ##

This is the minimum set of features the first release of brad2
should support. Some features which are currently available in
either brad or bliff will not be present in the first release to
give us more time for a proper implementation.

  * install procedure (new)
  * i18n
  * export of static scenes
  * render settings window (bliff or brad model?)
  * sky options for gensky/gendaylit
  * layer manager (new)
  * basic material support (from bliff)
    * load files manually into text object
    * material files can contain meta information (i.e. preview) in comments
  * basic luminaire support (from bliff)
    * load IES files manually from file system
    * save IES text to text object in Blender
  * numeric object support (new)

## Features planned for later releases ##

  * Radiance scene file import
  * HDR skies
  * dynamic daylight simulations
  * animation and time sequences
  * improved luminaire support
    * luminaire database connection
    * ldt2ies.py or ldt2rad.py script
  * improved material support
    * material database connection
    * material editor
