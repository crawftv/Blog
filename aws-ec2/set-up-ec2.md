# Set Up EC2

Deploying and running the Prefect app on AWS EC2. But the principles should be universal.

## Setting up your instance <a id="setting-up-your-instance"></a>

### Step 1. Find EC2 on your AWS console <a id="step-1-find-ec2-on-your-aws-console"></a>

​![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LfHjgZuHVs1RRcggVZn%2F-LiRAElmzp09CPpyYLPp%2F-LiRAuJ24ynw-syUq-nJ%2Fec2_1.PNG?alt=media&token=534e176b-14d2-4070-8418-b74eb7e6e0e2)

### Step 2. Select an Ubuntu instance <a id="step-2-select-an-ubuntu-instance"></a>

​![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LfHjgZuHVs1RRcggVZn%2F-LiRAElmzp09CPpyYLPp%2F-LiRB6O_sIJ-kEtL8WRt%2Fec2_2.PNG?alt=media&token=5dc958b9-ead6-4181-9ada-47377a58c044)

### Step 3. Select a general purpose t2.micro <a id="step-3-select-a-general-purpose-t-2-micro"></a>

The t2.micro should be the default. It should also be the free tier.​![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LfHjgZuHVs1RRcggVZn%2F-LiRAElmzp09CPpyYLPp%2F-LiRBKmkv5AS4IsNvPaS%2Fec2_3.PNG?alt=media&token=13c7d917-8f39-4452-aa95-556eb8c3f9a7)

### Step 4. Whitelist your IP address. <a id="step-4-whitelist-your-ip-address"></a>

AWS should stop you from launching your instance before you set up your ec2 instance. Clicking on the **Source** drop down should allow you to whitelist your own IP by selecting My IP. You can also fill in custom IP's if you need to add others.​![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LfHjgZuHVs1RRcggVZn%2F-LiRAElmzp09CPpyYLPp%2F-LiRBwe_-hVTp2vAn4t6%2Fec2_4.PNG?alt=media&token=4e5eadfe-ed8b-4f7e-891b-01e1012af408)

### Step 5. Download a new or existing pair of ssh keys. <a id="step-5-download-a-new-or-existing-pair-of-ssh-keys"></a>

You can create a new pair of ssh keys or associate your instance with an existing pair of keys if you have some already.​![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LfHjgZuHVs1RRcggVZn%2F-LiROx3ym8-FOzwxlsps%2F-LiRPQm4hxK3hox2whww%2Fec2_5.PNG?alt=media&token=95a1c2dd-7ef0-4fb2-9ad8-5ebf6470e384)

## Connecting to Your Instance <a id="connecting-to-your-instance"></a>

### Optional Step. Move your new ssh key into your project directory <a id="optional-step-move-your-new-ssh-key-into-your-project-directory"></a>

This step isn't necessary but I did it so I could stay organized and make the upcoming commands easier. I used my GUI window explorer to move my keys from my downloads folder to my project repo. If you don't know what your are looking for, the file should be called _whatever-you-named-your-keys-in -step-5.pem_

### Step 7. Find your IPv4 address <a id="step-7-find-your-ipv4-address"></a>

#### Find your instance from the EC2 dashboard. <a id="find-your-instance-from-the-ec2-dashboard"></a>

​![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LfHjgZuHVs1RRcggVZn%2F-LiRNlGUJL-W6k8udgAs%2F-LiRO1tQNef7S78zhKJG%2Fec2_55.PNG?alt=media&token=20ad7bff-ded2-4c1d-85dc-9b58f397406e)

Go to your instance dashboard by clicking the link on Running Instances or Instances.

#### Find your IPv4 Address <a id="find-your-ipv4-address"></a>

​![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LfHjgZuHVs1RRcggVZn%2F-LiROOcS85d4lGu64nkZ%2F-LiRO_zMOkzt_ehpO1L1%2Fec2_6.PNG?alt=media&token=c966dbcc-5153-4d07-8cb9-93df6d05a7b2)

Your IPv4 address should be of the form xxx.xxx.xxx.xxx or 127.0.0.0. Max of 3 digits between each each dot, sometimes its just 1 or two digits.

### Step 8. Use ssh to connect to your instance <a id="step-8-use-ssh-to-connect-to-your-instance"></a>

Now you have your IPv4 address and know where your ssh\_keys are saved. Run the command below.

```text
sudo ssh - i path/to/ssh/key.pem ubuntu@xxx.xxx.xxx.xxx
```

If you are successful, your terminal should match the below picture​![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LfHjgZuHVs1RRcggVZn%2F-LiROEM1INI4GGjST-ee%2F-LiROMuOT_PrD8fbTOt-%2Fec2_8.PNG?alt=media&token=3ad3bd07-8ffc-4c79-bffe-09deb9853081)

### Step 9. Set up your project <a id="step-9-set-up-your-project"></a>

this tutorial assumes you have used github or a similar product for version control. So you can simply run git clone your\_repo.git to create your new folder.

### Step 10. Set up your environment <a id="step-10-set-up-your-environment"></a>

If you have used a pipenv environment in development of your project, you can just follow the guide in the chapter Starting with Pipenv.

### Step 11. Remember to add your .env variables <a id="step-11-remember-to-add-your-env-variables"></a>

If your project uses environment variables remember to transfer them to your new environment. I just copy and pasted them from a local directory into the ec2 terminal.

##  <a id="run-your-program"></a>

