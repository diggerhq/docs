# How it works

Main components:

* CLI that can be run both locally and in your CI system
* Github Action
* DynamoDB table for code-level locks (auto-provisioned on the first run of the action). For clouds other than AWS we use alternative resources to DynamoDB. (Storage tables in Azure and Buckets in GCP).
* Optional Backend component that handles webhooks from Github / Gitlab / Bitbucket.

## Digger Architecture

<figure><img src="../.gitbook/assets/Screen Shot 2023-07-21 at 17.24.06 (1).png" alt=""><figcaption></figcaption></figure>

The main idea behind Digger is that it does not need you to host any persistent compute - instead, it runs Terraform natively in the compute environment provided by your CI system.

Here are some high level architecture considerations that Digger makes in comparison to other systems:

* Compute environment for terraform - Digger doesn't need this, unlike Terraform Cloud, Atlantis, Spacelift, Terrakube etc. With Digger the terraform binary runs natively in your CI system's compute, whatever it is - Github Action or private worker or your K8S cluster in case you are using smth like ArgoCD
* Optional Application backend of Digger for handling webhooks etc - Digger comes with a serverless function that can be easily provisioned in your cloud account. Unlike other terraform collaboration tools, Digger doesn't require a backend to run. Digger CLI natively provides functionality to run terraform collaboratively. For advanced features, there is an optional backend that can be self hosted or be consumed as a managed service (Digger Pro).
* Code-level locks table in DynamoDB (Or Azure Storage Table/ GCP Bucket) - used to store the locks per PR / project. This is not the same as low-level state locks natively supported by terraform (those only prevent multiple applies modifying the same state; not code-level race conditions). The table is automatically created by Digger action on its first run.
* Terraform state backend service (SOON) - HTTP service on top of state storage, configured as "cloud" backend in your Terraform code. Terraform Cloud and Spacelift come with such service, typically it's just a wrapper on top of S3. At Digger we believe that it's sufficient to just use an S3 backend directly, which is natively supported by Terraform. **Note that as of today digger does not manager your state backend and you are expected to write provider configuration code to link your terraform to your own state backend otherwise it will lead to loss of state file.**
* S3 state + DynamoDB low-level locks table (native to terraform) configured as "S3 backend" in your Terraform code. Digger does not interfere with your state backend setup, you can keep using it as-is.
