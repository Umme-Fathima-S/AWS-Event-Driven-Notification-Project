# AWS-Event-Driven-Notification-Project
Each time a new video/series/documentary is uploaded in S3 Storage Bucket, you receive a notification.
The S3 bucket triggers Lambda Function. 
This information is sent to SNS (Simple Notification Service).
From SNS, email notifications are sent to all the subscribers. 
The event-driven architecture leveraging S3, Lambda, and SNS provides a scalable, flexible, and reliable solution for sending automated email notifications to customers.
It allows for efficient handling of events, processing of data, and customizable notification delivery, all while benefiting from the inherent features and capabilities of AWS services.


import boto3
mysns = boto3.client("sns")

def lw(x, y):

  mysns.publish(
    TopicArn='arn:aws:sns:ap-south-1:351345331205:myeventopic',
    Message='New Video is uploaded',
    Subject='New doc is uploaded in s3 bucket',
  
  )
  print("Hey i am from AWS s3 bucket... Now I am going to Call sns...")
