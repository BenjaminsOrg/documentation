# Terraform Pipelines using Github Actions

**Feature Name:** Github actions for terraform pipelines

**Type:** Infrastructure change

**Start Date:** 09-05-2024

**Author:** Ainharan Subramaniam

**Related components:** N/A

**Trello issues:** [Trello Link](https://trello.com/c/e9ahsE5l)

**Status: IN REVIEW**

## Summary

Github actions should initiate terraform pipelines without causing conflicts

## Motivation


Github actions should initiate terraform pipelines without causing conflicts. Developers should not manually applying changes to AWS

## Detailed design

On a high-level Github actions agent should assume an IAM role which allows it to perform terraform apply. The terraform backend should also be using a DynamoDB instance to lock the terraform statefile.

### System Design

![terraform_pipelines](/img/terraform_pipelines.png)

## Alternatives

Jenkins pipelines were discussed but actions was the past of least resistance, although self-hosting will probably be required at some point in the future whether it is our own github agent, self-hosted gitlab or jenkins for pipelines. Cost analysis will be due when we reach out github actions limits

## Unresolved questions

N/A

