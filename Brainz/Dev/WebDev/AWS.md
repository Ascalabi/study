# AWS
Amazon Web Services offers a lot of cloud computing services. 

>**Serverless app** uses a **lambda** service that allows us that our code is excecuted on demand. We don't need to have servers running all the time even when they are not needed. And there is no need for updating servers. And we pay onl per request.
>

## Core services

> **Hosting (Serve Static App):** S3 (Simple Storage Service)
> **API (REST API):** API Gateway
> **Logic (Execute Code on Demand):** Lambda
> **Data (Store & Retrieve Data)**: [[DynamoDB]]
> **Auth:** Cognito
> **DNS:** Route 53

## Elastic Beanstalk
A simple hosting service

## Lambda
Code on demand. You create functions that can be run via API

> **Handler: **index.handler... index stands for file name and handler stand for function exports name 
```js
exports.handler = (event, context, callback) => {
    // TODO implement
    callback(null, {message: "It works?"}) //ker nimamo error damo null
};
```

Okay so you can use [aws-sdk](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/) for any lambda function (an easy way to communicate with DB). 

```typescript
const AWS = require('aws-sdk');
const dynamodb = new AWS.DynamoDB({region: '', apiVersion: ''}) //have to set the region

exports.fn = (event, context, callback) {
	console.log(event);
	const age = event.age;
	callback(null, age)
}
//Where age is an attribute of event...

//okej uno je mejbi old use this
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```

## CLI
run amplify update storage for CRUD operations