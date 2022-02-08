# beemeteo

[![CI](https://github.com/beedata-analytics/beemeteo/actions/workflows/main.yml/badge.svg)](https://github.com/beedata-analytics/beemeteo/actions/workflows/main.yml)
[![Python version](https://img.shields.io/badge/python-2.7-blue)](https://img.shields.io/badge/python-2.7-blue)
[![Python version](https://img.shields.io/badge/python-3.8-blue)](https://img.shields.io/badge/python-3.8-blue)
[![Dependencies Status](https://img.shields.io/badge/dependencies-up%20to%20date-brightgreen.svg)](https://github.com/beedata-analytics/beedatadis/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Aapp%2Fdependabot)
[![Pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/beedata-analytics/beedatadis/blob/master/.pre-commit-config.yaml)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Imports: isort](https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat&labelColor=ef8336)](https://pycqa.github.io/isort/)


A python package and command line tool to download weather data from different sources.

## Sources Available

- [CAMS Radiation Service](http://www.soda-pro.com/web-services/radiation/cams-radiation-service/info) (CAMS)
  - Provides Europe and Africa historical solar radiation data ![OK](https://img.shields.io/badge/-OK-green)
- [Dark Sky API](https://darksky.net/dev) (DarkSky)
  - Provides worldwide historical meteorological data ![OK](https://img.shields.io/badge/-OK-green)
  - Provides worldwide prediction meteorological data ![NO](https://img.shields.io/badge/-OK-red)
- [MeteoGalicia](http://mandeo.meteogalicia.es) (MeteoGalicia)
  - Provides Europe historical solar radiation data ![OK](https://img.shields.io/badge/-OK-green)
  - Provides Europe prediction solar radiation data ![NO](https://img.shields.io/badge/-OK-red)
## Installation

To install the package, use pip and the Git repository at the newest version

```bash
pip install https://github.com/BeeGroup-cimne/beemeteo@v0.1.0
```
## Usage
The application can be used as a command line or imported in a python package.

### Timezones

When downloading data from a location, the application will detect the timezone of the location and download and return the data in the location's timezone.

If the passed datetime don't contain any timezone (they are naive). The location's timezone will be used as their timezone

If the passed datetime contains a timezone (not naive). The date will be translated to the location's timezone to perform the downloading.

### command line

To use the command line, the python package must be installed in the python environment of your computer. 

The execution of the command line program will generate a CSV file with the specified name in the "file" parameter
```console
$ python -m beemeteo --help

usage: __main__.py [-h] -s SOURCE -c CONFIG -lat LATITUDE -lon LONGITUDE -d1 DATE_FROM -d2 DATE_TO -f FILE

optional arguments:
  -h, --help            show this help message and exit
  -s SOURCE, --source SOURCE
                        The source to get data from one of [DarkSky, CAMS, MeteoGalicia]
  -c CONFIG, --config CONFIG
                        The configuration file path
  -lat LATITUDE, --latitude LATITUDE
                        The latitude
  -lon LONGITUDE, --longitude LONGITUDE
                        The longitude
  -d1 DATE_FROM, --date_from DATE_FROM
                        The date to get weather from
  -d2 DATE_TO, --date_to DATE_TO
                        The date to get weather to (included)
  -f FILE, --file FILE  The file to store the data

Example: python -m beemeteo \
--name DarkSky \
--filename config.json \
--latitude 41.29 \
--longitude 2.19 \
--date-from 2021-09-01 \
--date-to 2021-09-05
--file data_file.csv
```

### python package
To use the package in python, you have to import the different data_sources, and use the functions to use them

#### cams

```python
import datetime
import os
import pytz
from beemeteo.sources.cams import CAMS

source = CAMS("config.json")
source.get_data(
    41.29,
    2.19,
    datetime.datetime(2021, 9, 1),
    datetime.datetime(2021, 9, 5),
)
```

#### darksky

```python
import datetime
import os

import pytz

from beemeteo.sources.darksky import DarkSky

source = DarkSky("config.json")
source.get_data(
    41.29,
    2.19,
    datetime.datetime(2021, 9, 1),
    datetime.datetime(2021, 9, 5),
)
```

#### meteogalicia

```python
import datetime
import pytz
from beemeteo.sources.meteogalicia import MeteoGalicia
source = MeteoGalicia("config.json")
source.get_data(
    41.29,
    2.19,
    datetime.datetime(2021, 9, 1),
    datetime.datetime(2021, 9, 5),
)
```
