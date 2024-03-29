# What is Digger

Digger is an open-source alternative to Terraform Cloud. It makes it easy to run `terraform plan` and `apply` in the CI / CD platform you already have, such as Github Actions.

## What's so hard about it?

Can't I just run it in my action as-is? Why do I need another tool?

Great question! You can; but you probably [don't want to](https://itnext.io/pains-in-terraform-collaboration-249a56b4534e).

Terraform has [state](https://developer.hashicorp.com/terraform/language/state), which is stored in a file. If you just run terraform on every PR like you would with application code, this can create a race condition. It can get pretty bad:

[Spotify accidentally deleted all its K8S clusters because of Terraform state confusion](https://youtu.be/ix0Tw8uinWs?t=306)

So there needs to be some sort of state-aware orchestration.

## What else is out there?

This isn't a new problem. Naturally, there are great products out there solving it.

#### Non-open-source

* [Terraform Cloud](https://cloud.hashicorp.com/products/terraform) - Terraform Cloud enables infrastructure automation for provisioning, compliance, and management of any cloud, data center, and service
* [Spacelift](https://spacelift.io/) - sophisticated CI/CD platform for Terraform, CloudFormation, Pulumi, Kubernetes, and Ansible
* [Env0](http://env0.com) - The Best Way to Manage Your Terraform and Infrastructure as Code
* [Scalr](https://www.scalr.com/) - Centralize Terraform administration while decentralizing operations
* [Terrateam](https://terrateam.io/) - Terraform GitOps with GitHub Pull Requests
* [Terrahub](https://www.terrahub.io/) - DevOps Hub for Terraform Automation
* [Terrahaxs](https://www.terrahaxs.com/) - GitOps CI/CD workflow for Terraform

#### Open-source

* [Atlantis](https://www.runatlantis.io/) - Terraform Pull Request Automation
* [Terrakube](https://terrakube.org/) - The Open Source Alternative to Terraform Enterprise
* [Hatchet](https://hatchet.run) - An all-in-one platform to automate, secure and monitor Terraform
* [OTF](https://github.com/leg100/otf) - An open-source alternative to Terraform Enterprise

## How is Digger different?

All of the existing solutions (both commercial and open-source) are effectively full-stack CI systems. But why have 2 CI systems, each with its own UI, compute, access controls and everything? If the problem is state, then it should be possible to just bridge the gap without duplicating what works well already.

Digger runs completely within your CI, such as Github Actions. It is a simple, fast binary written in go that manages your states and locks. This approach has the following benefits:

* No need to share sensitive data with another 3rd party - things like AWS secrets stay within your CI
* No need to host and maintain any compute backend - terraform binary runs natively in your managed CI environment

