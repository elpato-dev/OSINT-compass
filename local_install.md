# Local installation guide for OSINT-compass

## Setting up a postgres database and telegram bot

This step is only necessary, if you want to use the alering functionality.

We provide links to guides that explain how to do it:

- Telegram Bot: https://sendpulse.com/knowledge-base/chatbot/telegram/create-telegram-chatbot
- Postgesql database: https://1kevinson.com/how-to-create-a-postgres-database-in-docker/
 
 You also need to create the following table:
 
 ```plaintext
 CREATE TABLE alerts (
  id SERIAL PRIMARY KEY,
  term VARCHAR NOT NULL,
  scorelt NUMERIC(3,1),
  scoregt NUMERIC(3,1),
  contact_method VARCHAR,
  contact_details VARCHAR
);
 ```

## OSINT-compass-engine

1. clone the repo `git clone https://github.com/elpato-dev/OSINT-compass-engine.git` from [here](https://github.com/elpato-dev/OSINT-compass-engine)

2. install the requirements `pip install -r requirements.txt`

3. uncomment the last two lines from main.py so it looks like this:
  ```plaintext
  # Remove before deploying to render
  if __name__ == '__main__':
      app.run()
  ```
4. Copy the contents of [example_env.txt](https://github.com/elpato-dev/OSINT-compass-engine/blob/main/example_env.txt) into a `.env` file and fill in the needed information.
5. Run the app `python3 main.py` .
6. It now should be locally available on `http://127.0.0.1:5000` 

## OSINT-compass-alert-cron

1. clone the repo `git clone https://github.com/elpato-dev/OSINT-compass-alert-cron.git` from [here](https://github.com/elpato-dev/OSINT-compass-alert-cron)

2. install the requirements `pip install -r requirements.txt`
3. exchange the url of your API endpoint in line 28 in th alerter.py

4. uncomment the last two lines from api.py so it looks like this:
  ```plaintext
 #remove before deploying on render
 if __name__ == '__main__':
     app.run(port=1337)
  ```
5. Copy the contents of [example_env.txt](https://github.com/elpato-dev/OSINT-compass-engine/blob/main/example_env.txt) into a `.env` file and fill in the needed information. The API key that is used for accessing the engine API is also used for protecting the alerter API
6. Run the app `python3 main.py` .
7. It now should be locally available on `http://127.0.0.1:1337` 
8. You can trigger alerts via `http://127.0.0.1:1337/alert?apikey=<your api key>`
9. To run alerts setup something that periodically calls the alert API endpoint. (e.g. a cron job)


## OSINT-compass-portal

