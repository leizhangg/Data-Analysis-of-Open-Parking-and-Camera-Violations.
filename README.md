# Objective
This porject is a practice of how to connect AWS with Docker and Elasticsearch, kibana to analyze the Open Parking and Camera Violations. <br>
Data source: [NYC Open Data](https://dev.socrata.com/foundry/data.cityofnewyork.us/nc67-uf89)<br>
# Topic included:
- AWS EC2
- Elastic Search
- Elastic Kibana Visualization
- Socrata Open Data API
- Python 
- argparse libarary

Here is the info of the dataset for reviewer:
- DATASET_ID = "nc67-uf89"
- APP_TOKEN = "YLPWKFq3ghgRKnDktgcxbuWwb"
- INDEX_NAME = "opcv15"
- ES_HOST = "https://search-sta9760-t27zq5po3psy3jagr7vtqu7p2e.us-east-1.es.amazonaws.com"

# Table of Content
- [Step 1 AWS](#step-1-aws)
- [Step 2 Docker](#step-2-docker)
- [Step 3 ElasticSearch](#step-3-elasticsearch)
- [Step 4 Visualization](#step-4-visualization)
## Step 1 AWS
  ### Create EC2 on AWS
  Create EC2 following this [instructions](https://docs.google.com/document/d/13Lyd64fqevIKUnHH38-1YW_2HQdc3GZ8ovewdKdBpt0/edit#heading=h.cb4ce13d31ux)
## Step 2 Docker
  ### Create Dockefile and Docker image
  To manage and analyze big data efficiently, a containerization software [Docker](https://www.docker.com/) is used in this project. Instead of download the Docker software on local service, this project userd terminal on AWS to work with Docker.
  
  1. Start session on AWS. The session manager is located in system manager. Once the session is started, you will see a terminal in webpage.
  2. In the terminal, type "sudo su" ro change it to root user. 
  3. Type "docker" to see if it exists in the system, otherwise download it
  4. Intsall an in-browser file editor and open it with command line: 
  > #install
  > curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  > sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  > 
  > sudo apt-get update
  > 
  > apt-cache policy docker-ce
  > 
  > sudo apt-get install -y docker-ce
  > 
  > sudo systemctl status docker
  > 
  > sudo groupadd docker
  > 
  > sudo gpasswd -a $USER docker
  > 
  > newgrp docker
 
If it goes well, run thic line:
  > docker run -dv "$PWD":/srv -p 5001:80 filebrowser/filebrowser
5. After this command line runs successfully, copy the URL from EC2 in a new tab and add 5001 behind it. The URL is like this:
> http://ec2-54-226-125-75.compute-1.amazonaws.com:5001
6. If it runs correctly, it will show a login page of filebrowser. Log in with the username and password then there is a web page to create dock file sand folders
7. Create dockerfile with command line or create it through the file browser. 
8. A requirments.txt file is needed to request models. Here in this project, the requirments are:
> sodapy==2.1.0
> 
> requests==2.26.0
9. Create a Dockerfile with command line or file browser including the following contents which will tell Docker what should install and run 
> FROM python:3.9
>
> WORKDIR /app
> 
> COPY . /app
> 
> RUN pip install -r requirements.txt
> 
> ENTRYPOINT ["python","src/main.py"]
10. Finaly, create a **main.py** in a folder named as **src**

## Step 3 ElasticSearch
CRUD:
- connect ES with requests module
- create  authentication aspects 
- create index
- connect to data source and upload it to ES
- upload to ES by creating a doc
More details please view code [here](https://github.com/leizhangg/9760/blob/main/main.py)
## Step 4 Visualization 
If the code runs successfully, then the data will be uploaded to elastic search. <br>
Open the url from elasticsearch in AWS and find the index named "opcv15", it will show the properties of the dataset.<br>
The topics that I'm interested in are:
- Which county had the highest average reduction amount?
- Which violation was most popular? 
- Whch state has the most violation 
- What's the trend of the average fine, average penalty and average payment during the last 10 years
Here is my findings:
![alt](https://github.com/leizhangg/9760/blob/main/dashboard.png)
- Bronx has the highest average reduction amount
- The most popular viloation is traffic
- NY state has the most violation; NJ is the second
- The average penalty is decreased after 2015, while the average fine amount is incresed after 2015, then it decreased in 2017 and bounced back in 2020. The avearage payment amount ahs been decreased since 2013, until 2020 it incresed. <br>






  
