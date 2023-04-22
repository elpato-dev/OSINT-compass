# OSINT-compass extension guide

This page details how to integrate your own tool or API into OSINT-compass. If you have any questions or need help, please contact us. You can open a pull request with new festures or ceate an issue for ideas.

## General design

The tool consists of the following parts: 
- an engine (exposes the functionality via API and retrieves the data from tools, APIs and other soures)
  - written in Python/Flask
- a web interface (allows the user to interact with all the tools in a GUI)
  - written in Angular
- a database for storing the alerts
 - currently Postgresql
- an alert cron job
 - has to be configured to run periodically to check the database for alerts and send alerts out, when the criteria is met
 - written in Python

The following image tries to illustrate the architecture

![oc_modules](https://github.com/elpato-dev/OSINT-compass/blob/main/images/architecture.PNG)

## Extending the API

First here is another beautiful image of how our modules are integrated with eachother:

![oc_modules](https://github.com/elpato-dev/OSINT-compass/blob/main/images/modules_engine.PNG)

The arrows show which module feeds data to which other module.

For you to implement a tool or API into OSINT-compass you should first add an API endpoint in the main.py.

Let's look at the email endpoint for example:

```text
@app.route('/email', methods=['GET']) #define the endpoint and method
@require_api_key # protects it with the apikey so request has to include apikey=<apikey
def email_endpoint(): 
    email = request.args.get('email') # get the args you need
    if not email: 
        return jsonify({'error': 'email argument is required.'}), 400 # do some error handling (check if required args are there)
    result = get_email_data(email) # call the method for data collection and get result (is available bc of "from emailgetter import get_email_data" at the top)
    return jsonify(result) # returns the result you retrieved as JSON as response to the request
```




