# E-Commerce Order Processing System

This project implements an event-driven architecture for processing orders in an e-commerce platform using AWS services: SNS, SQS, Lambda, and DynamoDB.

## Setup Instructions

### 1. Create DynamoDB Table
- **Table Name**: `Orders`
- **Partition Key**: `orderId` (String)
- **Attributes**:
  - `userId`
  - `itemName`
  - `quantity`
  - `status`
  - `timestamp`

### 2. Create SNS Topic
- **Topic Name**: `OrderTopic`
- **Action**: Add the SQS queue (`OrderQueue`) as a subscription to this SNS topic.

### 3. Create SQS Queue
- **Queue Name**: `OrderQueue`
- **Configuration**:
  - Set a Dead Letter Queue (DLQ) with the `maxReceiveCount` set to 3.

### 4. Create Lambda Function
- **Purpose**: Consume messages from the `OrderQueue` and store them in DynamoDB.
- **Steps**:
  1. Create a Lambda function using the provided code (see below).
  2. Set the Lambda function to be triggered by messages from `OrderQueue`.

## Lambda Function Code
```python
# Include your Lambda function code here
