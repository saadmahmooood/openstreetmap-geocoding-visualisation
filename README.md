# OpenStreetMap Geocoding and Visualization

## Overview

This project uses the free **OpenStreetMap Nominatim API** to convert university names entered by users into geographical locations (latitude and longitude) and displays these locations on an OpenStreetMap map.

The processed data is stored in an SQLite database (`opengeo.sqlite`), which can be used for further analysis or visualizations. The project is divided into two main phases: geocoding and visualization.

## Features

- **Geocoding**: Convert university names into geographical coordinates (latitude and longitude) using OpenStreetMap’s Nominatim API.
- **Database**: Store geocoded data in an SQLite database for future use.
- **Visualization**: Visualize the locations on OpenStreetMap using JavaScript (via OpenLayers).

## Installation

1. **Install Dependencies**  
   You'll need Python 3 and the following Python libraries:
   - `requests` (for making API requests)
   - `sqlite3` (for working with SQLite databases)

   Install dependencies using pip:
   ```bash
   pip install requests sqlite3
   ```

2. **SQLite Browser**  
   To view and modify the SQLite database, you can use [DB Browser for SQLite](https://sqlitebrowser.org/).

## Getting Started

### Phase 1: Geocoding University Names

To start the process of geocoding university names, use the `geoload.py` script. This script will read the input file `where.data`, check if each university already exists in the database, and, if not, send a request to the Nominatim API to retrieve the coordinates and store them in the `opengeo.sqlite` file.

1. **Run `geoload.py`**  
   ```bash
   python3 geoload.py
   ```

   If the data is already in the database, the program will skip querying the API for those locations. If a location is not in the database, it will make an API request and save the result.

   Example output:
   ```bash
   Found in database AGH University of Science and Technology
   Found in database Academy of Fine Arts Warsaw
   Retrieving https://py4e-data.dr-chuck.net/opengeo?q=BITS+Pilani
   Retrieved 794 characters ...
   ```

2. **Database Reset**  
   If you want to start over and re-fetch the data, simply delete the `opengeo.sqlite` file:
   - On **Mac**: `rm opengeo.sqlite`
   - On **Windows**: `del opengeo.sqlite`

### Phase 2: Visualizing the Data

Once the data has been loaded into the database, you can visualize it by using the `geodump.py` script. This script reads from `opengeo.sqlite` and generates a JavaScript file (`where.js`) containing the geocoded data.

1. **Run `geodump.py`**  
   ```bash
   python3 geodump.py
   ```

   Example output:
   ```bash
   AGH University of Science and Technology, Kraków, Poland 50.065703299999996 19.918958667058632
   Academy of Fine Arts, Warsaw, Poland 52.2397515 21.015564130658333
   ...
   260 lines were written to where.js
   ```

2. **Visualize in Browser**  
   Open the `where.html` file in your browser to see the locations plotted on the map. Each location is shown as a pin, and you can hover over or click on each pin to see more details about the location.

   Example data format in `where.js`:
   ```javascript
   myData = [
     [50.065703299999996, 19.918958667058632, 'AGH University of Science and Technology, Kraków, Poland'],
     [52.2397515, 21.015564130658333, 'Academy of Fine Arts, Warsaw, Poland'],
     ...
   ];
   ```

### Note on Nominatim API Usage

- The Nominatim API has a usage limit of one request per second. If you make too many requests in a short time, your IP may be blocked.
- The program handles this limitation by querying the API only if the location is not already in the database.

### Troubleshooting

- **No data in map**: Ensure that JavaScript is enabled in your browser and check the browser’s developer console for errors.
- **Slow geocoding**: The process might take time, especially if querying many locations. You can control the number of requests by modifying the script to set a limit (`count` variable) on the number of queries.
