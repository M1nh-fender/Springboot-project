Spring Cloud - a project under spring eco system, spring data spring secutiy
- concentrates on microservices.  gives common set of design patterns

What are different services will build
- product service: create view products, a product catalog
- order service: can order products
- inventory service: it is where order service can check if product in stock or not before submitting order
- notification service: send notifications after order is placed

Order, inventory, and notification will interact

**Synchrounous and Asynchrnous communication in place**
- Synchronous communication is like a phone call. You call someone and wait for them to answer before continuing. Example: A service makes an HTTP request to another service and waits for a response.

- Asynchronous communication is like texting. You send a message and continue with your day, and the other person replies when they can. Example: A service sends a message to a queue (like Kafka or RabbitMQ), and another service processes it later.

- Synchronous is simpler but can slow things down if a service takes too long to respond. Asynchronous is faster and more reliable but more complex to set up.


---

Product service - talks to mongodb
order service - talks to my sql
inventory - talks to mysql to store inventory database
notification service

but mainly a microservice is services talking to each other, so order talks to inventory service (order checks if inventory is available with synchronous notification) and then an asynchronous notification with the notification service if order successful

asynchronous communcation will use kafka

surrounding services to enable microservice 
- api gateway 
	- help sends request from user to diff services
	- ex: user want to talk to product service, dont want to give host name or ip address of product service to talk to it, so it gateway is like a gateway
- auth server with key cloak
- grafana for observability
- grfana loki to aggregate logs and view
- grafana tempo for distrubted tracing
- prometheus to collect metrics
- grafana to visualize metrics
- containerize all services with docker and migrate to kubernetes

---

architecture for each service (below are all layers)
- controller
- service
- repository

- http request from client goes into controller, 
- from there **business logic** is on service layer and some services may have a message queue
- store information in database via repository layer 


**Business logic** is the "rules" of how a program should work.
For example, in an online store:
- You **can’t buy** something if it’s out of stock.
- A **discount** is applied if you have a coupon.
- Your **order is confirmed** only after payment is successful.
It’s the part of the code that makes decisions based on the business needs.