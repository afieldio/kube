## Overview
Running a NooElec rtl-sdr
Dump1090 processes data
Outputs to json files
These could be posted to a separate server for access

## Todo
Need to get this running on the Pi.
Should then be able to post the files regularly to another server for consumption


Need to source other data
* ATC Real Time data!
* Weather (METAR)
* ICAO hex codes for mapping aircraft (currently have the list from IATA in a csv)
* Flight Schedule Data
	* OAG
	* Flightaware


## Key Links
Main info from here - https://www.rtl-sdr.com/

Various forks of the DUMP1090 software that interprets / manages the data
Original - https://github.com/antirez/dump1090

https://github.com/flightaware/dump1090

https://www.developer.aero/


## Files
airline_code_countries.csv
- can be used to map the output from the aircraft.json / history files with the name of the airline operating them
- anything not on this list is probably not worthy of worrying about too much

*aircraft.json*
- live feed constantly updated

Example

 {	"hex":"4076e3",
 	"squawk":"6337",
 	"flight":"BAW2DP",
 	"lat":44.714355,
 	"lon":1.632475,
 	"nucp":7,
 	"seen_pos":0.8,
 	"altitude":17675,
 	"vert_rate":-1472,
 	"track":169,
 	"speed":352,
 	"category":"A3",
 	"mlat":[],
 	"tisb":[],
 	"messages":988,
 	"seen":0.1,
 	"rssi":-19.2
 },

 Things of note:
 * flight - allows us to get flight schedule data for specific day
 * lat, long - from this you should be able to determine distance left to run
 * speed - speed will reduce over time but the profile of a landing plane is usually pretty standard so you can estimate landing time
 * seen - gives an indication of 'freshness' of this data (if its not been seen for a while its probably not worth worrying about)

history
- contains snapshots in time of the aircraft.json file
- when program runs it just keeps on adding new files


--write-json <dir>       Periodically write json output to <dir> (for serving by a separate webserver)
--write-json-every <t>   Write json output every t seconds (default 1)


## Server
dump1090 does have an internal server but not entirely sure this would work very well
