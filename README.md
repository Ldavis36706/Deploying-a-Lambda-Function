# Deploying-a-Lambda-Function
I created an AWS Lambda function and configured an event notification for Amazon Simple Storage Service (Amazon S3).


<h2>Description</h2>
I created an AWS Lambda function that resizes an uploaded image to create a thumbnail. I configured an event notification in Amazon S3 to trigger the Lambda function whenever an image is uploaded. The function processes the original image, resizes it into a thumbnail, and saves it in a target S3 bucket. Finally, I monitored the Lambda function's performance and logged events using Amazon CloudWatch.<br />

<h2>Languages and Utilities Used</h2>

- <b> Amazon S3: </b> Amazon S3 is an object storage service that offers industry-leading scalability, data availability, security, and performance. It stores and protect any amount of data for a range of use cases, such as data lakes, websites, built-for-the-cloud applications, backups, archives, machine learning, and analytics.
- <b> IAM: </b> AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. With IAM, you can centrally manage permissions that control which AWS resources users can access. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources.
- <b>AWS Lambda: </b> AWS Lambda is a compute service that lets you run code without provisioning or managing servers. Lambda runs your code on a high availability compute infrastructure and performs the administration of the compute resources, including server and operating system maintenance, capacity provisioning and automatic scaling, and logging. With Lambda, all you need to do is supply your code in one of the language runtimes that Lambda supports.
- <b>AWS CloudWatch: </b> AWS CloudWatch is a monitoring and observability service provided by AWS that allows you to monitor your AWS resources and the applications you run on AWS in real time. It collects and tracks metrics, which are variables you can measure for your resources and applications, providing system-wide visibility into resource utilization, application performance, and operational health.

<h2>Environments Used </h2>

 <b>Cloud Platform:
AWS (Amazon Web Services): This project was conducted entirely in AWS, utilizing several of its services.
</b> 
<h2>Program walk-through:</h2>

<p align="center">

The following diagram depicts the basic architecture of application flow. 

 <br/>
 
