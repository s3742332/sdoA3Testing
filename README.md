# Assignment 3 - s3742332 - Harris Charalambous
Stonks INC is looking forward to deploying their containerised application onto Kubernetes, and use Circle CI to package and deploy the application.
This will involve:
1. Generate an artifact that may be deployed
2. Deploying a Kubernetes cluster
3. Configure Circle CI variables
4. Create HELM Chart
5. Deploy Application

# Step 1
First, set your AWS credentials. Then, from the root directory, run the 'make bootstrap' command, notice how several resources have random strings attached (kops_state, tfrmstate, terraform_statelock). After the command finishes running, go into the AWS console, then the EC2 dashboard and log down what the random strings are for each variable. Finally, go into the config.yml in the .circleci folder and update the three variables with the correct random string (lines 34 + 109), aswell as line 8 in the infra makefile.

# Step 2
Run the following commands from the root directory:
1. make kube-create-cluster
2. make kube-secret
3. make kube-deploy-cluster
4. make kube-config

Next go into the AWS VPC dashboard and get the vpc_id and subnet ids that have been created, and insert into the terraform.tfvars file in the infra folder as specified via comments. Then from the infra folder run:
1. make init ENV=test
2. make up ENV=test

# Step 3
Add the project to Circle CI, then enter the project settings and go to the environment variables tab, and add in your AWS credentials.

# Step 4
A Helm chart with the name todo has been created with a service and deployment manifest. The service is a type Loadbalancer, and the deployment takes in dummy variables for the image and database endpoint (these are parsed in during the next step).

# Step 5
A testing environment has been established, and the application is configured to deploy onto it (see config.yml -> deploy_test job). This will establish the docker image to run on, initialise and apply the infra, and output the db_endpoint.Before deploying, make sure a namespace called 'test' has been created (can run 'make namespace-up' from root). From here, the artifact that has been generated is upgraded (updated) and the variables for image and endpoint are set. Finally, after everything is finished, the smoke-test step is run to validate if there has been a successful deployment. To get the link to access the application, run 'kubectl get services -n test' from the root folder and copy down the EXTERNAL-IP, this will be the link to access.






# Simple Todo App with MongoDB, Express.js and Node.js
The ToDo app uses the following technologies and javascript libraries:
* MongoDB
* Express.js
* Node.js
* express-handlebars
* method-override
* connect-flash
* express-session
* mongoose
* bcryptjs
* passport
* docker & docker-compose

## What are the features?
You can register with your email address, and you can create ToDo items. You can list ToDos, edit and delete them. 

# How to use
First install the depdencies by running the following from the root directory:
```
npm install --prefix src/
```

To run this application locally you need to have an insatnce of MongoDB running. A docker-compose file has been provided in the root director that will run an insatnce of MongoDB in docker. TO start the MongoDB from the root direction run the following command:

```
docker-compose up -d
```

Then to start the application issue the following command from the root directory:
```
npm run start --prefix src/
```

The application can then be accessed through the browser of your choise on the following:

```
localhost:5000
```
## Container
A Dockerfile has been provided for the application if you wish to run it in docker. To build the image, issue the following commands:

```
cd src/
docker build . -t todoapp:latest
```

## Terraform

### Bootstrap
A set of bootstrap templates have been provided that will provision a DynamoDB Table, S3 Bucket & Option Group for DocumentDB & ECR in AWS. To set these up, ensure your AWS Programmatic credentials are set in your console and execute the following command from the root directory

```
make bootstrap
```

### To instantiate and destroy your TF Infra:

To instantiate your infra in AWS, ensure your AWS Programattic credentials are set and execute the following command from the root infra directory:

```
make up -e ENV=<environment_name>
```

Where environment_name is the name of the environment that you wish to manage.

To destroy the infra already deployed in AWS, ensure your AWS Programattic credentials are set and execute the following command from the root directory:

```
make down -e ENV=<environment_name>
```

## Testing

Basic testing has been included as part of this application. This includes unit testing (Models Only), Integration Testing & E2E Testing.

### Linting:
Basic Linting is performed across the code base. To run linting, execute the following commands from the root directory:

```
npm run test-lint --prefix src/
```

### Unit Testing
Unit Tetsing is performed on the models for each object stored in MongoDB, they will vdaliate the model and ensure that required data is entered. To execute unit testing execute the following commands from the root directory:

```
npm run test-unit --prefix src/
```

### Integration Testing
Integration testing is included to ensure the applicaiton can talk to the MongoDB Backend and create a user, redirect to the correct page, login as a user and register a new task. 

Note: MongoDB needs to be running locally for testing to work (This can be done by spinning up the mongodb docker container).

To perform integration testing execute the following commands from the root directory:

```
npm run test-integration --prefix src/
```


###### This project is licensed under the MIT Open Source License
