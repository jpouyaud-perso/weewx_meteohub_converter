# Meteohub raw data converter
A converter for [meteohoub](https://wiki.meteohub.de/Main_Page) raw data.
It converts meteohub raw data to csv, which is importable by **Meteotemplate**
## Development
this dev is now able to get consistent Rain cumul and manage also the timezone which is important ofr Meteotemplate

## Usage
This python module reads the meteohub raw data and converts it to csv files which can be imported to WeeWX using the WeeWX wee_import util.

`./wm_convert.py -i 202001/raw -o 202001.csv`

Then use *wee_import* to import the csv file to WeeWX, f.e. `wee_import --import-config=config/csv.conf`

I provided an example config file in */config*.

## How does it work?
The converter reads the given raw data file, sorts the datasets (lines) by date and writes this sorted data to a temporary file. (I don't want to touch the original data - just for the case that someone doesn't have a backup.) I discovered the issue that the meteohub raw data files may not be in chronological order. This causes issues during calcuating the time condensed input data. That's the cause why I sort the input data.
The converter reads now the data for 5 minute intervals and calculates the mean values for temperatures, humidity and wind. Rain will be summed up for that interval.
At the end the intervals are written to the output file in CSV format. You can import now this data to Meteotemplate. At the moment it makes sense, to look over the output file - you may open it in Excel or Numbers f.e. - and do a visual inspection.
the script is also removing incomplete raw/sample per 5 mn interval (important for some block/plugin to function well with Meteotemplate)

## Todo
- Support for sensors with leading 0 in the value string
- Adding support for importing a complete raw data path
- Implement T-sensors

## History
- Added input file sorting 
- fork
- corrected rain0 cumulative calcul
- added TempMin and TempMax per interval
- removed UV and solar things
- managed the case some sample are missing to avoid to feed Meteotemplate DB with Null value which break some BLock/plugin