![Figure 1](https://i.imgur.com/u9r445z.png)

<i>Figure 1: The diagram illustrates a user uploading an object to the source bucket in Amazon S3 (object-created event). Amazon S3 detects the object-created event and publishes the event to AWS Lambda by invoking the Lambda function and passing event data as a function parameter. AWS Lambda runs the Lambda function. From the event data it receives, the Lambda function knows the source bucket name and object key name. The Lambda function reads the object and creates a thumbnail using graphics libraries, then saves the thumbnail to the target bucket..</i>
<br />
<br />
<br />


My first step was creating two Amazon Simple Storage Service buckets. The first bucket was my source bucket. This bucket is where the original photo is storage. The second bucket is my target bucket where the original photo will be converted to a thumbnail. One challenge that I faced with creating my Amazon S3 three bucket is naming the bucket. The first name I chose was already in use. The buckets must contain a globally unique name. To overcome this challenge I added some numbers to the original name I selected to achieve cardinality. My goal was to have a very unique name.</b>.

![Verify Apache Server Working](https://i.imgur.com/T0KWtBc.png)

<br />

<br />

<br />



The next step is to create the role that will be attached to the Lambda function. When creating an IAM role, it's essential to attach the appropriate permissions. I always follow the <b> <i>  principle of least privilege,</b></i> granting only the permissions necessary to perform specific tasks. You can either use AWS-managed policies or create custom policies tailored to your needs.

![IAM Role](https://i.imgur.com/MLazWTL.png)



<br />

To automate image processing, a Lambda function is configured with an Amazon S3 trigger, enabling it to execute whenever a new object is uploaded to the source S3 bucket. AWS Lambda is a compute service that lets you run code without provisioning or managing servers. The steps to create a Lambda function are listed below:

<ol>
  <li><b>Open the AWS Management Console</b></li>
  <li><b>Search for Lambda and select it</b></li>
  <li><b>Click create function</b></li>                 
 <li><b>Select Author from scratch</b></b></li>
  <li><b>In the Basic information section, configure the following:</b><br /> Function name: Create-Thumbnail <br /> Runtime: Select Python 3.9 from the available options.<br /> Expand Change default execution role and choose: Execution role: <br />Use an existing role Existing role: lambda-execution-role (Grants permission to read and write images in S3 buckets.</li>
  <li><b>Click create function</b></li></li>
</ol>
<br />
Once the function is successfully created, a "Successfully created the function" message appears at the top of the page.

<br />

![Lambda Function](https://i.imgur.com/65OVM6k.png)



<br />

I set up an Amazon S3 trigger to automatically invoke the Lambda function whenever an image is uploaded to the source S3 bucket. The Lambda function then processes the image. This kind of automation is essential in cloud computing for efficient workflows. The steps to create an S3 trigger function are listed below:

<ol>
  <li><b>Go to the Create-Thumbnail Lambda function page</b></li>
  <li><b>In the Function overview section, click Add trigger</b></li>
  <li><b>Click create function</b></li>                 
  <li><b>In the Trigger configuration section, configure the following settings:</b><br /> Source: Select S3 <br />Bucket: Choose the source bucket.<br /> Acknowledge the Recursive invocation warning </li>
  <li><b>Click Add</b></li></li>
</ol>
<br />
Once the trigger is successfully added, a "Trigger was successfully added to function" message appears at the top of the page. 

<br />
<br />

![Adding S3 Trigger](https://i.imgur.com/DY0b8z3.png)



<br />
<br />

To enable image processing, I configured the Lambda function by uploading the <strong> CreateThumbnail.zip </strong> deployment package in the Code tab of the Lambda console. Please refer to the <strong> CreateThumbnail.zip </strong> file attached to this repository.
The function performs several tasks: receiving an event with the image name, downloading the image, resizing it using the Pillow library, and uploading the resized image to the target S3 bucket.

<br />
<br />

![Instance Inbound Rules](https://github.com/user-attachments/assets/b47ef338-c2e4-45ba-91b6-75e97eb0e990)


<br />

I added an inbound rule to allow all traffic from any IP address.
![Updated inbound rules](https://github.com/user-attachments/assets/0e8f4572-969a-4639-bbe2-6a8d6c3bc345)



<br />

I reloaded the webpage after my configuration changes. It was successful this time.
![Successfully loaded webpage](https://github.com/user-attachments/assets/0947084f-eea7-4eee-939e-ad14f1f82cc2)


<br />

I moved the existing index.html file to a backup location. To do this: Return to the AWS Cloud9 Bash terminal, and run the following command: <b> sudo mv /var/www/html/index.html /var/tmp/</b>.
I tried to reload the webpage after making these changes, but the page would not load.
![No files in Root location](https://github.com/user-attachments/assets/92144ab3-8f4b-4fc0-a23a-547ecd74b64a)

<i>Figure 2: This message is displayed because there are currently no files in the DocumentRoot location.</i>


<br />

I configured the AWS Cloud9 IDE environment window to show the web server document root. To do so, in the AWS Cloud9 Bash terminal, run the following commands: <b> ln -s /var/www/ /home/ubuntu/environment/www
sudo chown -R ubuntu /var/www/</b>.  The first command created a symlink to the /var/www directory in your environment directory. The second command changed ownership of the files in the /var/www directory so the ubuntu user (which is you) can edit the files in this www directory. If you want, you can verify the user by running whoami.
![Configure root directory](https://github.com/user-attachments/assets/a2bd39a4-98ff-488f-807f-0b75c87e2a58)


<br />

I began creating a <span zeum4c20="PR_1_0" data-ddnwab="PR_1_0" aria-invalid="grammar" class="Lm ng">webpage</span> for "Anycompany Bicycle parts". I was so proud of myself for getting the header to display that I deviated from the project. Here is a temporary message where I'm asking for a job.
![Successful webpage](https://github.com/user-attachments/assets/576b59f4-0434-4933-a737-fcd9fbcef71a)


Now it's time to get back on track. I added some heading and paragraph elements. I was able to add hyperlinks to jump to different sections in my webpage. Additionally, I was able to open links in a different <span zeum4c20="PR_2_0" data-ddnwab="PR_2_0" aria-invalid="spelling" class="LI ng">browswer</span> from my webpage. I added a target attribute and gave it the value of "_blank." I added an HTML entity as well. I figured out how to include the copyright symbol. It can be achieved by typing: <b><p>&copy; </b>. Below you will see a snippet of my HTML code.
![HTML code](https://github.com/user-attachments/assets/be5b5706-5f3a-43f1-a119-17471606231e)


<br />

Finally I completed my webpage. It turned out better than I expected.
![Completed Webpage](https://github.com/user-attachments/assets/de1a58fb-1875-431d-a32f-35b14e18a029)

<br/>

