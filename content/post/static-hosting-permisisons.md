---
title: "Static website permissions with aws S3 and cloudfront"
date: 2019-07-21T19:08:29+12:00
draft: false
---
## So you want to host a website?

I'm hosting this blog using AWS and s3 through cloudflare it's a mix of technology that is useful for me to pick up right now because it's matured and I like [boring technology](http://boringtechnology.club/).

However the biggest issue I faced was inconsistient documentation from AWS relating to permissions so I'm going to note down what I've done to help anyone else wanting to do the same thing.

I followed the links here but made some changes that I'll note along side them

* https://docs.aws.amazon.com/AmazonS3/latest/dev/HostingWebsiteOnS3Setup.html
    
    Make sure you set public access as part of this setup
    Take a note of the URL to access the bucket for the cloudflare part 

* https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html

    I'm using cloudflare for my DNS so I translated the section about route53 to the put the relevant values in cloudflare
    I also chose not to serve over http because it's becoming more likely to cause errors in browsers and I can get free certificates through cloudflare or cloudfront.
    

* https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-cloudfront-walkthrough.html
    This is where you can set the permissions on the bucket so that only cloudfront can access it through a wizard.
    This [link](https://stackoverflow.com/a/24377823) is pretty useful to disambiguate which DNS value to set in cloudfront

That was working for the homepage but I was getting persistient errors like this on everything else

![forbidden error](/aws-forbidden.png)

So what I found eventually is that you need to grant the read priveledges to something called the S3CannonicalID from your cloudfront distribution which is mentioned in the [troubleshooting docs](https://aws.amazon.com/premiumsupport/knowledge-center/s3-website-cloudfront-error-403/)

# Instructions
Here are the steps to correct the ACLs on your bucket objects (files).
Replace the sections in curly braces {{}} with the values from the output of your commands from your aws account

1. Get your cloudfront distribution ID

    ```
    aws cloudfront list-distributions
    ```

2.  Get the canonical user ID for the s3 bucket access

    ```
    aws cloudfront get-cloud-front-origin-access-identity-config --id {{CLOUDFRONT_ID}}
    ```

3.  Set permissions on the bucket for cloudfront

    ```
    aws s3api put-bucket-acl --bucket {{YOUR_BUCKET}} --grant-full-control id={{YOUR_ACCOUNT_ID}} --grant-read id={{YOUR_S3_ID}} --grant-read-acp id={{YOUR_S3_ID}}
    ```

4.  Set the permission on the object
    ```
    aws s3api put-object-acl --bucket {{YOUR_BUCKET}} --key {{WEBSITE_FILE}} --grant-read id={{YOUR_S3_ID}}
    ```

That should get you access via the cloudfront URL. Or your public URL if you've configured the domain and SSL etc.
As I update the blog I'll be using the grants syntax with the s3 cp comamnd to set all the oject ACLs when uploaded.

```
aws s3 cp --recursive {{WEBSITE_DIRECTORY}} s3://{{YOUR_BUCKET}}/ --grants full=id={{YOUR_ACCOUNT_ID}} read=id={{YOUR_S3_ID}} readacl={{YOUR_S3_ID}}
```