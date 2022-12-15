
# Rebalancing of shared micromobility services through dynamic pricing
Prepared by Talha Alvi and Farhan Wadia

## Introduction
The purpose of this project is to show how the rebalancing problem for a shared micromobility service can be solved via a metaheuristic, bio-inspired, reinforcement learning algorithm. Specifically, publically available data from Bike Share Toronto (TBS) will be used.

The rebalancing problem refers to the supply and demand imbalances that build up throughout a day of operations temporally and spatially. At certain locations and times, user demand might be high for bikes, but not enough bikes are available at the stations to meet this demand. In a similar vein, if demand is low while supply is high, the bikes are not being optimally distributed to meet the demand. At a user's destination, this latter problem would prevent a user from finding a parking spot for the bicycle, forcing them into an inconvenient situation where they have to park elsewhere, further from their desired destination.

Two main approaches exist to solve the rebalancing problem. The first approach, which is essential to the operations of any micromobility service provider, is to have rebalancing trucks pick up bikes from certain locations with low demand to certain locations with high demand. This is typically done overnight in a fashion that meets the travel needs of the majority, but can also be done continuously over the course of the day's operations.

The second approach, which is explored for this project, is to provide users a monetary incentive to pick up bikes from, or move bikes to particular locations depending on the time of day. If done optimally, this helps lead to a win-win scenario for both the operator and the users. Assuming they find the incentive worthwhile, users get a discounted trip. For operators, they benefit from having a more balanced network which satisfies customer demand, and potentially lower costs associated with static rebalancing thanks to this.

## Dependencies List
geopandas : 0.11.1
pandas    : 1.5.0
sys       : 3.10.5 (tags/v3.10.5:f377153, Jun  6 2022, 16:14:13) [MSC v.1929 64 bit (AMD64)]
gym       : 0.21.0
numpy     : 1.23.3
matplotlib: 3.6.1
requests  : 2.28.1
plotly: 5.10.0
json  : 2.0.9

## Data
All data collected for this project can be found in the `Data` folder. A description of the dataset is provided below. An exploratory data analysis can be found in `Data_Visualization.ipynb`

### Bike Share Toronto Datasets
Bike Share Toronto has several publicly available datasets which will be required for this project. The scope of data used for this project will be limited to data from October 2022.

#### Ridership Dataset
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

#### Station Information Dataset
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

#### Station Status Dataset
Station status data can be found at [Bike Share Toronto - City of Toronto Open Data Portal](https://open.toronto.ca/dataset/bike-share-toronto/) and downloaded as a json following the [General Bikeshare Feed Specification \(GBFS\)](https://github.com/MobilityData/gbfs) format from https://tor.publicbikesystem.net/ube/gbfs/v1/en/station_status. 

The `bikeshare_station_status_downloader.py` Python script was developed by our team and has been scheduled to run indefinitely using Windows Task Scheduler every 15 minutes starting October 10, 2022 at 3 PM to download this data.

Each json file contains the following features:
`station_id`: ID of a station
`num_bikes_available`: The number of bikes currently available at the station. Where applicable, this count is further segmented into the number of mechanical bikes and the number of e-bikes available at the station.
`num_bikes_disabled`: The number of disabled bikes currently parked at the station
`num_docks_available`: The number of available docks at the station
`num_docks_disabled`: The number of disabled docks at the station
`last_reported`: UNIX timestamp (in GMT +0000 timezone) of when the data was last fetched for the station
`is_charging_station`: Values of `TRUE` or `FALSE`
`status`: Values of `IN_SERVICE`, `NOT_IN_SERVICE`, `END_OF_LIFE

## Modelling
Refer to the `Modelling` folder. The `bike_share.ipynb` file shows how the Advantage Actor Critic (A2C) and Soft Actor Critic (SAC) developed for solving this problem can be run. Note that results were initially obtained from a separate Python script that was run for over 100,000 iterations before being consolidated into this notebook. 