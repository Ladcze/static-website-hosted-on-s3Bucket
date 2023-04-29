# static-website-hosted-on-AWS

---

#### Introduction
Consider this is a simple project (hosting a static website) on AWS. This is a fairly simple and straight forward project which would take about 15mins to implement. 

---

#### Services deployed
These are the 2 primary services required for this project

![](Route53.png)         
![](S3.png)


---

#### Steps (in summary)
- Create a custom domain name using Route 53 
- Create an S3 Bucket
- Upload a sample website (.html file) 
- Configure static website hosting connected to the domain name
 - Update DNS record. 

---

* Create/Register a custom domain name
Log  onto the AWS console.
From the list of services, search for & navigate to Route 53. 
Using the Register domain menu, enter a name to see if this available. Proceed to checkout if name is available and complete the order. 

* Create S3 Bucket 
From the AWS console, search for/navigate to S3
Create a new bucket (using same name as with your domain). 
Select preferred/closer region (to reduce latency)
Keep defaults object ownership but uncheck  “block all public access”. This allows users to access your bucket and publicly accessible to others via the internet.
Keep other settings – click create button (to save entry)

* Upload a sample website (.html file) using Amazon S3 Bucket
Using the upload meu/feature, import your file(s) onto the bucket
![](S3-BucketProperties.png)
Make necessary adjustments within the properties and permissions tab
 ![](S3-BucketPropertiesPermissions.png)

 
* Enable static website hosting - This ensures service-users will have the permissions to access the content of this bucket. 
Go to properties tab to enable static website (scroll to the bottom)
![](staticWebsiteHosting.png)
At the index section, type in file name of html uploaded. Click save changes
* Re permissions tab – attach a bucket policy
Next, go to permissions tab. Add a bucket policy. This is a resource-based policy used to grant access permissions to your bucket and the objects in it. This can be created using the template option on the console. 
![](bucketPolicy.png)

* Bucket policy sample:  
{  
    "Version": "2012-10-17",  
    "Statement": [  
        {  
            "Sid": "PublicReadGetObject",  
            "Effect": "Allow",  
            "Principal": "*",  
            "Action": "s3:GetObject",  
            "Resource": "arn:aws:s3:::domainName.com/*"  
        }  
    ]  
}  


* Create/Update a DNS record
The website is now hosted on Amazon S3 but the object URL (https://s3.eu-west-2.amazonaws.com/ladcze.click/index.html) isn’t reflecting the custom URL/domain name (ladcze.click) as indicated below. 
![](staticWeb-Pre-DNS-Update.png)
A record is required to redirect ladcze.click to the S3 bucket. 
From the AWS console, go to Route53. 
Select Hosted zones
![](hostedZones.png)
Select the relevant domain name – click create record 
using the wizard, select  “simple routing” policy – Define simple record   
Value: Alias to S3 website endpoint  
Choose region – (as with hosted bucket)  
Select the s3 endpoint  
![](simpleRecord.png)  
Disable evaluate target health, and Confirm by clicking define simple record.  
<br/> Changes should be live within a few minutes. Navigating to ladcze.click will return the same website with correct custom domain name thereafter. 
![](staticWeb-Post-DNS-Update.png)

---

#### Summary
We created a S3 Bucket with the contents of our (static) website has been uploaded/hosted. The S3 bucket is connected to a custom domain name so that when visitors navigate via a browser to the custom domain name, they can view the website.  
