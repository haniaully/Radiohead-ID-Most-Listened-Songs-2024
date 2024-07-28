# Radiohead-ID-Most-Listened-Songs-2024
✿ most listened Radiohead songs in Spotify as of July 2024! :)

✿ this repository contains a visualization of the most listened songs by Radiohead in Indonesia as of the year 2024. the purpose for all of these are for my personal-learning only.

## visualization

![Visualization](https://github.com/haniaully/Radiohead-ID-Most-Listened-Songs-2024/blob/4faf7a3fd9f248d1bab7e2ae53ce52ff38c4e7a0/Radiohead%20ID%202024.png)

## prerequisites

- Python 3.x
- pandas library
- requests library
- Spotify Developer Account
- Tableau Public
  
## steps to recreate this project
  
### step 1: Spotify API access

#### a. create a Spotify Developer account
1. go to the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard/login).
2. i'm creating a new account.
3. create a new app:
   - click on "Create an App".
   - fill in the required information (App Name, App Description, etc.).
   - agree to the terms and click "Create".

#### b. get Spotify API credentials
1. once your app is created, note down your _`Client ID`_ and _`Client Secret`_.

#### c. obtain an access token
1. use the following Python script to obtain an access token:

   ```python
   import requests
   import base64

   client_id = 'your_client_id'
   client_secret = 'your_client_secret'

   auth_url = 'https://accounts.spotify.com/api/token'
   auth_headers = {
       'Authorization': 'Basic ' + base64.b64encode((client_id + ':' + client_secret).encode('utf-8')).decode('utf-8'),
   }
   auth_data = {
       'grant_type': 'client_credentials',
   }

   response = requests.post(auth_url, headers=auth_headers, data=auth_data)
   access_token = response.json()['access_token']
   print('Access Token:', access_token)

2. replace _'your_client_id'_ and _'your_client_secret'_ with your actual Spotify API credentials.
3. save the file with a .py extension. for example: get_spotify_token.py.
4. run the script by selecting Run > Run Module. --- END-TO-END, I'M USING TERMINAL. :)
5. copy the **access token** from the output.

### step 2: fetch data from Spotify API for Indonesian market

#### a. get Radiohead's Spotify ID
1. create a new Python script and copy the following code:
   ```python
   import requests

   # replace with your actual access token
   access_token = 'your_access_token'

   # get Radiohead's ID
   search_url = 'https://api.spotify.com/v1/search'
   search_headers = {
      'Authorization': 'Bearer ' + access_token,
   }
   search_params = {
      'q': 'Radiohead',
      'type': 'artist',
   }

   response = requests.get(search_url, headers=search_headers, params=search_params)
   radiohead_id = response.json()['artists']['items'][0]['id']
   print('Radiohead ID:', radiohead_id)

2. replace _'your_access_token'_ with the token obtained earlier.
3. save the file with a .py extension. for example: get_radiohead_id.py.
4. run the script by selecting Run > Run Module.
5. note down the **radiohead_id** from the output.

#### a. get Radiohead's Spotify ID
1. get top tracks in Indonesia
   ```python
   import requests
   import pandas as pd

   # replace with your actual access token
   access_token = 'your_access_token'
   radiohead_id = 'your_radiohead_id'

   # get top tracks in Indonesia
   tracks_url = f'https://api.spotify.com/v1/artists/{radiohead_id}/top-tracks'
   tracks_headers = {
      'Authorization': 'Bearer ' + access_token,
   }
   tracks_params = {
      'market': 'ID',  # Specify Indonesia market
   }

   response = requests.get(tracks_url, headers=tracks_headers, params=tracks_params)
   tracks_data = response.json()['tracks']

   # extract relevant data
   tracks_list = []
   for track in tracks_data:
      track_info = {
          'name': track['name'],
          'popularity': track['popularity'],
          'release_date': track['album']['release_date']
      }
      tracks_list.append(track_info)

   # create a DataFrame and save as CSV
   tracks_df = pd.DataFrame(tracks_list)
   tracks_df.to_csv('radiohead_tracks_indonesia.csv', index=False)
   print("Data saved to 'radiohead_tracks_indonesia.csv'")

2. replace _'your_access_token'_ and _'your_radiohead_id'_ with your access token and the ID obtained earlier.
3. save the file with a .py extension. for example: get_radiohead_tracks.py.
4. run the script by selecting Run > Run Module.
5. verify the CSV file radiohead_tracks_indonesia.csv is created in the directory where you saved the script.

### step 3: Tableau visualization
1. open Tableau Public.
2. import the CSV file:
     - go to File > Open and select the radiohead_tracks_indonesia.csv file.

## Recommendations

for those looking to explore Radiohead's music further, here are some other great songs by the band:

- **Black Star**
- **Nude**
- **Let Down**
- **Paranoid Android**

## Data Source

the data for this project is sourced from Spotify's API. it includes information on each song's popularity, album, release date, and more.
