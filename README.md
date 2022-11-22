# Global Solution Alliance Software Interface

The Global Solutions Alliance (GSA) is an international nonprofit organization (based in the USA) composed of individual and organizational stakeholders aligned around a commitment to collective impact and creating holistic models to inspire and inform action. Through comprehensive, scientific data-driven tools collaboratively designed for use by policy makers, investors, NGOs and other on-the-ground decision-makers, change agents will be able to accelerate their work to solve global warming and secure reliable prosperity for communities in mutually beneficial relationships with the natural environment. 

The founding members of the GSA are Regenerative Intelligence PB LLC, CoLab Cooperative, Buckminster Fuller Institute, The World Fund, The Global Council for Science and the Environment, Grounded, Future Horizon/Future Quest and Sacred Headwaters of the Amazon.

The GSA originated from the recognition that cooperation in a system is integral to creating conditions for life to thrive. Rather than reinventing the wheel, which only stagnates implementation, leads to confusion, and slows progress, the GSA is an evolving ecosystem of partners that are committed to working together in mutually beneficial ways that support the emergence of regenerative economic and social systems. Addressing the global emergencies of warming, climate chaos, ecocide and species extinction requires humanity to move beyond siloed-thinking and work in-parallel towards a future that benefits all. 

Our collective work always begins with rigorous research and analysis to identify the most impactful systemic solutions benefitting people and the planet. The GSA ecosystem of organizations, researchers, and contributors work together to create and maintain a solutions-orientated knowledge commons and accompanying tools hosted on the Solutions Collaboratory (‘Collaboratory’): a free and open source digital public good designed by and for the planet’s solutionists. 

The Collaboratory serves as an open, objective, independent platform and community bringing the best available information and data to the fingertips of global change agents implementing solutions to the climate crisis, biodiversity loss, poverty and hunger, gender inequality, health and well-being, and regenerative economic growth.  

---

## The GSA Solutions Collaboratory and Repos

The Solutions Collaboratory is a digital platform designed with a multi-stakeholder, commons-based approach to knowledge and data sharing, code development, peer-to-peer collaboration, design, and end-user communications all in service to real solutions to advance a regenerative economy benefiting the planet and humanity. 

Interdependent Pillars of the Solutions Collaboratory:

- __The Global Solutions Alliance__. A membership-based, globally distributed network of independent and interconnected research associations, partners, and contributors who collaborate in building, maintaining, and evolving the Collaboratory as a living ecosystem.
The Model Solutions Engine. The science-based modeling tool itself; a techno-economic discrete-time simulation model. It facilitates the integrated calculation of 15 areas of intervention including in natural systems (agriculture, land use, oceans), food systems, energy systems, and the built environment.
- __The Software Solutions Interface__. The digital web platform that will be freely available to the public through web-based open interfaces/portals/APIs that are developed through collaborative, distributed design principles and inclusive user engagement. This will enable users to use the model at any scale of interest and experience an overview effect to understand how human civilization can reduce its impact on the planet, changing the way individuals see themselves, their actions, and their decisions, in relation to the whole.
- __The Collaboration Tool Interface__. The digital web platform that will be freely available to the public that allows individuals and teams to collaborate on developing analyses, creating modeling scenarios, sharing data, comparing results across scenarios and sharing datasets and case studies. 
- __The Knowledge Repository__. An open-source repository for data storage for all other pillars. Data here include source data (at varied geographical and local scales), model scenarios, case studies, and contributor data.

---

## Overview of the Project
Planned efforts include:

