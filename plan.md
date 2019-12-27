# Thought process

## Motivation
With more and more AI first companies coming up, its important for these companies to be able to conduct data science experiments in a convinent trackable fashion.
Also, following a standard way to run experiments helps the team in scaling and building components better.
This is the motivation behind [sentinels](https://matrix.fandom.com/wiki/Sentinel). The name refers to the autonomous beings from the movie matrix.


## Proposed architecture

Here is the proposed architecture : 

<img src="https://github.com/deep-learning-for-humans/sentinels/blob/master/MachineLearningExperimentsPipeline.png"/>

## Personas who use the system

Data scientist :

  * As a data scientist, I want to be able to get the status of the current jobs that are running via command line interface
  * As a data scientist, I want to be able to get the next jobs that are to be executed in the queue via command line interface
  * As a data scientist, I want to be able to push a docker container and its required configuration to the queue
  * As a data scientist, given a job ID, I should be able to get the status of the job
  * As a data scientist, I want to track all the previous experiments via ML flow server
  * As a data scientist, I want to view logs of the experiment via ELK server
  * As a data scientist, I want to track the infrastructure usage of an experiment via Prometheus dashboard
  
Data Engineer : 

  * As a data engineer, I want to be able to view all the current jobs via command line
  * As a data engineer, I want to be alerted on slack if a job complete or fails
  * As a data engineer, I want to be alerted if infrastructure resources are used up
  * As a data engineer, I should have the capability to prioritize certain experiments using configuration
 
Devops :

  * As a Devops engineer, I want to be able to setup kubernetes cluster with all the components via a single helm chart
  * As a Devops engineer, I should be able to add kubernetes nodes at will and remove them when not required
  * As a Devops engineer, I should be able to monitor and setup alerts and notifications with integrations to slack
  
  
  ## Proposed components to be built
  
  * Command line interface that can be used to push jobs, get status and manage queue by data scientists
  * Message broker that queues up all the jobs
  * Resource Monitoring system that checks resources on kubernetes
  * Job scheduler on an available node via Kubernetes jobs
  * ML Flow server, ELK server, Prometheus probing to be setup on a dedicated node 
 
 
 ## Proposed workflow 
 
 * Data scientists push dockerfile url and configuration to run the experiment to the queue via command line interface. In return, he recieves an job id.
 * The job queue APi recieves the job and adds it to the queue.
 * The resource monitor checks for available resources to schedule a job.
 * If resource available, job scheduler, schedules the job on the available nodes.
 * Track the status of the job via MLFlow, ELK and prometheus dashboards
 * Data scientist can query the status of the job via command line interface. 
 * On completion of the job, store the artifacts in storage for further perusal. 
 
 ## Assumptions on usage
1. Data is accessed via a shared store of reads, so path to the data store directory. Currently, will support only files and directories. 
2. Docker containers are present in docker registry. 

3. A user from command line would send docker container Id/ url with a path to configuration yaml and other flags. 

4. The job is queued.

5. Runs as a k8s job with resources present. 

6. All metrics, logs pushed to mlflow server and elk.

