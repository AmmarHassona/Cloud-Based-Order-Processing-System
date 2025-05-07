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
import json
import boto3

def lambda_handler(event, context):
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('Orders')
    
    for record in event['Records']:
        sns_message = json.loads(record['body'])
        
        order_data = json.loads(sns_message['Message'])
        
        order_id = order_data['orderId']
        user_id = order_data['userId']
        item_name = order_data['itemName']
        quantity = order_data['quantity']
        status = order_data['status']
        timestamp = order_data['timestamp']
        
        print(f"Received order: {order_id} , {item_name} , {quantity} , {status} , {timestamp}")
        
        try:
            response = table.put_item(
                Item = {
                    'orderId' : order_id ,
                    'userId' : user_id ,
                    'itemName' : item_name ,
                    'quantity' : quantity ,
                    'status' : status ,
                    'timestamp' : timestamp
                }
            )
            print(f"Order {order_id} saved to DynamoDB.")
        except Exception as e:
            print(f"Error saving order to DynamoDB: {e}")
    
    return {
        'statusCode': 200 ,
        'body': json.dumps('Message processed successfully')
    }
