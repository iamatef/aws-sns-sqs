# SNS to SQS Demo for AWS Cloud9 and Node.js 
This is a demo of how to use SNS to send messages to SQS. the app.js code when copied and run on cloud9 will create the queue, send a message to the topic and then receive the message from the queue. You should change the topic ARN to match the topic you created in the AWS console.

## setup
1. Create a topic in SNS and note the topic's ARN
2. Create a queue in SQS and note the queue's URL [if run the app.js code, it will create the queue for you]
3. Subscribe the queue to the topic by visiting the topic's page in the AWS console and clicking the "Create subscription" button. Select the queue you created in step 2 and click "Create subscription".
4. Modify the queue access policy to allow SNS topic to send Messages to the queue, add the correct queue ARN and topic ARN to the policy below and apply it to the queue. The policy should look something like this: 
````
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "sns.amazonaws.com"
      },
      "Action": "sqs:SendMessage",
      "Resource": "arn:aws:sqs:us-east-1:692779810082:backspace-lab",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:sns:us-east-1:692779810082:test-topic"
        }
      }
    }
  ]
}
````
5. Publish a message to the topic by visiting the topic's page in the AWS console and clicking the "Publish message" button. Enter a message and click "Publish message".
6. Verify that the message was received by the queue by visiting the queue's page in the AWS console and clicking the "Receive messages" button. You should see the message you published in step 5.
7. to create a new message programmatically in Node.js, use the following code:
````
var AWS = require('aws-sdk');
var sns = new AWS.SNS();
var params = {
  Message: 'Hello from SNS to SQS!', /* required */
  TopicArn: 'arn:aws:sns:us-east-1:692779810082:test-topic'
};
sns.publish(params, function(err, data) {
  if (err) console.log(err, err.stack); // an error occurred
  else     console.log(data);           // successful response
});
````
8. Code in app.js creates the queue and reads the message from the queue. it has a create messages function but is commented out so it will just read and process the message you created in step 5 after automatically sent to the queue by SNS.
