# Provisioning Tools

## What is Provisioning?

Provisioning means **setting up the infrastructure a system needs before an application can run on it** — servers, networks, storage, load balancers, databases, security groups, etc.

> "Infrastructure as Code means treating infrastructure the same way you treat application code — versioned, tested, and repeatable."
> — *Terraform: Up & Running*, Yevgeniy Brikman

Instead of manually creating a VM, attaching storage, and configuring networking by hand every time, provisioning tools let you **define the desired infrastructure in code** and let the tool build it for you — consistently, every time.

## Why We Need Provisioning Tools

- Manually creating infrastructure is slow, error-prone, and hard to repeat exactly
- Environments (Dev, QA, Prod) need to match — provisioning tools guarantee that
- Scaling from 1 server to 100 servers should take the same effort — one command, not 100 manual steps
- Infrastructure changes need a history, just like code changes

## Popular Provisioning Tools

| Tool | Type | Best For |
|---|---|---|
| **Terraform** | Declarative, multi-cloud | Provisioning infra across AWS, Azure, OCI, GCP with one tool |
| **AWS CloudFormation** | Declarative, AWS-only | Native AWS infrastructure provisioning |
| **OCI Resource Manager** | Declarative, OCI-only | Native Oracle Cloud provisioning (Terraform-based) |
| **Pulumi** | Imperative (real code) | Teams that prefer Python/TypeScript/Go over a DSL |
| **Ansible** | Configuration + light provisioning | Combining provisioning with configuration management |

## Example: Provisioning a Server with Terraform

```hcl
# main.tf
resource "aws_instance" "web_server" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t2.micro"

  tags = {
    Name = "MyWebServer"
    Env  = "Dev"
  }
}
```

Run it:

```bash
terraform init      # Download provider plugins
terraform plan       # Preview what will be created
terraform apply      # Create the actual infrastructure
```

## Example: Provisioning on Oracle Cloud (OCI)

```hcl
# main.tf
resource "oci_core_instance" "app_server" {
  compartment_id      = var.compartment_id
  availability_domain = var.availability_domain
  shape                = "VM.Standard.E4.Flex"

  source_details {
    source_type = "image"
    source_id   = var.image_id
  }

  display_name = "app-server-01"
}
```

## Example: Destroying Infrastructure Cleanly

```bash
terraform destroy
```

One command tears down exactly what was created — no manual cleanup, no leftover resources costing money.

## Provisioning vs Configuration Management (Quick Distinction)

| Provisioning | Configuration Management |
|---|---|
| "What infrastructure should **exist**?" | "What **state** should that infrastructure be in?" |
| Terraform, CloudFormation | Ansible, Puppet, Chef |
| Creates the server | Installs software, sets configs on that server |

They're often used together: **Terraform provisions the server, Ansible configures it.**

## A Good Quote to Remember

> "You want your infrastructure to be like cattle, not pets. If a server dies, you don't nurse it back to health — you provision a new one from code."
> — Common DevOps/SRE principle (popularized in cloud-native circles)

## In One Sentence

**Provisioning tools let you define your infrastructure as code — so you can create, update, and destroy environments reliably, repeatably, and at any scale, with a single command.**
