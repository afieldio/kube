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


## Dump Commands
--device-index <index>   Select RTL device (default: 0)
--gain <db>              Set gain (default: max gain. Use -10 for auto-gain)
--enable-agc             Enable the Automatic Gain Control (default: off)
--freq <hz>              Set frequency (default: 1090 Mhz)
--ifile <filename>       Read data from file (use '-' for stdin)
--iformat <format>       Sample format for --ifile: UC8 (default), SC16, or SC16Q11
--throttle               When reading from a file, play back in realtime, not at max speed
--interactive            Interactive mode refreshing data on screen. Implies --throttle
--interactive-rows <num> Max number of rows in interactive mode (default: 22)
--interactive-ttl <sec>  Remove from list if idle for <sec> (default: 60)
--interactive-rtl1090    Display flight table in RTL1090 format
--raw                    Show only messages hex values
--net                    Enable networking
--modeac                 Enable decoding of SSR Modes 3/A & 3/C
--net-only               Enable just networking, no RTL device or file used
--net-bind-address <ip>  IP address to bind to (default: Any; Use 127.0.0.1 for private)
--net-ri-port <ports>    TCP raw input listen ports  (default: 30001)
--net-ro-port <ports>    TCP raw output listen ports (default: 30002)
--net-sbs-port <ports>   TCP BaseStation output listen ports (default: 30003)
--net-bi-port <ports>    TCP Beast input listen ports  (default: 30004,30104)
--net-bo-port <ports>    TCP Beast output listen ports (default: 30005)
--net-ro-size <size>     TCP output minimum size (default: 0)
--net-ro-interval <rate> TCP output memory flush rate in seconds (default: 0)
--net-heartbeat <rate>   TCP heartbeat rate in seconds (default: 60 sec; 0 to disable)
--net-buffer <n>         TCP buffer size 64Kb * (2^n) (default: n=0, 64Kb)
--net-verbatim           Do not apply CRC corrections to messages we forward; send unchanged
--forward-mlat           Allow forwarding of received mlat results to output ports
--lat <latitude>         Reference/receiver latitude for surface posn (opt)
--lon <longitude>        Reference/receiver longitude for surface posn (opt)
--max-range <distance>   Absolute maximum range for position decoding (in nm, default: 300)
--fix                    Enable single-bits error correction using CRC
--no-fix                 Disable single-bits error correction using CRC
--no-crc-check           Disable messages with broken CRC (discouraged)
--mlat                   display raw messages in Beast ascii mode
--stats                  Print stats at exit
--stats-range            Collect/show range histogram
--stats-every <seconds>  Show and reset stats every <seconds> seconds
--onlyaddr               Show only ICAO addresses (testing purposes)
--metric                 Use metric units (meters, km/h, ...)
--gnss                   Show altitudes as HAE/GNSS (with H suffix) when available
--snip <level>           Strip IQ file removing samples < level
--debug <flags>          Debug mode (verbose), see README for details
--quiet                  Disable output to stdout. Use for daemon applications
--show-only <addr>       Show only messages from the given ICAO on stdout
--ppm <error>            Set receiver error in parts per million (default 0)
--html-dir <dir>         Use <dir> as base directory for the internal HTTP server. Defaults to ./public_html
--write-json <dir>       Periodically write json output to <dir> (for serving by a separate webserver)
--write-json-every <t>   Write json output every t seconds (default 1)
--json-location-accuracy <n>  Accuracy of receiver location in json metadata: 0=no location, 1=approximate, 2=exact
--dcfilter               Apply a 1Hz DC filter to input data (requires lots more CPU)
--help                   Show this help

Debug mode flags: d = Log frames decoded with errors
                  D = Log frames decoded with zero errors
                  c = Log frames with bad CRC
                  C = Log frames with good CRC
                  p = Log frames with bad preamble
                  n = Log network debugging info
                  j = Log frames to frames.js, loadable by debug.html