+ **Combined Model Implementation**
See [GSA Model Engine](https://github.com/Global-Solutions-Alliance/model-engine) for the core model implementation.

+ **UI aimed at researchers and broader audiences**
This repo you are in right now is a web-based user interface for working with "workbooks" of solutions, primarily for use by researchers looking to work with the model but additionally potentially of use to decisionmakers and policymakers in specific topics. This git repo is the current implementation, which requires & imports the model repo listed above as the core calculation engine. There is currently rudimentary features for sharing and collaborating on user workbooks. The UI provides controls for manipulating the values in the model, displaying graphs and output data for the resulting calculations.

---

## Getting the source code

1. Get a copy of this source code:
```sh
$ git clone https://github.com/Global-Solutions-Alliance/software-interface
$ cd software-interface
$ git checkout develop
```

## Environment Variables

### API Environment

1. Copy the example
```
$ cp service/api/env-example service/api/.env
```

2. A valid Google OAuth key will be necessary; GitHub key is optional. See [Github](https://docs.github.com/en/free-pro-team@latest/developers/apps/authorizing-oauth-apps) and [Google](https://developers.google.com/identity/protocols/oauth2/openid-connect) instructions for how to obtain these client ID and client secret keys. Update the .env file.
```
GITHUB_CLIENT_ID=somegithubclientid
GITHUB_CLIENT_SECRET=somegithubclientsecret
GOOGLE_CLIENT_ID=somegoogleclientid
GOOGLE_CLIENT_SECRET=somegoogleclientsecret
```

3. For running in production, you'll probably want a secure JWT secret key in the .env file as well:
```
JWT_SECRET_KEY=somejwtsecretkey
```

### Web Environment
No Environment configuration is needed for web

---

## Getting started with Docker (recommended)

If you have docker and docker-compose installed, you should be able to get started fairly quickly, following these steps:

```sh
$ cp docker-compose.yml.local.example docker-compose.yml
# When using Docker, the environment variable that was previously configured will need to be copied over to the `docker-compose.yaml` file.

$ docker-compose up
# our project is mounted to the container, so changes will automatically be reflected after saving. We would only need to restart the container when introducing a new external dependency. 
```
to build the docker containers. 

_NOTE: For windows machine using WSL (Windows Subsystem for Linux), we will need to enable 'WSL Integration' for required distro in Windows Docker Desktop (Settings -> Resources-> WSL Integration -> Enable integration with required distros)._


Once docker has finished building, 2 applications will be built and run under

```
web:  localhost:3000
API:  localhost:8000
```

The `web` application may take some time to load as it is being built post Docker. Check your container logs for status.


_DEVELOPER NOTE:_ If you choose to run via Docker, any changes to the library dependencies (`pip install` or `npm install`) will mean you will have to rebuild your container by restarting docker and running:

```sh
$ docker-compose build --no-cache

$ docker-compose up
```
---
## Getting started without Docker

### Building the API Service

You will need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Python 3](https://docs.python.org/3/using/index.html) (>= 3.8) installed. You will need [Postgres](https://www.postgresql.org/) and [Redis](https://redis.io/) running.

Python 3.9
```sh
$ cd service

$ pipenv shell
# Or assuming you have multiple versions installed use the following 
$ pipenv --python /Users/sam/.pyenv/versions/3.8.6/bin/python shell
# Now inside the virtual env install tools
$ pip install -r requirements.txt
# install dev dependencies
$ pip install -r requirements-dev.txt
```
### Building the Web Service
You will need [Node >= v.14](https://nodejs.org/en/) installed
```sh
$ cd web

$ npm install
```

### Database Creation
You will need to have postgres running and you will need the psql program.
```sh
$ psql -h 0.0.0.0 -p 5432 -U postgres
postgres=# CREATE DATABASE gsa-db;
```

You will need to have postgres running. You will want a valid connection string contained in `service/api/.env` for `DATABASE_URL`. Using `pipenv shell` run the following to apply existing migrations:
```sh
$ alembic upgrade head
```

to run the API:
```sh
$ cd service

$ uvicorn api.service:app --reload
```
And finally, to run the web:
```sh
$ cd web

$ npm run start
```
---
## Initializing the data

To create the default workbooks, enter `localhost:8000/initialize` in your browser. This will generate a variety of data, including the 3 Model Engine canonical workbooks. This will also load some CSVs into the database for easy retrieval, and provide the data for the `localhost:8000/resource/{path}` endpoints.

To improve performance for the app, it is recommended you run `localhost:8000/calculate` for the 3 canonical workbooks as a first step, as this will cache results for all the technologies for the workbooks. Any variation updates, when calculated, will take advantage of this initial cache as much as possible.

## Schema Updates
When changing models in `service/api/db/models.py` run the following to create migrations:
```sh
$ cd service

$ alembic revision -m "add provider column" --autogenerate
```

Note: if you are not using docker-compose, you will need to manually run:

```sh
$ cd service

$ alembic upgrade head
```

### Some gotchas

When updating variation values, you may find you need to update like this:

```
"some_solution_variable": 1234.56
```

And other times, you may find you need to update the value like this:

```
"some_different_variable": {
  "value": 1234.56,
  "statistic": "mean"
}
```

This matches the existing .json files extracted from the original excel files. At some point, we may want to normalize all values to the second option, but for now just keep in mind you will need to know when to use which. You can access the `resource/scenario/{id}` endpoint to determine which format to send.

---

## License

The python code for the collaboratory is licensed under the GNU Affero General Public license and subject to the license terms in the LICENSE file. No part of this Project, including this file, may be copied, modified, propagated, or distributed except according to the terms contained in the LICENSE file.

Data supplied (mostly in the form of CSV files) is licensed under the [CC-BY-NC-2.0](https://creativecommons.org/licenses/by-nc/2.0/) license for non-commercial use. The code for the model can be used (under the terms of the AGPL) to process whatever other data the user wishes under whatever license those data carry.

---

## Support
Please use the [Issues List](https://github.com/Global-Solutions-Alliance/software-interface/issues) to report any bugs you find, or ask any questions you have.

---

## Acknowledgements

The initial versions of these repos were ported from those of Project Drawdown, which are in the public domain.

---

## Contact

Nabil Sutjipto (nabil@colab.coop) is currently the technical point of contact for this project.


