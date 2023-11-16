# Machine Learning Steam Classifier

## What is it
We are creating a classifier that displays Steam games that contain tags most similarly found in our (or your) library of Steam games.

## Datasets
### All Games
For the general Steam library, we are using the following dataset from Kaggle.

https://www.kaggle.com/datasets/fronkongames/steam-games-dataset

There is a lot of information on the dataset here, about 38 other columns of information we did not end up needing. In order to clean it up, Zach wrote a short python script, `cleanup.py`, which parses through the json and extracts the few columns we want, and put all the wanted information in a new file, `cleaned_data.json`.

### User Ganes
For the user games, we got a list of gamesIDS using the Steam API for each user. The link we used is listed below, please take note of the `STEAM_ID_HERE` note.

https://api.steampowered.com/IPlayerService/GetOwnedGames/v0001/?key=108D77E9CE4D8439FDBC139442D3B25A&steamid=STEAM_ID_HERE&include_appinfo=1&include_played_free_games=1&format=json 

Using the retirved data, we compared and matched the IDs to the same ID occurences within the `cleaned_data.json`. This lets us return a json list of games for eatch team member with matching information to the `cleaned_data.json` file.
