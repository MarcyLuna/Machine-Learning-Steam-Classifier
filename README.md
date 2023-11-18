# Machine Learning Steam Classifier

## What is it
We are creating a classifier that displays Steam games that contain tags most similarly found in our (or your) library of Steam games.

## Datasets
### All Games
For the general Steam library, we are using the following dataset from Kaggle.

https://www.kaggle.com/datasets/fronkongames/steam-games-dataset

There is a lot of information on the dataset here, about 38 other columns of information we did not end up needing. In order to clean it up, Zach wrote a short python script, `cleanup.py`, which parses through the json and extracts the few columns we want, and put all the wanted information in a new file, `cleaned_data.json`.

### User Games
For the user games, we got a list of gamesIDS using the Steam API for each user. The link we used is listed below, please take note of the `STEAM_ID_HERE` note.

https://api.steampowered.com/IPlayerService/GetOwnedGames/v0001/?key=108D77E9CE4D8439FDBC139442D3B25A&steamid=STEAM_ID_HERE&include_appinfo=1&include_played_free_games=1&format=json 

Using the retrieved data, we compared and matched the IDs to the same ID occurences within the `cleaned_data.json`. This lets us return a json list of games for each team member with matching information to the `cleaned_data.json` file.

## Data Cleaning
To properly clean the main game json file, you must run the json through three python scripts in this order:
1. Run the json through cleanup.py
2. Next, you can parse out all the game ids from the base API request response with gameparse.py
3. Finally,  you can get all the info for the parsed user game ids and matched them with the cleaned_data.json to get all the fields for those game ids for each user data json with compare.py

This will yield a cleaned set of game data that you can use for our model
## Preprocessing
After aquiring our datasets, its now time to preprocess them before training the model.

Our code has a function called `preprocess_data`, which takes each of our datasets as well as the cleaned_data json. This function fills NaN instances with zeros. We use pd.get_dummies to apply binary labels to the tags of the data for them to be used in classification. This function also normalizes the positive and negative review columns so a more popular game doesn't take high preference simply because it has more view.

## Model Selection
For our model, we decided to use a Random Forest Classifier for our data.

We first create a `combined_dataset` to set up the training and testing set. We use Zac's dataset as a POC but the name can be replaced by any of the datasets easily for personalized classification. We then utilize train_test_split to create our sets.

A simple imputer is applied to the `X_train` and `X_test` and a `param_grid` is set up to be used for GridSearchCV. We create the `grid_search` instance to be applied to our sets with a cv of 3, scoring based on accuracy.

We fit our training set and acquire our `best_rf_classifier` and use this to find our prediction values.

We discover our accuracy on the test set to be **0.9979301423027167**. This is astoundingly good which most likely means a slight amount of overfitting; however, this does not take away from the fact that Random Forest was perfect for this model.

Finally, we created 3 visualizations to display our work:
* The first displays our learning curve between our training score and the cross-validation score
* The second displays our ROC curve which depicts an area of .93
* The last displays our Feature Importance for each of the tags in our data_sets.
