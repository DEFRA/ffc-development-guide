# Troubleshooting Guide
A troubleshooting guide for common problems faced by developers on the FFC programme

## Service bus

#### Application logs message or service timeout

What this looks like
+ In the application logs, `data: {"message": "Unable to create the amqp session due to operation timeout."}` 
+ `ServiceBusError: Unable to create the amqp session due to operation timeout at ... code: 'ServiceTimeout'`
<br>

What this means
+ The application is unable to send a message to its requested topic due to network or configuration issues
<br>

What is the solution
+ Ensure the machine is connected to the Defra network (a VPN provides access to users off-site)
+ Check the environmental topic variables are correct for the service (these are usually stored locally within the user's profile file)
+ (Stop)[https://docs.docker.com/engine/reference/commandline/stop/] and (prune all Docker containers, networks and volumes)[https://docs.docker.com/engine/reference/commandline/system_prune/]
+ Use this (testing framework)[https://github.com/johnwatson484/azure-service-bus-test-client] to check outbound requests independently of the application
<br>

#### Request does not reach the Azure topic

What this looks like
+ On the Azure portal, the requests chart for the topic has no activity once a request has been sent
<br>

What this means
+ The request is not being received within the Azure topic
<br>

What is the solution
+ Ensure you are looking at the correct Azure topic within the correct subscription
+ Check that the request does not (timeout)[#application-logs-message-or-service-timeout]
+ If the application is web-based, try again as webpack was possibly running
+ If the application is web-based, clear the web-browser's cache or use a (private/incongito window)[https://www.computerworld.com/article/3356840/how-to-go-incognito-in-chrome-firefox-safari-and-edge.html]
+ Check (Azure platform's health)[https://status.azure.com/en-gb/status]
<br>

#### Messages in a topic's Service Bus Subscription(s) ae not processed

What this looks like
+ On the Azure portal, the message count for the topic's Service Bus subcription(s) increase once a request has been sent
<br>

What this means
+ The messages received from the Azure topic are pushed to the Service Bus subscription(s) but not being processed
<br>

What is the solution
+ Check a subcription is setup for the topic, Azure automatically deletes idle subscriptions after 14 days unless specified to never do so during creation
+ Usually, another microservice is responsible for ingesting and processing these messages, ensure it is up
+ Check the environmental subscription variables are correct for the ingesting service (these are usually stored locally within the repository)
<br>

## Docker

#### Tests cannot access database migration data

What this looks like
+ `await db.tblName.findAll()` returns nothing for a table with insert data in a changelog file(s)
+ `await db.tblName.create(tblRecord).catch(err => { console.log(err) })` returns `relation "public.tblName" does not exist`
<br>

What this means
+ The application is attempting to read from an empty database table
<br>

What is the solution
+ Check the `docker-compose.migrate.yaml` file was ran 
+ Confirm the database migration was successful
+ If using (`docker compose`)[https://docs.docker.com/compose/cli-command/], ensure the database volume is mapped correctly
+ If using (`docker compose`)[https://docs.docker.com/compose/cli-command/], ensure Docker Desktop is configured correctly for V2 by checking/unchecking the `Use Docker Compose V2` in Docker Desktop's preferences
