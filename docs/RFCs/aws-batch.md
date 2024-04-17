# AWS Batch for Web Scraping

**Feature Name:** AWS Batch for scraping

**Type:** Infrastructure change

**Start Date:** 15-04-2024

**Author:** Ainharan Subramaniam

**Related components:** N/A

**Trello issues:** https://trello.com/c/9mTYhRbg

## Summary

AWS batch for scraping job post data from other job board websites.

## Motivation

Need a robust batching service, that will track failure of jobs. I.e. we should not have duplicated data on failure/re-run. Jobs should be written in a idempotent way to mitigate this issue, and on a infrastructure level our system should be able to track and log the state of the current batch process. The design should be cost effective as well as be able to handle dependent and sequential jobs.

## Detailed design

Jobs should be written in an idempotent way since we will be utilizing Spot instances to reduce costs. With Spot instances there's a risk of interruption as Spot Instances can be terminated by AWS when the Spot price exceeds your bid or when capacity is no longer available.

### Batch
- Managed Service: AWS Batch is a fully managed service that simplifies batch computing in the cloud. It handles the provisioning and scaling of the compute resources for you.
- Removes complexity of creating our own scheduling solution: AWS Batch supports job scheduling and dependencies, allowing you to manage complex batch workflows.
    - Even if we use an out of the box scheduling/batching solution like Spring Dataflow, we still need to containerize and manage it. AWS batch avoids this.
- AWS Batch itself does not have a separate management fee, but you pay for the compute resources, storage, and data transfer associated with your batch jobs.
- Rotating IPs: webscraping will typically get blocked by firewalls and batching 
- Integration: It integrates well with other AWS services like S3, Lambda, and CloudWatch, making it easier to build end-to-end batch processing workflows.
- Data transfer within the same AWS region is generally free, but cross-region or internet-bound data transfer may incur charges.
    - This is true for ECS/EC2 as well
- If you're not familiar with AWS Batch, there might be a learning curve involved.

### System Design

![batch_solution](/img/batch_solution.png)

### What are we scaping
- Name
- Description
- Url

## Drawbacks

Limited control. I believe that simplifying the workload is more important.

## Alternatives

### EC2
- Full control
- We are responsible for availablity & faul, database maintainence (unless we use a managed service like RDS)
- A lot of work to maintain
- EC2 we will need to configure when to turn on and off instance for cost optimization
- Cheap

### ECS
- Better control of container environment. Allows you to customize the runtime environment for your batch jobs.
- ECS integrates well with other AWS services and supports Docker containers, making it easier to manage dependencies and configurations.
- Flexible and scalable platform to run containerized batch jobs
- Integration: ECS integrates well with other AWS services and supports Docker containers, making it easier to manage dependencies and configurations.
- ECS uses EC2's on the backend or Fargate (simplier but more expensive option so I will not expand on this)
    - EC2's can be Spot/on-demand/reserved


## Unresolved questions

What parts of the design are still to be done?
