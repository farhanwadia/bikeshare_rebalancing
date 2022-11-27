# Load Balancing for Shared Micromobility Services - Problem Dataset
Prepared by Talha Alvi and Farhan Wadia

## Introduction
The purpose of this project is to explore applications of bio-inspired search algorithms in solving the load-balancing and optimal availability problem (or similar, related problems) for a micromobility service. Specifically, we plan to use publicly available data from [Bike Share Toronto](https://bikesharetoronto.com/). A key challenge with the operation of bike sharing services is the undersupply of bikes in areas with high demand, and oversupply of bikes in areas with low demand changing rapidly over the course of a day. To ensure bikes are available at stations from where users want to start their journey, and that parking spaces will be available at their destination, bike sharing services use relocating trucks to move bikes from some locations to others. Ensuring that every station in the network has adequate availability of bikes, and scheduling the truck trip of which locations to pick up bikes from, how many to pick up, and where to distribute them is a non-trivial task. In addition to the use of relocating trucks, some bike share operators have also began providing on-demand user incentives such as reduced pricing, or even paying users, to move bikes between particular locations.

## Bike Share Toronto Datasets
Bike Share Toronto has several publicly available datasets which will be required for this project. The scope of data used for this project will be limited to data from October 2022.

### Ridership Dataset
Monthly, anonymized ridership trip data in csv format from 2020 onwards can be downloaded from [Bike Share Toronto Ridership Data - City of Toronto Open Data Portal](https://open.toronto.ca/dataset/bike-share-toronto-ridership-data/). 

Each csv file contains the following features:

`Trip Id`: ID for the trip
`Trip Duration`: Duration (in seconds) of the trip
`Start Station Id`: ID of the station where the trip started
`Start Time`: Date and time at the start of the trip
`Start Station Name`: Name of the start station (typically the intersection where the station is located) 
`End Station Id`: ID of the station where the trip ended
`End Time`: Date and time at the end of the trip
`End Station Name`: Name of the endstation (typically the intersection where the station is located) 
`Bike Id`: ID of the bike used
`User Type`: Type of User (annual member or casual member)
### Station Information Dataset
Station information data can be found at [Bike Share Toronto - City of Toronto Open Data Portal](https://open.toronto.ca/dataset/bike-share-toronto/) and downloaded as a json following the [General Bikeshare Feed Specification \(GBFS\)](https://github.com/MobilityData/gbfs) format from https://tor.publicbikesystem.net/ube/gbfs/v1/en/station_information. This dataset only needs to be downloaded once.

The json file contains the following features:

`station_id`: ID of a station
`name`: Name of a station (typically the intersection where it is located)
`physical_configuration`: Values of `REGULAR`, `ELECTRICBIKESTATION`, `SMARTMAPFRAME`, `REGULARLITMAPFRAME`, `SMARTLITMAPFRAME`
`lat`: Latitude of station
`lon`: Longitude of station
`altitude`: Altitude of station
`address`: Address of station
`capacity`: Number of bike docks at the station
`is_charging_station`: Values of `TRUE` or `FALSE`. Relevant for e-bike usage. 
`rental_methods`: All payment options available at the station (combinations of `KEY`, `TRANSITCARD`, `CREDITCARD`, `PHONE`
### Station Status Dataset
Station status data can be found at [Bike Share Toronto - City of Toronto Open Data Portal](https://open.toronto.ca/dataset/bike-share-toronto/) and downloaded as a json following the [General Bikeshare Feed Specification \(GBFS\)](https://github.com/MobilityData/gbfs) format from https://tor.publicbikesystem.net/ube/gbfs/v1/en/station_status. 

The `bikeshare_station_status_downloader.py` Python script was developed by our team and has been scheduled to run indefinitely using Windows Task Scheduler every 15 minutes starting October 10, 2022 at 3 PM to download this data. A sample of some of these downloaded files has been provided.

Each json file contains the following features:
`station_id`: ID of a station
`num_bikes_available`: The number of bikes currently available at the station. Where applicable, this count is further segmented into the number of mechanical bikes and the number of e-bikes available at the station.
`num_bikes_disabled`: The number of disabled bikes currently parked at the station
`num_docks_available`: The number of available docks at the station
`num_docks_disabled`: The number of disabled docks at the station
`last_reported`: UNIX timestamp (in GMT +0000 timezone) of when the data was last fetched for the station
`is_charging_station`: Values of `TRUE` or `FALSE`
`status`: Values of `IN_SERVICE`, `NOT_IN_SERVICE`, `END_OF_LIFE