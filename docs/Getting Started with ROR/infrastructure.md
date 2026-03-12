---
title: Infrastructure
deprecated: false
hidden: true
metadata:
  robots: index
---
ROR provides an openly available [REST API](doc:rest-api) and [web-based search interface](doc:web-search) as access points to ROR JSON data. Code for these services and their corresponding infrastructure configuration is open source <Anchor label="in ROR's GitHub repositories" target="_blank" href="https://github.com/ror-community/">in ROR's GitHub repositories</Anchor>. These services primarily utilize the following underlying technologies:

* Amazon Web Services (AWS) infrastructure, including OpenSearch, Elastic Container Service, EC2, and S3
* Django open-source Python framework
* Ember open-source Javascript framework
* Docker containerization technology
* DataDog log/metrics aggregation and analysis tools
* Terraform infrastructure automation

AWS resources are located in the AWS eu-west-1 (Ireland) region, in order to ensure compliance with EU data protection policies such as GDPR. ROR’s DataDog account is also located in the EU.

ROR also publishes its entire dataset in JSON and CSV formats on Zenodo at [https://doi.org/10.5281/zenodo.6347574](https://doi.org/10.5281/zenodo.6347574) .

# Robustness

ROR’s infrastructure is designed to be highly scalable and highly available. OpenSearch, EC2 load balancer and ECS container instances can be scaled as needed to accommodate traffic volume. ROR has two instances of its API container running in its production environment to allow failover, and one instance of all other services. Containerized services are configured to automatically recover from any interruptions. ROR also has isolated development and staging instances of each service to allow testing and utilizes an automated CI/CD process to deploy changes from one environment to another.

# Sustainability

While ROR currently runs on AWS infrastructure, it could be easily ported to another provider because ROR has carefully captured all configuration as Terraform code in GitHub. This allows a developer to easily understand all the components needed to run ROR services, along with their configurations.

# Security

The ROR dataset does not contain sensitive data, which minimizes data privacy/security risks. ROR software also does not provide user accounts or other access-controlled services that could result in exposed credentials or unauthorized access. ROR mitigates security risks posed by potential unauthorized access to systems through the following approaches:

* Following the principle of least access for user accounts as well as tokens and keys use for programmatic access
* Using multi-factor authentication where available
* Using a secure password storage/sharing service
* Passing credentials into code via environment variables rather than storing values in application code files. Values for these environment variables are set using either GitHub or Terraform secure secret storage.

ROR does not systematically collect or store information about users of its services, except IP addresses, which are collected in access log files. Access logs are stored in private AWS S3 buckets and also sent to Datadog. Access logs are deleted from Datadog after 30 days.

<br />
