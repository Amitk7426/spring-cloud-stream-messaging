# Building Event Driven microservices with Spring Cloud Stream and RabbitMQ

## Introduction :

Spring Cloud Stream is a framework built on top of Spring Boot and Spring Integration framework,  that helps in creating event-driven microservices at scale.
In this application I introduce a simple order processing application using the capability of Spring Cloud Stream , following the  event driven micoservice architecture. 
## Overview of the application : 
It consists of four independent microservices. 
**gateway-service**,
**order-service**,
**account-service** and 
**product-service**

Communication between **order-service** and the **customer-service** happens through orders-request and orders-reply channels .Since order-service is the source of the message ,
it sends messages to it's request channel. On the other hand, customer-service
listens for incoming messages on the request channel and publishes the order status over order-reply channel to which order-service listens to .This configuration has been provided in the application.yml of the order-service and the account-service. Also the default topic exchange in spring cloud stream has been overidden with direct exchange to implement a point to point communication .  By default spring cloud stream creates a topic exchange, which I have overridden as a direct exchange by mentioning exchangeType:direct in application.yml file.

Call to the Order-service from an external client comes through the API gateway .Order service receives the order and saves it in the repository and sends the order details message to the orders-request channel. customer-service receives the order from the orders-request channel , validates the customer id and calls the inventory-service over the rest endpoint to check the availability of the produts . If products are acustomer has sufficient balance to place the order it sets the order to ACCEPTED else sets the status to REJECTED and send this order status to the orders-in channel . Order-service receives the order status from the orders-in channel and updates the status to the client when requested over an endpoint. Following diagram shows the architecture of the application.


![order_processing](https://user-images.githubusercontent.com/9249786/52193963-c56d3a80-2877-11e9-9a82-33498f05c037.png)



##### For the newbies to Spring Cloud stream , Spring Boot and AMQP :

###### RabbitMQ with AMQP: https://www.slideshare.net/roynilanjan/the-amqp-model
###### Spring Cloud Stream: https://www.slideshare.net/roynilanjan/spring-cloud-stream-overview-128365407
###### Spring Boot: http://www.slideshare.net/roynilanjan/spring-boot-for-buidling-microservices
###### Spring Cloud Data Flow: https://www.slideshare.net/roynilanjan/building-cloud-native-data-microservice

Happy Messaging !
