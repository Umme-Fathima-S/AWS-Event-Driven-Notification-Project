# AWS-Event-Driven-Notification-Project
Each time a new video/series/documentary is uploaded in S3 Storage Bucket, you receive a notification.
The S3 bucket triggers Lambda Function. 
This information is sent to SNS (Simple Notification Service).
From SNS, email notifications are sent to all the subscribers. 
The event-driven architecture leveraging S3, Lambda, and SNS provides a scalable, flexible, and reliable solution for sending automated email notifications to customers.
It allows for efficient handling of events, processing of data, and customizable notification delivery, all while benefiting from the inherent features and capabilities of AWS services.



import boto3

topic_arn = ""
def send_sns(message, subject):
    try:
        client = boto3.client("sns")
        result = client.publish(TopicArn=topic_arn, Message=message, Subject=subject)
        if result['ResponseMetadata']['HTTPStatusCode'] == 200:
            print(result)
            print("Notification send successfully..!!!")
            return True
    except Exception as e:
        print("Error occured while publish notifications and error is : ", e)
        return True

def lambda_handler(event, context):
    print("event collected is {}".format(event))
    for record in event['Records'] :
        s3_bucket = record['s3']['bucket']['name']
        print("Bucket name is {}".format(s3_bucket))
        s3_key = record['s3']['object']['key']
        print("Bucket key name is {}".format(s3_key))
        from_path = "s3://{}/{}".format(s3_bucket, s3_key)
        print("from path {}".format(from_path))
        message = "The file is uploaded at S3 bucket path {}".format(from_path)
        subject = "Processes completion Notification"
        SNSResult = send_sns(message, subject)
        if SNSResult :
            print("Notification Sent..") 
            return SNSResult
        else:
            return False
