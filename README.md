# On-Boarding Project
Example assignment to demonstrate the development lifecycle of microservices

## Part 1: Creating a Project
Start by creating a basic spring boot Project that implements some simple API endpoints (https://editor.swagger.io/)
   * You can use your IDE to generate the project or use https://start.spring.io/
     * Next you will want to start adding some code!!
       * NOTE: you will want to follow a design pattern of sorts when implementing your code, for this example I used the MVC pattern
     * Don't forget to write some Tests, try using Mockito and PowerMock.
     * Introduce some Persistence Layer
     * Create a Docker image containing your new Project
### Acceptance Criteria
   * A simple spring boot application with some exposed REST Endpoints
   * The application must be able to run and maintain a healthy state
   * Code must be clean and easy to read with clear comments, and appropriate logging
   * The code must also be tested, you should be aiming for 100% coverage, or as close as possible to this
   * A Docker image must be created from your project that can be executed in any Docker VM
   * You must be able to demonstrate the Api endpoints working and behaving as you would expect

## Part 2: Adding more components to my Microservice Cluster
Similar to Step one we can repeat this process and implement another API that can add more user value to the project
 * Repeat Part 1!!
 * Now we want to start integrating our apps
Typically  in Microservices you will always introduce a single point of entry. 
e.g. if you run the two of your services in a docker vm, they both cant listen on the same port, 
this can become a headache if you have multiple services and trying to remember what port number maps to what service.
This is where we introduce a gateway:
 * As an example I used a Netflix Zuul gateway https://cloud.spring.io/spring-cloud-netflix/multi/multi__router_and_filter_zuul.html 
(there are loads more you can choose from)
 * There is lots of documentation online you can follow for tutorials on how to implement Zuul and how it works under the hood..
 * Again we can create a docker image out of this app similar to your other two apps
 * Let's add a Docker compose file and use the docker orchestration tool to start our cluster
   * Don't forget there will be additional configs to be done for this task in our spring properties
   * As well as some network configurations
### Acceptance Criteria
By the end of this stage of the assignment we would expect to see:
 * A minimum of 2 app services
 * A gateway
 * Docker-compose file that can be used to start your cluster

## Part 3: Unifying the Microservices
The beauty of microservices is the fact they are so loosely coupled.
So if I stop service A, there is no impact on service B and vice versa,
this allows us to update microservices with very limited downtime and no impact on end user experience.
* Let's add a datastore to the cluster, for this example we can use MySQL
* Create the DB schema, we can use a tool to manage the DB migrations (e.g. flyway)
* Connect your services to the new DB.
There is a lot to consider when adding a database to your application such as the type of data being processed
for e.g. is it a continous flow of data from IoT devices? Is there sensitive user data involved? Are Regular backups required? The list is endless.

Now that we have a unified datastore, a gateway which sits infront of the two services, 
it looks to the end user like one big application. But we have added a lot of configurations to each of the services to make them
'aware' of each other, a complicated process when you introduce more and more services to your cluster, this can be automated using a discovery service.
For our project we will use a Eureka server https://www.baeldung.com/spring-cloud-netflix-eureka
* Add the discovery service to your cluster
* We should see each of our services register with the Eureka service after configurations have been added
Another popular example of discovery service is consul.

### Acceptance Criteria
We should now have a fully working microservice cluster
* Service A and Service B
* Zuul gateway
* Eureka Discovery Service
* MySQL DB
* docker-compose YAML file