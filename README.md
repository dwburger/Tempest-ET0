# Tempest-ET0

This Python script calculates ET0 (Reference ET) using weather data from a Tempest weather station.  Data from the Tempest includes:

1. Air Temperature (air_temp)
2. Wind Speed (wind_speed)
3. Relative Humidity (rel_humidity)
4. Solar Radiation (solar_radiation)

are used to calculate ET0 using the Penman-Monteith equation.  The Penman-Monteith model is particularly effective because it encompasses both the physiological and physical aspects of evaporation and transpiration, providing a comprehensive and theoretically sound estimate of ET0. It's commonly used in irrigation planning and agricultural applications to estimate the amount of water needed by crops, reflecting real environmental conditions.

**Script Overview**

This Python script is designed to interact with the Weatherflow Tempest Weather Station API to fetch real-time environmental data and calculate the reference evapotranspiration (ET0) using the Penman-Monteith equation. The script is structured to perform several key tasks, including data fetching, processing, and scheduled execution for regular updates.

**Main Functionalities**

**Data Fetching:**

The script uses the fetch_tempest_data function to retrieve live weather data from the Tempest Weather Station via its API. This function makes HTTP GET requests to the API endpoint, passing in necessary authentication headers and handling the response.
It checks the response status and, upon a successful request, returns the JSON data. If the request fails, it attempts retries a specified number of times before giving up, providing robustness against temporary network issues or API downtimes.

**ET0 Calculation:**

Utilizing the meteorological data retrieved from the API, the script calculates the reference evapotranspiration using the Penman-Monteith formula, widely recognized for its accuracy in various climatic conditions.
The calculation takes into account several environmental factors such as air temperature, relative humidity, wind speed, and solar radiation, all of which affect the rate of evapotranspiration.

**Data Handling and Storage:**

The ET0 values calculated hourly are stored in a CSV file for record-keeping and further analysis. This file logs the date, time, and ET0 for each hour, allowing for a detailed time series analysis of evapotranspiration trends.
Additionally, the script manages another text file for logging daily precipitation data fetched from the API, keeping a rolling record of the last seven days' precipitation values in a structured list format.

**Scheduled Execution:**

The script employs the schedule library to automate the execution of its main tasks. The ET0 calculation is set to run at the top of each hour, ensuring that the data used is as current as possible.
The precipitation data fetching is scheduled to occur daily at 01:00 AM, capturing the total precipitation from the previous day.

**Error Handling and Logging**

The script includes basic error handling mechanisms for API communication issues and file operations, ensuring that failures in data fetching or file writing do not crash the program but instead are handled gracefully, with error messages logged for debugging.

**Usage Scenario**

This script is particularly useful for agricultural or environmental monitoring applications where precise and timely data on weather conditions and water requirements are crucial for decision-making processes. It automates the regular fetching and calculation of critical weather data, facilitating efficient resource management based on up-to-date environmental insights.

**Conclusion**

Overall, this Python script represents a practical tool for interfacing with weather data APIs and conducting essential environmental calculations automatically. Its structured approach to error handling, data processing, and scheduling makes it a robust solution for ongoing weather data monitoring and analysis.
