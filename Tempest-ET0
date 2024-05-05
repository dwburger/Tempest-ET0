import requests
import time
from datetime import datetime
import schedule

def fetch_tempest_data(api_key, device_id, retries=3, delay=10):
    """
    Fetches data from the Tempest Weatherflow API with retries.

    Args:
    - api_key (str): API key for authorization.
    - device_id (int/str): Device ID for the data source.
    - retries (int): Number of retry attempts.
    - delay (int): Delay between retries in seconds.

    Returns:
    - dict/None: The API response as a dictionary if successful, None otherwise.
    """
    base_url = "https://swd.weatherflow.com/swd/rest/observations/device/"
    headers = {
        "Authorization": f"Bearer {api_key}",
        "Accept": "application/json"
    }

    for attempt in range(retries):
        response = requests.get(base_url + str(device_id), headers=headers)
        if response.status_code == 200:
            return response.json()
        else:
            print(f"Attempt {attempt + 1} failed with status code: {response.status_code}")
            time.sleep(delay)  # Wait for 'delay' seconds before retrying

    print("All attempts failed.")
    return None

def calculate_et0(data):
    obs_data = data['obs'][0]
    air_temp = obs_data[7]
    wind_speed = obs_data[2]
    rel_humidity = obs_data[8]

    # Check if solar radiation data is available; use 0 if None
    solar_radiation_raw = obs_data[11]
    solar_radiation = solar_radiation_raw * 0.0864 if solar_radiation_raw is not None else 0  # MJ/m^2/day conversion

    ALBEDO = 0.23
    G = 0.0
    PSY_CONST = 0.665e-3
    es = 0.6108 * (10 ** ((7.5 * air_temp) / (237.3 + air_temp)))
    ea = es * (rel_humidity / 100.0)
    delta = (4098.0 * es) / ((air_temp + 237.3) ** 2)
    Rn = (1 - ALBEDO) * solar_radiation - 0.34 * (1.35 * solar_radiation / 2.3 - 1.0)
    et0_daily = (0.408 * delta * (Rn - G) + PSY_CONST * (900 / (air_temp + 273)) * wind_speed * (es - ea)) / (delta + PSY_CONST * (1 + 0.34 * wind_speed))
    
    et0_hourly = et0_daily / 24  # Daily to hourly ET0
    et0_in_inches = et0_hourly / 25.4  # Convert mm to inches
    return et0_in_inches

def job():
    api_key = "YOUR API KEY GOES HERE"  # NOTE: BETWEEN QUOTATION MARKS
    device_id = YOUR DEVICE ID GOES HERE # NOTE: NO QUOTATION MARKS
    data = fetch_tempest_data(api_key, device_id)
    if data:
        et0 = calculate_et0(data)
        current_time = datetime.now().strftime("%m/%d/%Y %H:%M:%S")
        print(f"Current time: {current_time}, Hourly ET0 = {et0:.2f} inches/day")

print("ET0 data fetching script started.")
schedule.every().hour.at(":00").do(job)

while True:
    schedule.run_pending()
    time.sleep(1)
