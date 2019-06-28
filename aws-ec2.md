---
description: >-
  Deploying and running the Prefect app on AWS EC2. But the principles should be
  universal.
---

# AWS EC2

## Setting up your instance

#### Step 1. Find EC2 on your AWS console

![](.gitbook/assets/ec2_1.PNG)

#### Step 2. Select an Ubuntu instance

![](.gitbook/assets/ec2_2.PNG)

#### Step 3. Select a general purpose t2.micro

The t2.micro should be the default. It should also be the free tier.

![](.gitbook/assets/ec2_3.PNG)

#### Step 4. Whitelist your IP address.

AWS should stop you from launching your instance before you set up your ec2 instance. Clicking on the **Source** drop down should allow you to whitelist your own IP by selecting My IP. You can also fill in custom IP's if you need to add others. 

![](.gitbook/assets/ec2_4.PNG)

