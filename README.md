# Geolocation

## Overview
This script fetches geolocation data for a given IP address and generates an interactive map displaying the location. The map is saved as an HTML file.

## Requirements
- Python 
- `requests` library for HTTP requests
- `folium` library for generating maps

## Script Explanation
### Functions

#### `get_geolocation(ip_address)`
This function takes an IP address as input and returns the geolocation data.
```python
def get_geolocation(ip_address):
    try:
        response = requests.get(f'http://ip-api.com/json/{ip_address}')
        data = response.json()
        if data['status'] == 'success':
            return data
        else:
            print(f"Error fetching data: {data['message']}")
            return None
    except Exception as e:
        print(f"An error occurred: {e}")
        return None
```
- Sends a GET request to the `ip-api.com` service.
- Parses the JSON response.
- Returns geolocation data if the request is successful.
- Handles errors by printing a message and returning `None`.

#### `create_map(latitude, longitude, location_name)`
This function generates a map with a marker at the specified latitude and longitude.
```python
def create_map(latitude, longitude, location_name):
    location_map = folium.Map(location=[latitude, longitude], zoom_start=12)
    folium.Marker([latitude, longitude], popup=location_name).add_to(location_map)
    return location_map
```
- Creates a `folium` map centered at the given latitude and longitude.
- Adds a marker with a popup showing the location name.

### Main Function

#### `main()`
The main function coordinates input, geolocation retrieval, and map creation.
```python
def main():
    ip_address = input("Enter the IP address: ")
    geolocation_data = get_geolocation(ip_address)
    if geolocation_data:
        latitude = geolocation_data['lat']
        longitude = geolocation_data['lon']
        location_name = geolocation_data['city']
        location_map = create_map(latitude, longitude, location_name)
        location_map.save("geolocation_map.html")
        print("Map has been saved as 'geolocation_map.html'")
    else:
        print("Could not retrieve geolocation data.")
```
- Prompts the user for an IP address.
- Retrieves geolocation data using `get_geolocation`.
- If data is retrieved successfully, extracts latitude, longitude, and city name.
- Creates a map using `create_map`.
- Saves the map as `geolocation_map.html`.
- Prints a success or failure message.

### Execution
The script executes the `main` function when run directly.
```python
if __name__ == "__main__":
    main()


## Usage
1. Run the script:
   ```bash
   python geolocation_map.py
   ```
2. Enter an IP address when prompted.
3. The script will generate and save a map as `geolocation_map.html`.

The HTML file can be opened in any web browser to view the interactive map.
