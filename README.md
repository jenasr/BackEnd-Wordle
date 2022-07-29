# Back-End Wordle Clone
## Authors:
* Joseph Nasr
* Mohammad Qazi
* Kevin Sierras
* Samuel Valls
* Kevin Sarmiento

## Description:
This is the Back-End version of Wordle. It utilizes 5 different microservices m1, m2, m3, m4 and m5. These are implemented using FastAPI and pydantic

* Microservice 1 serves as word validation. It operates on the word list database 'words.db' and has 3 endpoints:
  1. Check for valid word in the word list
  2. Add a possible guess to the word list
  3. Delete a possible guess from the word list

* Microservice 2 is a answer checking service. It queries the answer database 'answers.db'  It has 2 endpoints:
  1. Check to see if a guess is correct, if not, return the color values of each letter
  2. Change the word associated with a given game

* Microservice 3 tracks user wins and losses. It employs the database shards of 'stats.db' called 'stats1.db', 'stats2.db', 'stats3.db', and 'user_profiles.db', It has 5 endpoints:
  1. Record a game win or loss
  2. Initialize values for a new game
  3. Retrieve the stats of a user
  4. Get the top 10 users by number of wins
  5. Get the top 10 users by streak

* Microservice 4 tracks game state through a key-value store created using Redis. It has 3 endpoints:
  1. Store a new game a user has begun to play
  2. Update the game a user is playing
  3. Fetch a game the user has already begun to play

* Microservice 5 is the Wordle game. It manages gameplay by making http calls, through httpx in python, to the microservices mentioned above. It has 2 endpoints:
  1. Starts a new game for the player
  2. Handles the the guess of a player


## Databases (Executables located in DB/):
Initialize database: execute create_words_db.py, create_answer_db.py, and create_stats_db.sh in the terminal

* Giver Execute permission: chmod u+x create_words_db.py
* Execute: ./create_words_db.py

Create shards folder: create empty folder named "Shards" in folder "DB"

Create shards: execute create_shards.py in the terminal

## Starting services:
### Standalone:
* Microservice 1:
  * uvicorn m1:app --reload

* Microservice 2:
  * uvicorn m2:app --reload

* Microservice 3:
  * uvicorn m3:app --reload

* Microservice 4:
  * uvicorn m4:app --reload

* Microservice 5:
  * uvicorn m5:app --reload

#### Using Microservice 5 in terminal:
foreman start

### Steps For Testing Microservice 5:
Step 1: Go to docs page for link given after executing microservice 5

ex: http://127.0.0.1:5400/docs

Step 2: Insert a username registered on Microservice 3's streaks for first endpoint

ex: ucohen

Step 3: Copy game_id and user_id from the JSON response of first endpoint

ex:

(game_id) 2068

(user_id) 123e4567-e89b-12d3-a456-426614174000

Step 4: Insert game_id and user_id with a 5 letter word guess for the game into second endpoint

(game_id) 2068

(user_id) 123e4567-e89b-12d3-a456-426614174000

(guess) break

Step 5: Give 5 more guesses with the same game_id and user_id with a 5 letter word guess to get win or loss

ex:

brine

fling

great

trade

learn
