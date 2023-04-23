# Local installation guide for OSINT-compass

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
