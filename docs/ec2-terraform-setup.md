# AWS EC2 instance setup with Terraform

Terraform is an **Infrastructure as Code** tool that allows you to define infrastructure resources using a declarative language.

Terraform files into the [terraform folder](../terraform) of this repository allows you to easily get all the needed infrastructure resources to run deployment workflows.

## Prerequisites

- An active [AWS account](https://aws.amazon.com/console/)
- Basic familiarity with the AWS Management Console
- Basic familiarity with some bash commands and Linux systems

## Get your AWS CLI credentials

AWS CLI access keys enable Terraform to provision cloud infrastructure. You need:

* **AWS Access Key ID** – public identifier.
* **AWS Secret Access Key** – private key.

Generate keys via your [AWS Console](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user_manage_add-key.html) and save them as two secrets named `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`.

## Set up an SSH keypair

SSH key pairs enable secure connections to your EC2 instance. The following operations allows you and your github action to add a public key to the EC2 instance and correctly connect to it when you run your workflow.

The `infrastructure-provisioning` workflow automatically creates an SSH keypair on the GitHub runner and automatically updates all the secrets for deployment workflows.

You can read workflow logs in order to know the updated secret values. 

## Create a GitHub Personal Access Token

Personal Access Tokens (PAT) allows you to identify yourself whe you want to interact with the GitHub API. For example when you want to update a secret value using GitHub API.

The `infrastructure-provisioning` workflow automatically updates some repository secrets, so it needs a PAT token in order to do it.

On GitHub:
* Go to **Profile** > **Settings** > **Developer settings** > **Personal Access tokens** > **Fine-grained tokens**.
* Click on the **Generate new token** button and insert the requested information.
* Remember to add permissions about **Seccrets** and **Actions**.
* Copy and paste the generated token into the `PAT_TOKEN`secret of the repository.

## Manually trigger terraform workflow

When you want to get an infrastructure using Terraform you can trigger the `infrastructure-provisioning` workflow.

On GitHub:
* Go to repository **Actions** tab.
* Select the `infrastructure-provisioning` workflow.
* Click on the **Run workflow** button.

At the end of the workflow execution you can check workflow logs and get some informations like:
- Your machine public IP
- The generated keypair (public and private key)

## Test SSH connection to the machine

In your terminal:

* Create a private key file, paste into it the content of actions logs and set correct permsission:

```bash
cd ~/.ssh
> deploy_key
vim ./deploy_key
chmod 400 ./deploy_key
```

* Open an SSH connection to your instance:

```bash
cd ~/.ssh
ssh -i deploy_key ubuntu@<hostname>
```

>[!NOTE]
> Copy and paste **ALL** the content of the log into the secret variable. Include also the following lines:
>
> `-----BEGIN OPENSSH PRIVATE KEY-----`
>
> `-----END OPENSSH PRIVATE KEY-----`