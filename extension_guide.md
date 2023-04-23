# OSINT-compass extension guide

This page details how to integrate your own tool or API into OSINT-compass. If you have any questions or need help, please contact us. You can open a pull request with new features or ceate an issue for ideas.

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

The following image illustrates the architecture:

![oc_modules](https://github.com/elpato-dev/OSINT-compass/blob/main/images/architecture.png)

## Extending the API

The following diagram shows the integration of the modules:

![oc_modules](https://github.com/elpato-dev/OSINT-compass/blob/main/images/modules_engine.png)

The arrows show which module feeds data to which other module.

For you to implement a tool or API into OSINT-compass you should first add an API endpoint in the [main.py](https://github.com/elpato-dev/OSINT-compass-engine/blob/main/main.py).

Let's look at the email endpoint for example:

```text
@app.route('/email', methods=['GET']) #define the endpoint and method
@require_api_key # protects it with the apikey so request has to include apikey=<apikey>
def email_endpoint(): 
    email = request.args.get('email') # get the args you need
    if not email: 
        return jsonify({'error': 'email argument is required.'}), 400 # do some error handling (check if required args are there)
    result = get_email_data(email) # call the method for data collection and get result (is available bc of "from emailgetter import get_email_data" at the top)
    return jsonify(result) # returns the result you retrieved as JSON as response to the request
```

You can add your API endpoint in the same style and step one is done.

## Adding a module

Continuing with our email example let's look at the email modules implementation ([emailgetter.py](https://github.com/elpato-dev/OSINT-compass-engine/blob/main/emailgetter.py)) (The current implementation looks a bit different from this one, but as this one is easier, we explain it using this one)

```text
import requests # import needed modules

def get_email_data(email): # define method that will be imported and called in main.py
    
    # do the necessary data collection
    
    # query spycloud
    spycloud_response = requests.get("https://portal.spycloud.com/endpoint/enriched-stats/" + email)
    spycloud_data = spycloud_response.json()

    # query pingutil

    pingutil_response = requests.get('https://api.eva.pingutil.com/email?email=' + email, verify=False)
    pingutil_data = pingutil_response.json()
    
    # formatting of the data in a format that the UI can handle
    
    email_data = {
        "sources":[
            
            {
                "title": "spycloud",
                "content": spycloud_data
            },
            {
                "title": "pingutil",
                "content": pingutil_data
            }

        ]
    } 
    return email_data # returning the data (that is what the UI gets)

```
<a name="JSON"></a>

While other JSON structures can be implemented in the UI the following is the easiest way for now. 
It will create a simple card style layout in the frontend.:

```text
{
        "sources":[
            
            {
                "title": <will be title of card>,
                "content": <will be content of card>
            }, ... (how ever many cards of the format above you want)

        ]
    } 
```

Note that this card has not much formatting besides title and content, so please do not put too much stuff into the content.

And that is it, the API is now exposing your tool:

```text
https://<apiurl>/<name of your endpoint>?<query param>=<value of param>&apikey=<your api key>
```

Note: For the email and domain functionality (and any other future functionality that uses that simple card style) adding new sources is very easy. Since the UI already dynamically adds all the JSON objects from the sources list, you can just query your source in the python script that gets the data. Then you only need to add it tho the sources list as explained above and the UI will display it in that defined card format.

## Creation of simple cards (recursive display)

When the frontend application is receiving a JSON input like shown above it has the ability to loop through all the JSON objects in the sources list.
It will create a card for each JSON object. The title will be used as the title for the card. The frontend will try to unravel to JSON in `content` and find a way to display it nicely.

The following example shows how it will be done:

![card_creation](https://github.com/elpato-dev/OSINT-compass/blob/main/images/card_creation.png)

## Extending the UI


The UI can be extened in many ways. One that we want to highlight, is the very easy option to add new, simple search modules.

### Adding an endpoint

Go to the [search-page.compnent.ts](https://github.com/elpato-dev/OSINT-compass-portal/blob/main/src/app/search-page/search-page.component.ts)

#### Step 1 :
Add a string for your tool to the categories list. This will be the name of the button that will be added below the searchbar:

```plaintext
   categories: string[] = [
    "Term",
    "E-Mail",
    "Domain",
    "snscrape"
   ];
```
#### Step 2 :
Add your tool to the switch statement. Here you define the endpoint-name and how your results are displayed.

```plaintext
   switch (this.selectedCategory) {
     case "Term" : endpoint = "term"; display = "term"; break;
     case "E-Mail" : endpoint = "email"; display = "recursive"; break;
     case "Domain" : endpoint = "domain"; display = "recursive"; break;
     case "snscrape" : endpoint = "snscrape"; display = "snscrape"; break;
   }
```
#### Step 3 (optional) :

To change paramter-style of your endpoint you have to add a case to the swtich statement in the Compass-Service [compass-api.service.ts ](https://github.com/elpato-dev/OSINT-compass-portal/blob/main/src/services/compassapi/compass-api.service.ts)

```plaintext
   switch (endpoint) {
     case "snscrape" :
      url = this.baseURL + '/' + endpoint + '?term=' + term + '&entries=10&reddit=true&apikey=' + this.apikey;
      break;
    default :
      url = this.baseURL + '/' + endpoint + '?' + endpoint + '=' + term + '&apikey=' + this.apikey;
      break;
   }
```

This will result in the frontend making an API request like the following `<your api url>/<endpoint name>?<endpoint name>=<what is entered in the search field>`

Notice that the endpoint name and name of the parameter have to be the same and it currently only supports one parameter. The frontend expects a response in the JSON format shown [here](#JSON).

### Adding a display-page

There are already three display-pages implemented.
  - term
  - recursive (recommended)
  - snscrape

If you want to define your own display-page do the following.

#### Step 1:
Add a component to handel the displaying of the data.
#### Step 2:
Add the component to the [SearchComponent](https://github.com/elpato-dev/OSINT-compass-portal/blob/main/src/app/search-page/search-page.component.ts)'s [html](https://github.com/elpato-dev/OSINT-compass-portal/blob/main/src/app/search-page/search-page.component.html). 
#### Step 3:
ResultData can be passed via import. Look into [SnscrapeDisplayComponent](https://github.com/elpato-dev/OSINT-compass-portal/blob/main/src/app/result-page/snscrape-display/) for a implemented example.
