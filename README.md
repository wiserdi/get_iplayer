## get_iplayer: BBC iPlayer/BBC Sounds Indexing Tool and PVR

## Features

* Downloads TV and radio programs from BBC iPlayer/BBC Sounds
* Allows multiple programs to be downloaded using a single command
* Indexing of most available iPlayer/Sounds catch-up programs from previous 30 days (not Red Button, iPlayer Exclusive, or Podcast-only)
* Caching of programs index with automatic updating
* Regex search on programs name
* Regex search on programs description and episode title
* Filter search results by channel
* Direct download via programs ID or URL
* PVR capability (may be used with cron or Task Scheduler)
* HTTP proxy support
* Perl 5.16+ required, plus LWP, LWP::Protocol::https, XML::LibXML, Mojolicious, and CGI modules
* Requires ffmpeg for conversion to MP4 and AtomicParsley for metadata tagging
* Runs on Linux/BSD (Ubuntu, Fedora, OpenBSD and others), macOS (10.10+), Windows (7/8/10)

**NOTE:**

- **get_iplayer can only search for programs that were scheduled for broadcast on BBC linear services within the previous 30 days, even if some are available for more than 30 days on the iPlayer/Sounds sites. Red button programs, iPlayer box sets, web-only content, and BBC podcasts are not searchable. programs that are still available after 30 days must be located on the iPlayer/Sounds sites and downloaded directly via PID or URL.**
- **get_iplayer does not support downloading news/sport videos, other embedded media, archive programs, special collections, educational material, programs clips or any content other than whole episodes of programs scheduled for broadcast on BBC linear services within the previous 30 days. However, it is generally possible to download other content such as red button programs, iPlayer box sets, or podcasts directly via PID or URL. get_iplayer DOES NOT support live recording from BBC channels.**

## Documentation

<https://github.com/get-iplayer/get_iplayer/wiki>

## Installation

<https://github.com/get-iplayer/get_iplayer/wiki/installation>

## Usage

	get_iplayer --help
	get_iplayer --basic-help
	get_iplayer --long-help

## Examples

* List all TV programs (`--type=tv` set by default):

	`get_iplayer ".*"`

	Search output appears in this format:

		...
		208:  Doctor Who: Series 7 Part 2 - 1. The Bells of Saint John, BBC One, b01rryzz
		209:  Doctor Who: Series 7 Part 2 - 2. The Rings Of Akhaten, BBC One, b01rx0lj
		210:  Doctor Who: Series 7 Part 2 - 3. Cold War, BBC One, b01s1cz7
		...

	Format = `<index>: <name> - <episode>, <channel>, <pid>`

* List all TV programs with long descriptions:

	`get_iplayer --long ".*"`

* List all radio programs:

	`get_iplayer --type=radio ".*"`

* List all TV programs with "doctor who" in the name (matching is case-insensitive):

	`get_iplayer "doctor who"`

* List all TV and radio programs with "doctor who" in the name:

	`get_iplayer --type tv,radio "doctor who"`

* List all BBC One programs:

	`get_iplayer --channel="BBC One" ".*"`

* List Radio 4 and Radio 4 Extra programs with "Book at Bedtime" in the title:

	`get_iplayer --type=radio --channel="Radio 4" "Book at Bedtime"`

* List only Radio 4 programs with "Book at Bedtime" in the title:

	`get_iplayer --type=radio --channel="Radio 4$" "Book at Bedtime"`

	*(The `$` regular expression metacharacter matches "Radio 4" only at the end of the channel name, thus avoiding matches against "Radio 4 Extra")*

* Record TV programs number 208 (index from search results) in HD, with fallback to lower quality if not available:

	`get_iplayer --get 208` [default setting]

	or
	
	`get_iplayer --get 208 --tv-quality=hd,sd,web,mobile` [explicit setting]

* Record TV programs number 208 in lower resolution only (704x396@50):

	`get_iplayer --get 208 --tv-quality=web`

* Record TV programs number 208 and download subtitles in SubRip (SRT) format:

	`get_iplayer --get 208 --subtitles`

* Record multiple TV programs (using index numbers from search results):

	`get_iplayer --get 208 209 210`

* Record a TV programs using its iPlayer URL:

	`get_iplayer https://www.bbc.co.uk/iplayer/episode/b01sc0wf/Doctors_Series_15_Perfect/`

* Record a TV programs using the PID (b01sc0wf) from its iPlayer URL:

	`get_iplayer --pid=b01sc0wf`

* Record a radio programs using its Sounds URL:

    `get_iplayer https://www.bbc.co.uk/sounds/play/b07gcv34`

* Record a radio programs using the PID (b07gcv34) from its Sounds URL in high quality (320k), with fallback to lower quality if not available (default setting):

	`get_iplayer --pid=b07gcv34` [default setting]

	OR

	`get_iplayer --pid=b07gcv34 --radio-quality=high,std,med,low` [explicit setting]

* Record a radio programs using the PID (b07gcv34) from its Sounds URL with lower bit rate only (96k):

	`get_iplayer --pid=b07gcv34 --radio-quality=med`

* Record multiple radio programs (using PIDs from Sounds URLs):

	`get_iplayer --pid=b07gcv34,b07h60ld` [comma-separated list]

	OR

	`get_iplayer --pid=b07gcv34 --pid=b07h60ld` [multiple arguments]

NOTE: Sometimes you may not be able to download a listed programs immediately after broadcast (usually available within 24hrs of airing). Some BBC programs may not be available from iPlayer/Sounds.
