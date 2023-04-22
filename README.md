# OSINT Compass

## Team Members
This project was developed by [@her0marodeur](https://github.com/her0marodeur), [@elutac](https://github.com/elutac) and [@FlexusDev](https://github.com/FlexusDev). We welcome contributions from the open-source community. If you have any questions or suggestions, please feel free to open an issue or submit a pull request. Thank you for your interest in OSINT Compass!

## Tool Description
OSINT Compass is a powerful tool that allows users to collect, organize, and analyze open-source information from various sources. It provides an integrated search engine to gather relevant information, a social media monitoring interface, and the ability to store and organize data in different formats.

Moreover, OSINT Compass uses machine learning technologies to help users identify important trends and patterns in the collected information. The tool also allows users to create reports and presentations based on the collected information to gain insights into important topics or trends.

## Installation
Installation currently is not very easy, since the tool consists of multiple services deployed on render.com and docker containers. A hosted instance can be found here: https://osint-compass-portal.onrender.com

## Usage
This sections includes detailed instructions for using the tool. If the tool has a command-line interface, include common commands and arguments, and some examples of commands and a description of the expected output. If the tool has a graphical user interface or a browser interface, include screenshots and describe a common workflow.

## Additional Information
This section includes any additional information that you want to mention about the tool, including:
- Potential next steps for the tool (i.e. what you would implement if you had more time)
- Any limitations of the current implementation of the tool
- Motivation for design/architecture decisions

## Submodules
This repository includes three submodules: the `OSINT-compass-engine`, `OSINT-compass-portal` and `OSINT-compass-API-cron` modules. The `OSINT-compass-engine` submodule contains the backend of the OSINT Compass tool, which handles the data storage, processing, and analysis. The `OSINT-compass-portal` submodule provides a user-friendly interface to interact with the tool. The OSINT-compass-API-cron submodule includes Cron Jobs for alerting.

To use OSINT Compass, please clone the repository and initialize the submodules. For more information, please refer to the documentation included in each submodule.

