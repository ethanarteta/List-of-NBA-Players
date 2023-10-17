# List-of-NBA-Players
``` python
import requests
import csv

# This python code will utilize rapid api to retrieve all NBA player information and make a csv file from the json response. 

url = "https://free-nba.p.rapidapi.com/players"

# Define the initial parameters
per_page = 100  
page = 1
all_player_data = []  

headers = {
    "X-RapidAPI-Key": "****************************************",
    "X-RapidAPI-Host": "free-nba.p.rapidapi.com"
}

while True:
    # Set the page parameter for the current page
    querystring = {"page": str(page), "per_page": str(per_page)}

    # Send a GET request to the API
    response = requests.get(url, headers=headers, params=querystring)

    if response.status_code == 200:
        data = response.json()

        # Extract player data from the 'data' list and append to the all_player_data list
        for player in data['data']:
            player_id = player['id']
            first_name = player['first_name']
            last_name = player['last_name']
            position = player['position']
            team_name = player['team']['full_name']

            all_player_data.append([player_id, first_name, last_name, position, team_name])

        # Checks to see if there are more pages to retrieve
        if page * per_page >= data['meta']['total_count']:
            break  

        # Increment the page number for the next request
        page += 1
    else:
        print(f"Failed to get data for page {page}. Status code: {response.status_code}")
        break 

# Define the CSV file name and headers
csv_filename = "nba_players_all.csv"
headers = ["Player ID", "First Name", "Last Name", "Position", "Team Name"]


with open(csv_filename, 'w', newline='') as csv_file:
    csv_writer = csv.writer(csv_file)
    csv_writer.writerow(headers)
    csv_writer.writerows(all_player_data)

print(f"Data has been successfully written to {csv_filename}")

```
