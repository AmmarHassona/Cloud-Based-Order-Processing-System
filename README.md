# E-Commerce Order Processing System
This project implements an event-driven architecture for processing orders in an e-commerce platform using AWS services: SNS , SQS , Lambda , and DynamoDB.

Setup Instructions
Create DynamoDB Table:

Table name: Orders

Partition Key: orderId (String)

Add the following attributes: userId , itemName , quantity , status , timestamp

Create SNS Topic:

Topic Name: OrderTopic

Add the SQS queue (OrderQueue) as a subscription to this SNS topic.

Create SQS Queue:

Queue Name: OrderQueue

Set a Dead Letter Queue (DLQ) with the maxReceiveCount set to 3.

Lambda Function:

Create a Lambda function to consume messages from the OrderQueue and store them in DynamoDB.

Use the following Lambda function code (included below).

Set the Lambda function to be triggered by messages from OrderQueue.
