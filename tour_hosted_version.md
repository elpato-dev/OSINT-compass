# OSINT-compass-portal hosted version tour

The hosted version can be found on: https://osint-compass-portal.onrender.com/ .

## The start page

The start page provides access to all the search functionalities of OSINT-compass.
The following search features are implemented in the hosted version:

### Term search

The term search functionality lets you see recent news and tweets for the term you enter as well as related links from wikipedia.
Furthermore it provides basic sentiment analysis:

![image](https://user-images.githubusercontent.com/101996103/233848009-e065d2e1-57e8-49b7-b378-ffa3ccadb782.png)

### E-Mail search

The email search functionality lets you see basic data for an email address.

- Pingutil: displays basic info about the email
- Spycloud: provides basic breach occurence data of the provided email address and its domain

![image](https://user-images.githubusercontent.com/101996103/233847949-ab9ad650-5abe-48e5-acbd-a0189d933d2f.png)


### Domain search

The domain search functionality provides basic information on a domain:
- If it has a robots.txt it will be displayed
- If the URL was indexed by waybackmachine you can click on the latest index of it

![image](https://user-images.githubusercontent.com/101996103/233848064-5a17cad7-97ec-4b2b-a749-1b235c6e2e0f.png)

### snscrape
This functionality shows that other tools can be integrated in our tool. Currently only the reddit functionality of snscrape is available, since many other functionalities are currently broken.

The snscrape functionality provides reddit posts for the term you specify:
- information about the post
- post content
- sentiment analysis

![image](https://user-images.githubusercontent.com/101996103/233848102-9c828f92-5058-4c2e-aa38-d1334ba603c2.png)


## The alerting functionality 

When you press the "ALERT ME" button in the top right corner the alert dialog will open.
Currently alerting only supports telegram.

The alerts will be send out every hour at xx:18 but you can trigger them manually here https://osint-compass-alerter.onrender.com/alert?apikey=mysuperkey .

You can only alert for lesser than or greater than or both but currently the API needs input for both. So please input 1 for greater than, if you only want an alert for lesser than. Also input -1 for lesser than, if you want to only alert for greater than. In the other field input the desired value. Keep in mind that realistic ranges are between -0.3 and 0.3.

**IMPORTANT: To receive alerts on telegram please start a chat with @osintcompassbot on Telegram.**

![image](https://user-images.githubusercontent.com/101996103/233840173-5b79e9f8-8f9f-425e-a04d-f0b972a0de21.png)


You will get an alert like the following:

![image](https://user-images.githubusercontent.com/101996103/233841791-ab9ce64b-2f9f-439a-a814-2fc01d44a105.png)

