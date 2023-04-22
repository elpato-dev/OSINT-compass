#OSINT-compass extension guide

This page details how to integrate your own tool or API into OSINT-compass. If you have any questions or need help, please contact us. You can open a pull request with new festures or ceate an issue for ideas.

##General design

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

![oc_architecture]([http://url/to/img.png](https://github.com/elpato-dev/OSINT-compass/blob/main/images/architecture.PNG))
