----------------------------------------------------------

docker-compose up --build
http://localhost

----------------------------------------------------------

Travis + AWS Elastic Beanstalk deployment

A Dockerrun.aws.json file is an Elastic Beanstalk–specific JSON file that describes 
how to deploy a set of Docker containers as an Elastic Beanstalk application. 
You can use a Dockerrun.aws.json file for a multi-container Docker environment.

----------------------------------------------------------

Travis:

1. Specify DOCKER_ID, DOCKER_PASSWORD

----------------------------------------------------------

AWS:

https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html#container_definitions

1. Elastic Beanstalk
    - create a new application
    - create a new environment (web server)
    - platform - multi-container docker, sample application

2. Inspect network settings
    - VPC
    - Security Groups

3. Set up Amazon Elastic Cache (Redis)
    - Redis, create
    - set name (multi-docker-redis), node type (t2-micro), number of replicas 0
    - create a new subnet group - name: redis-group, select all subnets

4. Set up Amazon Relational Database Service (Postgres)
    - create database - Postgres
    - enter DB instance type, username and password
    - specify database name, eg. multi-docker-postgres

5. Security group
    - create security group, name eg. multi-docker
    - edit inbound rule, port range 5432-6379, source - sg-00cf3dd80df70b9eb group id of our security group
    - apply to Redis - select, modify, VPC security groups
    - apply to Postgres - select, modify, security groups
    - apply to Elastic Beanstalk - configuration, modify instances

6. Enter params
    - Elastic Beanstalk - configuration, software
    - REDIS_HOST: multi-docker-redis.we2agv.0001.usw2.cache.amazonaws.com
    - REDIS_PORT: 6379
    - PGUSER: postgres
    - PGPASSWORD: postgrespassword
    - PGHOST: multi-docker-database.czrznal1t4e8.us-west-2.rds.amazonaws.com
    - PGPORT: 5432
    - PGDATABASE: postgres

7. IAM 
    - add user: amw061-complex-app-deployer (programmatic access)
    - attach existing policy
    - select all with Elastic Beanstalk
    - note: access key ID, secret access key

8. Travis
    - set AWS_ACCESS_KEY and AWS_SECRET_KEY

----------------------------------------------------------
