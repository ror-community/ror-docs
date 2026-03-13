---
title: Infrastructure
deprecated: false
hidden: false
metadata:
  robots: index
---
ROR provides an openly available [REST API](doc:rest-api) and [web-based search interface](doc:web-search) as access points to ROR JSON data. Code for these services and their corresponding infrastructure configuration is open source <Anchor label="in ROR's GitHub repositories" target="_blank" href="https://github.com/ror-community/">in ROR's GitHub repositories</Anchor>. These services primarily utilize the following underlying technologies:

* Amazon Web Services (AWS) infrastructure, including OpenSearch, Elastic Container Service, EC2, and S3
* Django open-source Python framework
* Ember open-source Javascript framework
* Docker containerization technology
* Datadog log/metrics aggregation and analysis tools
* Terraform infrastructure automation

AWS resources are located in the AWS eu-west-1 (Ireland) region, in order to ensure compliance with EU data protection policies such as GDPR. ROR’s DataDog account is also located in the EU.

ROR also publishes its entire dataset in JSON and CSV formats on Zenodo at [https://doi.org/10.5281/zenodo.6347574](https://doi.org/10.5281/zenodo.6347574) .

# Robustness

ROR’s infrastructure is designed to be highly scalable and highly available. OpenSearch, EC2 load balancer and ECS container instances can be scaled as needed to accommodate traffic volume. ROR has two instances of its API container running in its production environment to allow failover, and one instance of all other services. Containerized services are configured to automatically recover from any interruptions. ROR also has isolated development and staging instances of each service to allow testing and utilizes an automated CI/CD process to deploy changes from one environment to another.

# Sustainability

While ROR currently runs on AWS infrastructure, it could be easily ported to another provider because ROR has carefully captured all configuration as Terraform code in GitHub. This allows a developer to easily understand all the components needed to run ROR services, along with their configurations. Similarly, since all ROR code is open source, 

# Security

The ROR dataset does not contain sensitive data, which minimizes data privacy/security risks. ROR software also does not provide user accounts or other access-controlled services that could result in exposed credentials or unauthorized access. ROR mitigates security risks posed by potential unauthorized access to systems through the following approaches:

* Following the principle of least access for user accounts as well as tokens and keys use for programmatic access
* Using multi-factor authentication where available
* Using a secure password storage/sharing service
* Passing credentials into code via environment variables rather than storing values in application code files. Values for these environment variables are set using either GitHub or Terraform secure secret storage.

IP addresses from queries of the ROR API are collected in access log files. Access logs are stored in private AWS S3 buckets and also sent to Datadog. Access logs are deleted from Datadog after 30 days.

Email addresses are collected from users of the ROR API who [register for a client ID](doc:client-id), meaning that ROR API requests with a client ID can be connected to a user's email address. Email addresses supplied at client ID registration are used only to contact API users for support and troubleshooting purposes and are not shared outside of ROR technical infrastructure.

# Licensing

ROR’s code and software processes are open source and are <Anchor label="stored and documented on GitHub" target="_blank" href="https://github.com/ror-community">stored and documented on GitHub</Anchor>. Code is published under a fully permissible <Anchor label="MIT License" target="_blank" href="https://opensource.org/license/mit">MIT License</Anchor>.

The ROR dataset, including ROR IDs and metadata, are in the public domain and can be used by anyone at no cost: ROR data is provided under the <Anchor label="Creative Commons CC0 1.0 Universal Public Domain Dedication" target="_blank" href="https://creativecommons.org/publicdomain/zero/1.0/">Creative Commons CC0 1.0 Universal Public Domain Dedication</Anchor>.

ROR [logos](https://ror.readme.io/docs/display) are available under the <Anchor label="Creative Commons Attribution No Derivatives 4.0 International License" target="_blank" href="https://creativecommons.org/licenses/by-nd/4.0/">Creative Commons Attribution No Derivatives 4.0 International License</Anchor>, which means that when using a ROR logo you must credit ROR (a link is sufficient) and that you may not make derivatives of the image.

ROR website content is licensed under the <Anchor label="Creative Commons Attribution 4.0 International License" target="_blank" href="https://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</Anchor>, meaning that content may be quoted and reproduced with attribution.

# Preservation

The ROR data file is published <Anchor label="on the Zenodo digital repository" target="_blank" href="https://doi.org/10.5281/zenodo.6347574">on the Zenodo digital repository</Anchor> and is subject to <Anchor label="Zenodo’s preservation practices" target="_blank" href="https://about.zenodo.org/infrastructure/">Zenodo’s preservation practices</Anchor>, which include regular backup, replication, and fixity processes, a retention policy of at least 20 years, and succession planning in the event of repository closure. ROR code repositories are automatically archived in <Anchor label="Software Heritage" target="_blank" href="https://www.softwareheritage.org/">Software Heritage</Anchor>. The schedule is determined by Software Heritage.

<br />
