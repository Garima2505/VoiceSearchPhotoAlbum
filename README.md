Implemented a photo album web application, that can be searched using natural language through both text and voice. Created an intelligent search layer to query your photos for people, objects, actions, landmarks and more.

### AWS Services Used
S3, Lex, ElasticSearch, Rekognition, Lambda function, CodePipeline,  CloudFormation and API Gateway

### Architecture
![image1](/Images/architecture.jpg)

### Implementation

1. Launch an ElasticSearch instance1
  1.1 Using AWS ElasticSearch service , create a new domain called “photos”.

2. Upload & index photos
  * Create a S3 bucket (B2) to store the photos.
  * Create a Lambda function (LF1) called “index-photos”.
  * Set up a PUT event trigger on the photos S3 bucket (B2), such that whenever a photo gets uploaded to the bucket, it triggers the Lambda function (LF1) to index it.
  * Implement the indexing Lambda function (LF1):
    - Given a S3 PUT event (E1) detect labels in the image, using Rekognition (“detectLabels” method). 
    - Store a JSON object in an ElasticSearch index (“photos”) that references the S3 object from the PUT event (E1) and an array of string labels, one for each label detected by Rekognition.

Use the following schema for the JSON object: <br>
  &nbsp;{ <br>
   &nbsp; &nbsp; “objectKey”: “my-photo.jpg”, <br>
   &nbsp; &nbsp; “bucket”: “my-photo-bucket”, <br>
   &nbsp; &nbsp; “createdTimestamp”: “2018-11-05T12:40:02”,<br>
   &nbsp;&nbsp;&nbsp; “labels”: [<br>
   &nbsp;&nbsp;&nbsp;&nbsp;  “person”, <br>
   &nbsp;&nbsp;&nbsp;&nbsp;  “dog”, <br>
   &nbsp;&nbsp;&nbsp;&nbsp; “ball”, <br>
   &nbsp;&nbsp;&nbsp;&nbsp;  “park” <br>
   &nbsp;&nbsp;&nbsp;         ] <br>
  &nbsp; } <br>

