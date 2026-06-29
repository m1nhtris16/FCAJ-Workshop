---
title: "Week 2 Worklog"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 2 Objectives:

* Master core AWS compute services, focusing on High Availability and Scaling (Elastic Load Balancing - ELB, and Auto Scaling Groups - ASG).
* Understand Amazon S3 (Simple Storage Service) fundamentals, storage classes, security mechanisms, and lifecycle management.
* Explore EBS (Elastic Block Store) types and volume management.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Learn about Elastic Load Balancing (ELB) types (ALB, NLB, GLB) <br> - Understand listener rules, target groups, and health checks | 06/22/2026 | 06/22/2026 | [Building Highly Available Web Applications](https://docs.aws.amazon.com/elasticloadbalancing/) |
| 3 | - Study Auto Scaling Groups (ASG) and launch templates <br> - Configure scaling policies (Target Tracking, Step Scaling) | 06/23/2026 | 06/23/2026 | [Scaling Applications with EC2 Auto Scaling](https://000006.awsstudygroup.com/) |
| 4 | - Learn S3 storage concepts: buckets, keys, security (Bucket Policies, ACLs) <br> - Study S3 Storage Classes and SSE-S3/SSE-KMS encryption | 06/24/2026 | 06/24/2026 | [Static Website Hosting with Amazon S3](https://000057.awsstudygroup.com/) |
| 5 | - Explore S3 advanced features (Versioning, Lifecycle Rules, Object Lock) <br> - Study EBS block storage types (gp3, io2) and EBS Snapshots | 06/25/2026 | 06/25/2026 | [S3 Security Best Practices](https://000069.awsstudygroup.com/) <br> [Amazon EBS Volume Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html) |
| 6 | - **Hands-on Practice:** <br> &emsp; + Create an S3 bucket with versioning and a lifecycle rule to transition objects to Glacier <br> &emsp; + Create a Launch Template, Target Group, Application Load Balancer (ALB), and Auto Scaling Group (ASG) to test high availability | 06/26/2026 | 06/26/2026 | [Building Highly Available Web Applications](https://docs.aws.amazon.com/elasticloadbalancing/) <br> [Static Website Hosting with Amazon S3](https://000057.awsstudygroup.com/) |


### Week 2 Achievements:

* **High Availability & Load Balancing**: Mastered ELB (Application Load Balancer) concepts, successfully configuring target groups and health checks to route traffic effectively between EC2 instances.
* **Auto Scaling**: Understood the mechanism of Auto Scaling Groups (ASG) combined with Launch Templates to automatically adjust instance capacity based on traffic demands or resource utilization.
* **Scalable Object Storage**: Gained a deep understanding of Amazon S3, including bucket permissions (Bucket Policies, block public access), encryption, and storage classes.
* **Lifecycle & Block Storage Management**: Created lifecycle rules in Amazon S3 to automate cost optimization, and explored different EBS volume types (gp3, io2) for optimal block storage configurations.
* **Hands-on Lab**: Built a highly available infrastructure featuring an ALB and ASG dynamically scaling EC2 instances across multiple availability zones, and set up a versioned S3 bucket with Glacier archiving policies.
