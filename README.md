# OSINT Compass

## Team Members
This project was developed by [@her0marodeur](https://github.com/her0marodeur), [@elutac](https://github.com/elutac) and [@FlexusDev](https://github.com/FlexusDev). We welcome contributions from the open-source community. If you have any questions or suggestions, please feel free to open an issue or submit a pull request. Thank you for your interest in OSINT Compass!

## Tool Description
OSINT Compass is a tool that allows users to collect, monitor, and analyze open-source information from various sources. It provides an integrated search engine to gather relevant information and a sentiment monitoring interface.

Its main purpose is to allow organizations to provide an easy to use, easy to extend, graphical tool to researchers. If you want you can also selfhost it (not straight forward yet).

 We would absolutely love, if you would integrate your own tools or APIs into OSINT-compass. <3 

## Usage
The README of the repository [OSINT-compass-engine](https://github.com/elpato-dev/OSINT-compass-engine/blob/main/README.md) has a detailed documentation of the API and its endpoints.

We hope the web interface on https://osint-compass-portal.onrender.com is self explanatory but we will provide some guidelines on how to use it.

### Start Page

The start page provides a search bar with three buttons below:
- term: just enter a term like `brazil` or `america president` (can be without the quotes)
  - results:
    - recent news from news API, with an overall sentiment score between -1(negative) and 1(positve)
      - real range of sentiment score is probably between -0.3 and 0.03
    - recent tweets from Twitter API, with an overall sentiment score between -1(negative) and 1(positve)
      - real range of sentiment score is probably between -0.3 and 0.03
      - tweets are not filtered by relevance and may include offensive content
    - links to wikipedia articles related to the term
    - a frequency count of words used in the news and tweets (extend the card by clicking on it)
- email: enter an email address in the format `email@domain.com`
     - get results form different sources
     - currently pingutils and spycloud since they need no API key and have no rate limiting
     - can be easily extended with other sources like emailrep and HaveIBeenPwned
- domain: enter a domain in the format `domain.com`
     - get results form different sources
     - currently checks for robots.txt, subdomains and if it is listed on the waybackmachine
     - can be easily extended with other sources

## Extending the tool

The tool was designed with extensibility in mind. Check out the guide on how to extend it [here](https://github.com/elpato-dev/OSINT-compass/blob/main/extension_guide.md).

## Installation
Installation currently is not very easy, since the tool consists of multiple services deployed on render.com and docker containers. A hosted instance can be found here: https://osint-compass-portal.onrender.com

Basically what you would do is take the submodules and deploy them to render.com. OSINT-compass-engine as a web service and and OSINT-compass-portal as a static website. 

For alerting you need to deploy a Postgresql datbase on render. Additionally you need something that can run cron jobs (on render they are paid only) and run the OSINT-compass-alert-cron in what ever intervall you want it to send out alerts. 

To simplify this process in the future we plan on releasing a Docker image, that combines all the services.

## Additional Information
Future ideas:
- implement a single Docker image to make self hosting easier
- add more tools and make the current ones more extensive
- timelines of sentiment score

Limitations:
- Self hosting the tool is possible but was not documented during the bellingcat hackathon, since it is currently quite some effort.
- I would suspect the tool to have at least some security vulnerabilities. We tried to do our best, but please do not provide very sensitive information to it.
- The sentiment score currently seems to have a low range, so do not really expect to see one above 0.3 or below 0.3 . 
- The tweets are currently not filtered by relevance so you might see some offensive material... sorry...

Archictecture principles:
- The tool aims to be very modular, to allow anyone to integrate a GUI for their tool or API into OSINT-compass.
- If you are only after data collection automation you could also use the API as a standalone service.

## Submodules
This repository includes three submodules: the `OSINT-compass-engine`, `OSINT-compass-portal` and `OSINT-compass-API-cron` modules. The `OSINT-compass-engine` submodule contains the backend of the OSINT Compass tool, which handles the data storage, processing, and analysis. The `OSINT-compass-portal` submodule provides a user-friendly interface to interact with the tool. The OSINT-compass-API-cron submodule includes Cron Jobs for alerting.
