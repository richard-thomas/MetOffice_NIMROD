# MetOffice_NIMROD
Python module to extract data from UK Met Office Rain Radar NIMROD image files.

Features: parses NIMROD format image files, displays header data and allows extraction of
raster image to an ESRI ASCII (.asc) format file. A bounding box may be
specified to clip the image to the area of interest. Can be imported as a
Python module or run directly as a command line script.

(This module is developed from a basic Python script written for a hydrological
modelling assignment of my [GIS MSc](http://richard-thomas.github.io/GIS_MSc/)).

Command line usage:
```
  python nimrod.py [-h] [-q] [-x] [-bbox XMIN XMAX YMIN YMAX] [infile] [outfile]

positional arguments:
  infile                (Uncompressed) NIMROD input filename
  outfile               Output raster filename (*.asc)

optional arguments:
  -h, --help            show this help message and exit
  -q, --query           Display metadata
  -x, --extract         Extract raster file in ASC format
  -bbox XMIN XMAX YMIN YMAX
                        Bounding box to clip raster data to
```

Note that any bounding box must be specified in the same units and projection
as the input file. The bounding box does not need to be contained by the input
raster but must intersect it.

Example command line usage:
```
  python nimrod.py -bbox 279906 285444 283130 290440
    -xq 200802252000_nimrod_ng_radar_rainrate_composite_1km_merged_UK_zip
    plynlimon_catchments_rainfall.asc
```

Example Python module usage:
```
    import nimrod
    a = nimrod.Nimrod(open(
        '200802252000_nimrod_ng_radar_rainrate_composite_1km_merged_UK_zip', 'rb'))
    a.query()
    a.extract_asc(open('full_raster.asc', 'w'))
    a.apply_bbox(279906, 285444, 283130, 290440)
    a.query()
    a.extract_asc(open('clipped_raster.asc', 'w'))
```

Notes:
  1. Valid for v1.7 and v2.6-4 of NIMROD file specification
  2. Assumes image origin is top left (i.e. that header[24] = 0)
  3. Tested on UK composite 1km and 5km data, under Linux and Windows XP
  4. Further details of NIMROD data and software at the [NERC BADC]
  (http://badc.nerc.ac.uk/browse/badc/ukmo-nimrod/) website:
         
----
Copyright (c) 2015 Richard Thomas

(`Nimrod.__init__()` method based on read_nimrod.py by Charles Kilburn Aug 2008)

This program is free software: you can redistribute it and/or modify
it under the terms of the Artistic License 2.0 as published by the
Open Source Initiative (http://opensource.org/licenses/Artistic-2.0)

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
