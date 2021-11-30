# My Approach

## Architecture Proposal
### Front end
React/Angular JS or someother front end technology to serve the UI for the user with below features.  This can also be scaled based on the load.
 - Login (oAuth integration or basic LDAP)
 - Buttons to upload the files
 - Buttons to Add/Delete the data
 - Buttons to search for the data
 - Place to display the result or message

### Backend
Python FastAPI or Node technologies can be leveraged here to serve the APIs that connects to backend DB instance.  This can be scaled based on the load and query processing time that each instance spent.
- Methods that allow GET and POST
- POST Method to Upload the files
- POST Method to add the data
- DELETE Method to delete a record
- GET Method to read the query param and query the data and return the results
- GET Method for a status page that verifies the health of the application

The code needs to connect to backend DB and proces the requests as instructured.

### Database
Since we are dealing with structured data here, I recommend to to use any Relational DB like sqlserver, mysql, Azure Tables etc.


## Legacy Implementation
This approach uses indiviaul VM and can be implemented with all open source software except the VM cost.
![image](https://user-images.githubusercontent.com/86123183/144100504-9871e151-04c7-41b4-afc5-34634c0eb677.png)
### Infra Setup
Terraform script that connects to specific cloud or VCenter that spin up the required VM.  The statefile can be stored in a remote backend like AWS S3 or Azure Storage Container

### Configuration
Ansible with avaialble galaxy roles or write new custom roles to configure the VMs to bring them in a desired state.

### CI/CD
A Multibranch pipeline is created to build and deploy the change upon every commit (ofcourse after passing the tests)

:NOTE: SSL Cert and Domain name can be considered for prod implementation.

### Conclusion:
PROS:
 - Easy to setup and maintain
 - Less price

CONS:
 - Scaling becomes challenging (unless we are ok with auto scaling policies)
 - Traditional approach
 - Need to arrange monitoring
 - Need to duplicate the environment for testing
 - Application may expect downtime during upgrade


## Latest Implementation (Kubernetes)
![image](https://user-images.githubusercontent.com/86123183/144100416-303ea534-2b49-40cd-947c-39823092b4c1.png)
Assuming that the company already have EKS/AKS/Rancher is setup

### Infra Setup
N/A

### Configuration
N/A

### CI/CD
Once the Jenkins job has successfully created the artifact(or output file), a Dockerfile with the required packages/software can build the image with the latest artifact and stores it in a private registry.  With kubectl command we can then rollout the deployments using Blue Green approach.

### Conclusion:
PROS:
 - Easy to manage the deployments
 - No need to get new cert or domain if company has a wild card certificate like *.mypass.company.com
 - Lower environment can be built easily
 - No downtime to applcation
 - Rollback becomes easy, just change the docker tag

CONS:
 - Kubernetes Setup is costly


## Another approach with pure cloud
Azure App Service that can serve as a Front END
Azure Function App with HttpTrigger --> to process API requests (costs us only when used)
Azure Backend DB.

If we want to protect the front end, we can take advantage of Azure FrontDoor for WAF

This approach is less pricy and avoid maintainenance of infra.
