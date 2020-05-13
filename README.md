# tw-interview demo
This demo project is to demonstrate how to streamline development and CI/CD with scripts. It consists of the following components:
  - static-test: this project holds all the css/js/images for the main site
  - simpleweb: this project provides a webpage that references resouces from above project
  - a virtual machine from AWS which hosts two web services: static-test and simpleweb
  - two docker images: one for java environment and webservice, one for ansible pipleline

# Requirement:
  1. docker 2.3 or above
  2. git 2.24 or above
  3. non Windows Desktop 
  
# How-to: 
  1. To start: clone this repo to your home folder, and go to *scripts*, run "*bash tw-deploy setup dev*"
        The step will down 2 docker images, the first(tomcat) will provide java environment for development; the second(ansible) is to provide ansible environment. Then it wil start a container for hosting websites and providing java environment, this container maps 8080 to 8082 on localhost. To access the website, browser to: http://localhost:8080/WebApp/. The static resources are accessible via http://localhost:8080/static/. This will also download two git repos needed for the demo in the location of ~/tw-interview.
  
  2. To build locally(for dev): run *bash tw-deploy build dev* to compile java classes, and package them as a .war, deploy them to tomcat.
  
  3. To deploy the projects to a remote server: run *bash tw-deploy simpleweb staging* 
      the tw-deploy script takes up to 3 parameters, the first is project name; the second is target hosts(test, staging, prd); the third is git repository branch, default to master. So you could run things like *bash tw-deploy simpleweb staging feature/newfeature*. There are no other branches in the demo project though. This step it will deploy the project to a remote AWS instance: 13.231.229.207, you can access them by http://13.231.229.207:8080/WebApp/ and http://13.231.229.207:8080/static/css/style.css.
  
# Things to improve
  This demo is as primitive as it could be, so there are a lot to improve, for example, dockerize remote servers, bring in docker container orchestration system like k8s, improve those scripts to be more rebust, monitoring, logging and etc.
  
  
# HA, Monitor, logging:
  As to last part of the questions about HA, monitor, logging, it depends on how the software systems are assesbled. My thoughts are
  1. traditional way: websites are hosted on barebone metal server, for HA, there are HAProxy, heatbeat, CDN services; for monitor, there are zabbix, nagios and other commercial tools. for logging, there are fluentd, elastic search, or maybe ELK.
  2. Cloud: For HA, there are load balancers, for example ALB/ELB/cloudfront from AWS; Cloud watch for monitor; centralized logging for log aggregation,analyze and display.
  3. kuberntes: k8s provides internal loadbalancing, with the use of external LB services; promethues, container advisor for monitoring; fluentd and ELK stack for logging.
