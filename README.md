# 30 Days DevOps Challenge - Game Day Notification
**Project Overview:** This project implements event-driven architecture for an alert system that sends real-time NBA game day score notifications to subscribed users via SMS/Email. It utilizes the following AWS services and tools:
- **Amazon Simple Notification Service (SNS):** For sending SMS and email notifications.
- **AWS Lambda:** To process and handle API data and notifications.
- **Amazon EventBridge:** For scheduling automation tasks.
- **NBA APIs:** For fetching live game scores.
![overview](images/overview.png)
The project demonstrates cloud computing principles and efficient notification mechanisms.

---

## Features
1. Fetches live NBA game scores using an external API.
2. Sends formatted score updates to subscribers via SMS/Email using Amazon SNS.
3. Scheduled automation for regular updates using Amazon EventBridge.
4. Designed with security in mind, following the principle of least privilege for IAM roles.

---

## Step-by-Step Procedure
1. **Setup SportsData.io:**
   - Create a free account at [sportsdata.io](https://sportsdata.io).
   - Obtain your NBA API key.

2. **Configure AWS Services:**
   - Create an SNS Topic
     1. Navigate to the **SNS Console** on AWS.
     2. Click on **Create topic** and choose **Standard**.
     3. Provide a name for your topic, e.g., `gd_sns_topic` as shown below.
        ![create_sns_topic](images/sns_topic.png)
     4. Note the **ARN (Amazon Resource Name)** in the image below and click **Create subscription**.
        ![create_sns_subscription](images/sns_subscribe.png)
     5. Add subscriber (SMS or Email):
        - For SMS, provide a phone number.
        - For Email, provide an email address and confirm the subscription.
        ![add_sns_subscriber](images/sns_subscriber.png)
             
   - Create an IAM Policies:
     1. Navigate to the **IAM Console** and click on **Policies**, then click **Create policies**.
        ![create_IAM_policies](images/iam_policies_1.png)
     2. Select **SNS** as a service
        ![select_service](images/iam_policies_2.png)
     3. Switch from **visual** to **JSON**, to modify configurations as shown below
        ![cofigure_service](images/iam_policies_3.png)
        
   - Create an IAM Role for Lambda:
     1. Navigate to the **IAM Console** and click on **Roles**, then click **Create role**.
        ![create_IAM_role](images/iam_role_1.png)
     2. Select **Lambda** as a **service/use case** as shown below:
        ![create_IAM_role](images/iam_role_2.png)
     3. Add the following permissions as shown below:
        - `gd_sns_policy`
        ![add_policy](images/iam_role_3.png)
        - `AWSLambdaBasicExecutionRole`
        ![add_policy](images/iam_role_4.png)
     4. Assign a name to the role, e.g., `gd-lambda-role`.
        ![finalise_policy](images/iam_role_5.png)

   - Create the Lambda Function:
     1. Open the **Lambda Console** and click **Create Function**.
     2. Choose **Author from scratch** and name the function, e.g., `gd-notifications`.
        ![create_lambda_funtion](images/lambda_func_1.png)
     3. Assign the previously created IAM role to the function (`gd-lambda-Role`).

   - Add the Python Code:
     1. Copy the sample Lambda function from `src/gd-notification.py` to the aws code editor
        ![lambda_code](images/lambda_func_2.png)
     2. Click the **deploy** button
     4. Navigate to the **configuration** tab and click **Environment Variables**, then click **Edit** to add Environment Variables:
        ![add_env](images/lambda_func_3.png)
     6. Add the two Environment Variables as shown below:
        - `NBA_API_KEY`: Your API key from SportsData.io.
        - `SNS_TOPIC_ARN`: The ARN of your SNS topic.
        ![add_env](images/lambda_func_4.png)
     7. Test the function:
        ![test_function](images/lambda_func_5.png)
        - If you encounter a **timeout error**, go to the **general configuration** and edit the timeout.
          ![timeout_edit](images/timeout_edit.png)
        - Increase the timeout to **15 seconds**.
          ![timeout_update](images/timeout_update.png)

   - Set up an EventBridge Rule:
     1. Go to the **EventBridge Console** and create a rule.
        ![create_event_bridge](images/eventbridge_1.png)
     2. Assing a name to eventbridge `gd_rule` and select **schedule** as shown below:
        ![set_name](images/eventbridge_2.png)
     3. Configure **schedule pattern** and set a **cron expression** (e.g., every 15 minutes)
        ![set_cron_job](images/eventbridge_3.png)
     4. Set the target as the Lambda function you create `gd-notification` as shown below:
        ![set_target_function](images/eventbridge_4.png)
     5. Click next and keep the other setting as the default
        ![proceed](images/eventbridge_5.png)
     8. ![proceed](images/eventbridge_6.png)

---

## Testing
Confirm that the EventBridge triggers the Lambda function as scheduled.
![confirm_triggers](images/email_triggers.png)

---

## Conclusion
This project demonstrates my hands-on approach to delivering an event-driven architecture using AWS services and APIs. By following this guide, you can set up and customize the system to meet your requirements.
