---
title: "Terraform Infra"
date: 2021-12-25T21:11:35+13:00
draft: false
---

## So I was wrong

To remove the public bucket alert I have updated my blog infrastructure.
Using code I intended to use for static publishing for a CMS tool.
<https://github.com/Mossman1215/mountainmoss-tf>
This uses a cloudfront distribution to control access and the s3 only allows the cloudfront identity to fetch content.

I'm running hugo locally but it would be possible to hook a cms server in ec2 to upload content to the bucket instead.
Having a subdomain for api or form content can then allow the dynamic content to be included in the site.
This module is intended for that use.

<https://github.com/Mossman1215/tf-static-site>

I thought that rewriting index.html with lambda code was required based on this article <https://danmc.net/posts/aws-cloudfront-default-index/>

[perma link](https://web.archive.org/web/20210516185139/https://danmc.net/posts/aws-cloudfront-default-index/)

but instead it's easier to turn on ugly urls in hugo (no code is the best code!)

<https://gohugo.io/content-management/urls/#ugly-urls>
