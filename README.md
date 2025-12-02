# Terraform-aws-security-group

# AWS Infrastructure Provisioning with Terraform

<p align="center">
  <img src="https://img.shields.io/badge/Terraform-Module-6610f2?style=for-the-badge&logo=terraform&logoColor=white"/>
  <img src="https://img.shields.io/github/stars/yrahul05/terraform-aws-security-group?style=for-the-badge"/>
</p>


> A clean and opinionated Terraform module by **[Rahul Yadav](https://github.com/yrahul05)**  
> To use this module, include it in your Terraform configuration file and provide the required input variables. Below is an example of how to use the module:
---

👤 ABOUT ME

Rahul Yadav  
Certified Cloud & DevOps Engineer  
CEO & CTO – [PrimeOps Technologies](https://primeops.co.in/)

## 🚀 [PrimeOps Technologies](www.primeops.co.in) – Services

> **Services Offered**
> - ✔️ Terraform, Kubernetes and Ansible automation
> - ✔️ CI/CD pipelines (GitHub Actions, GitLab CI, Jenkins, Argo CD)
> - ✔️ Cloud setup on AWS, Azure, GCP, Hetzner and DigitalOcean
> - ✔️ Multi-cloud architecture and cost optimization
> - ✔️ Security and DevSecOps (scanning, secrets, compliance)
> - ✔️ Docker, microservices and service mesh
> - ✔️ Monitoring and logging (Prometheus, Grafana, ELK)
> - ✔️ Migrations and modernization
> - ✔️ Managed services: 24/7 monitoring, maintenance and support


## 🔗 Links

### Personal Profiles
> **GitHub:** [https://github.com/yrahul05](https://github.com/yrahul05)  
> **LinkedIn:** [https://www.linkedin.com/in/rahulyadavdevops/](https://www.linkedin.com/in/rahulyadavdevops/)  
> **Upwork:** [https://www.upwork.com/freelancers/~0183ad8a41e8284283](https://www.upwork.com/freelancers/~0183ad8a41e8284283)

### PrimeOps Technologies
> **Website:** [https://primeops.co.in/](https://primeops.co.in/)  
> **GitHub:** [https://github.com/PrimeOps-Technologies](https://github.com/PrimeOps-Technologies)  
> **LinkedIn:** [https://www.linkedin.com/company/primeops-technologies](https://www.linkedin.com/company/primeops-technologies)  
> **Upwork Agency:** [https://www.upwork.com/agencies/1990756660262272773/](https://www.upwork.com/agencies/1990756660262272773/)

---

🌟 Features

- Creates and manages AWS Security Groups
- Supports new and existing Security Groups
- Flexible ingress and egress rule management
- Allows rules with CIDR blocks, prefix lists, and security group references
- Supports both IPv4 and IPv6 traffic rules
- Enables fine-grained control for TCP, UDP, and ICMP protocols
- Compatible with VPC and cross-VPC setups
- Automatically tags resources using the Labels module
- Fully compatible with other PrimeOps-Technologies Terraform modules

## Examples

## Example: Basic

```hcl
module "security_group" {
  source      = "git::https://github.com/yrahul05/terraform-aws-security-group.git?ref=v1.0.0"
  name        = local.name
  environment = local.environment
  vpc_id      = module.vpc.vpc_id

  ## INGRESS Rules
  new_sg_ingress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["172.16.0.0/16"]
    description = "Allow ssh traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["172.16.0.0/16"]
      description = "Allow Mongodb traffic."
    }
  ]

  ## EGRESS Rules
  new_sg_egress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["172.16.0.0/16"]
    description = "Allow ssh outbound traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["172.16.0.0/16"]
      description = "Allow Mongodb outbound traffic."
    }
  ]
}
```

## Example: Complete
```hcl
module "security_group" {
  source      = "git::https://github.com/yrahul05/terraform-aws-security-group.git?ref=v1.0.0"
  name        = local.name
  environment = local.environment
  vpc_id      = module.vpc.vpc_id

  ## INGRESS Rules
  new_sg_ingress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["172.16.0.0/16"]
    description = "Allow ssh traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["172.16.0.0/16"]
      description = "Allow Mongodb traffic."
    }
  ]

  new_sg_ingress_rules_with_self = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    description = "Allow ssh traffic."
  },
    {
      rule_count  = 2
      from_port   = 443
      to_port     = 443
      protocol    = "tcp"
      description = "Allow Mongodbn traffic."
    }
  ]

  new_sg_ingress_rules_with_source_sg_id = [{
    rule_count               = 1
    from_port                = 22
    to_port                  = 22
    protocol                 = "tcp"
    source_security_group_id = "sg-0f251332b1474ba77"
    description              = "Allow ssh traffic."
  },
    {
      rule_count               = 2
      from_port                = 27017
      to_port                  = 27017
      protocol                 = "tcp"
      source_security_group_id = "sg-0f251332b1474ba77"
      description              = "Allow Mongodb traffic."
    }]

  ## EGRESS Rules
  new_sg_egress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [module.vpc.vpc_cidr_block, "172.16.0.0/16"]
    description = "Allow ssh outbound traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["172.16.0.0/16"]
      description = "Allow Mongodb outbound traffic."
    }
  ]

  new_sg_egress_rules_with_self = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    description = "Allow ssh outbound traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      description = "Allow Mongodb traffic."
    }]

  new_sg_egress_rules_with_source_sg_id = [{
    rule_count               = 1
    from_port                = 22
    to_port                  = 22
    protocol                 = "tcp"
    source_security_group_id = "sg-0f251332b1474ba77"
    description              = "Allow ssh outbound traffic."
  },
    {
      rule_count               = 2
      from_port                = 27017
      to_port                  = 27017
      protocol                 = "tcp"
      source_security_group_id = "sg-0f251332b1474ba77"
      description              = "Allow Mongodb traffic."
    }]
}
```

## Example: Only_rules

```hcl
module "security_group_rules" {
  source         = "git::https://github.com/yrahul05/terraform-aws-security-group.git?ref=v1.0.0"
  name           = local.name
  environment    = local.environment
  vpc_id         = module.vpc.vpc_id
  new_sg         = false
  existing_sg_id = "sg-0474592052307dbb2"

  ## INGRESS Rules
  existing_sg_ingress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["10.9.0.0/16"]
    description = "Allow ssh traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["10.9.0.0/16"]
      description = "Allow Mongodb traffic."
    }
  ]

  ## EGRESS Rules
  existing_sg_egress_rules_with_cidr_blocks = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["10.9.0.0/16"]
    description = "Allow ssh outbound traffic."
  },
    {
      rule_count  = 2
      from_port   = 27017
      to_port     = 27017
      protocol    = "tcp"
      cidr_blocks = ["10.9.0.0/16"]
      description = "Allow Mongodb outbound traffic."
    }]
}
```

## Example: Prefix_list

```hcl
module "security_group" {
  source              = "git::https://github.com/yrahul05/terraform-aws-security-group.git?ref=v1.0.0"
  name                = local.name
  environment         = local.environment
  vpc_id              = module.vpc.vpc_id
  prefix_list_enabled = true
  entry = [{
    cidr = "10.19.0.0/16"
  }]

  ## INGRESS Rules
  new_sg_ingress_rules_with_prefix_list = [{
    rule_count  = 1
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    description = "Allow ssh traffic."
  }
  ]

  ## EGRESS Rules
  new_sg_egress_rules_with_prefix_list = [{
    rule_count  = 1
    from_port   = 3306
    to_port     = 3306
    protocol    = "tcp"
    description = "Allow mysql/aurora outbound traffic."
  }
  ]
}
```

## Examples
For detailed examples on how to use this module, please refer to the [Examples](https://github.com/yrahul05/terraform-aws-security-group) directory within this repository.


| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.5.0 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 5.1.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | >= 5.1.0 |

## Modules

| Name | Source                                                | Version |
|------|-------------------------------------------------------|---------|
| <a name="module_labels"></a> [labels](#module\_labels) | git::git@github.com:yrahul05/terraform-multicloud-labels.git | v1.0.0 |

## Resources

| Name | Type |
|------|------|
| [aws_ec2_managed_prefix_list.prefix_list](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ec2_managed_prefix_list) | resource |
| [aws_security_group.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group) | resource |
| [aws_security_group_rule.existing_sg_egress_with_cidr_blocks](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.existing_sg_egress_with_prefix_list](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.existing_sg_egress_with_self](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.existing_sg_egress_with_source_sg_id](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.existing_sg_ingress_cidr_blocks](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.existing_sg_ingress_with_prefix_list](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.existing_sg_ingress_with_self](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.existing_sg_ingress_with_source_sg_id](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.new_sg_egress_with_cidr_blocks](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.new_sg_egress_with_prefix_list](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.new_sg_egress_with_self](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.new_sg_egress_with_source_sg_id](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.new_sg_ingress_with_cidr_blocks](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.new_sg_ingress_with_prefix_list](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.new_sg_ingress_with_self](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.new_sg_ingress_with_source_sg_id](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group.existing](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/security_group) | data source |

## Inputs

| Name | Description                                                                                                                                                                                                                                                                 | Type | Default | Required |
|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------|---------|:--------:|
| <a name="input_enable"></a> [enable](#input\_enable) | Flag to control module creation.                                                                                                                                                                                                                                            | `bool` | `true` | no |
| <a name="input_entry"></a> [entry](#input\_entry) | Can be specified multiple times for each prefix list entry.                                                                                                                                                                                                                 | `list(any)` | `[]` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | Environment (e.g. `prod`, `dev`, `staging`).                                                                                                                                                                                                                                | `string` | `""` | no |
| <a name="input_existing_sg_egress_rules_with_cidr_blocks"></a> [existing\_sg\_egress\_rules\_with\_cidr\_blocks](#input\_existing\_sg\_egress\_rules\_with\_cidr\_blocks) | Ingress rules with only cidr block. Should be used when there is existing security group.                                                                                                                                                                                   | `any` | `{}` | no |
| <a name="input_existing_sg_egress_rules_with_prefix_list"></a> [existing\_sg\_egress\_rules\_with\_prefix\_list](#input\_existing\_sg\_egress\_rules\_with\_prefix\_list) | Egress rules with only prefic ist ids. Should be used when there is existing security group.                                                                                                                                                                                | `any` | `{}` | no |
| <a name="input_existing_sg_egress_rules_with_self"></a> [existing\_sg\_egress\_rules\_with\_self](#input\_existing\_sg\_egress\_rules\_with\_self) | Egress rules with only self. Should be used when there is existing security group.                                                                                                                                                                                          | `any` | `{}` | no |
| <a name="input_existing_sg_egress_rules_with_source_sg_id"></a> [existing\_sg\_egress\_rules\_with\_source\_sg\_id](#input\_existing\_sg\_egress\_rules\_with\_source\_sg\_id) | Egress rules with only source security group id. Should be used when there is existing security group.                                                                                                                                                                      | `any` | `{}` | no |
| <a name="input_existing_sg_id"></a> [existing\_sg\_id](#input\_existing\_sg\_id) | Provide existing security group id for updating existing rule                                                                                                                                                                                                               | `string` | `null` | no |
| <a name="input_existing_sg_ingress_rules_with_cidr_blocks"></a> [existing\_sg\_ingress\_rules\_with\_cidr\_blocks](#input\_existing\_sg\_ingress\_rules\_with\_cidr\_blocks) | Ingress rules with only cidr blocks. Should be used when there is existing security group.                                                                                                                                                                                  | `any` | `{}` | no |
| <a name="input_existing_sg_ingress_rules_with_prefix_list"></a> [existing\_sg\_ingress\_rules\_with\_prefix\_list](#input\_existing\_sg\_ingress\_rules\_with\_prefix\_list) | Ingress rules with only prefix\_list. Should be used when new security group is been deployed.                                                                                                                                                                              | `any` | `{}` | no |
| <a name="input_existing_sg_ingress_rules_with_self"></a> [existing\_sg\_ingress\_rules\_with\_self](#input\_existing\_sg\_ingress\_rules\_with\_self) | Ingress rules with only source security group id. Should be used when new security group is been deployed.                                                                                                                                                                  | `any` | `{}` | no |
| <a name="input_existing_sg_ingress_rules_with_source_sg_id"></a> [existing\_sg\_ingress\_rules\_with\_source\_sg\_id](#input\_existing\_sg\_ingress\_rules\_with\_source\_sg\_id) | Ingress rules with only prefix list ids. Should be used when there is existing security group.                                                                                                                                                                              | `any` | `{}` | no |
| <a name="input_label_order"></a> [label\_order](#input\_label\_order) | Label order, e.g. `name`,`application`.                                                                                                                                                                                                                                     | `list(any)` | <pre>[<br>  "name",<br>  "environment"<br>]</pre> | no |
| <a name="input_managedby"></a> [managedby](#input\_managedby) | ManagedBy, eg 'Rahul Yadav'.                                                                                                                                                                                                                                                | `string` | `"example"` | no |
| <a name="input_max_entries"></a> [max\_entries](#input\_max\_entries) | The maximum number of entries that this prefix list can contain.                                                                                                                                                                                                            | `number` | `5` | no |
| <a name="input_name"></a> [name](#input\_name) | Name  (e.g. `app` or `cluster`).                                                                                                                                                                                                                                            | `string` | `""` | no |
| <a name="input_new_sg"></a> [new\_sg](#input\_new\_sg) | Flag to control creation of new security group.                                                                                                                                                                                                                             | `bool` | `true` | no |
| <a name="input_new_sg_egress_rules_with_cidr_blocks"></a> [new\_sg\_egress\_rules\_with\_cidr\_blocks](#input\_new\_sg\_egress\_rules\_with\_cidr\_blocks) | Egress rules with only cidr\_blockd. Should be used when new security group is been deployed.                                                                                                                                                                               | `any` | `{}` | no |
| <a name="input_new_sg_egress_rules_with_prefix_list"></a> [new\_sg\_egress\_rules\_with\_prefix\_list](#input\_new\_sg\_egress\_rules\_with\_prefix\_list) | Egress rules with only prefix list ids. Should be used when new security group is been deployed.                                                                                                                                                                            | `any` | `{}` | no |
| <a name="input_new_sg_egress_rules_with_self"></a> [new\_sg\_egress\_rules\_with\_self](#input\_new\_sg\_egress\_rules\_with\_self) | Egress rules with only self. Should be used when new security group is been deployed.                                                                                                                                                                                       | `any` | `{}` | no |
| <a name="input_new_sg_egress_rules_with_source_sg_id"></a> [new\_sg\_egress\_rules\_with\_source\_sg\_id](#input\_new\_sg\_egress\_rules\_with\_source\_sg\_id) | Egress rules with only source security group id. Should be used when new security group is been deployed.                                                                                                                                                                   | `any` | `{}` | no |
| <a name="input_new_sg_ingress_rules_with_cidr_blocks"></a> [new\_sg\_ingress\_rules\_with\_cidr\_blocks](#input\_new\_sg\_ingress\_rules\_with\_cidr\_blocks) | Ingress rules with only cidr blocks. Should be used when new security group is been deployed.                                                                                                                                                                               | `any` | `{}` | no |
| <a name="input_new_sg_ingress_rules_with_prefix_list"></a> [new\_sg\_ingress\_rules\_with\_prefix\_list](#input\_new\_sg\_ingress\_rules\_with\_prefix\_list) | Ingress rules with only prefix list ids. Should be used when new security group is been deployed.                                                                                                                                                                           | `any` | `{}` | no |
| <a name="input_new_sg_ingress_rules_with_self"></a> [new\_sg\_ingress\_rules\_with\_self](#input\_new\_sg\_ingress\_rules\_with\_self) | Ingress rules with only self. Should be used when new security group is been deployed.                                                                                                                                                                                      | `any` | `{}` | no |
| <a name="input_new_sg_ingress_rules_with_source_sg_id"></a> [new\_sg\_ingress\_rules\_with\_source\_sg\_id](#input\_new\_sg\_ingress\_rules\_with\_source\_sg\_id) | Ingress rules with only source security group id. Should be used when new security group is been deployed.                                                                                                                                                                  | `any` | `{}` | no |
| <a name="input_prefix_list_address_family"></a> [prefix\_list\_address\_family](#input\_prefix\_list\_address\_family) | (Required, Forces new resource) The address family (IPv4 or IPv6) of prefix list.                                                                                                                                                                                           | `string` | `"IPv4"` | no |
| <a name="input_prefix_list_enabled"></a> [prefix\_list\_enabled](#input\_prefix\_list\_enabled) | Enable prefix\_list.                                                                                                                                                                                                                                                        | `bool` | `false` | no |
| <a name="input_prefix_list_ids"></a> [prefix\_list\_ids](#input\_prefix\_list\_ids) | The ID of the prefix list.                                                                                                                                                                                                                                                  | `list(string)` | `[]` | no |
| <a name="input_repository"></a> [repository](#input\_repository) | Terraform current module repo                                                                                                                                                                                                                                               | `string` | `"https://github.com/yrahul05/terraform-aws-security-group"` | no |
| <a name="input_sg_description"></a> [sg\_description](#input\_sg\_description) | Security group description. Defaults to Managed by Terraform. Cannot be empty string. NOTE: This field maps to the AWS GroupDescription attribute, for which there is no Update API. If you'd like to classify your security groups in a way that can be updated, use tags. | `string` | `null` | no |
| <a name="input_vpc_id"></a> [vpc\_id](#input\_vpc\_id) | The ID of the VPC that the instance security group belongs to.                                                                                                                                                                                                              | `string` | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_prefix_list_id"></a> [prefix\_list\_id](#output\_prefix\_list\_id) | The ID of the prefix list. |
| <a name="output_security_group_arn"></a> [security\_group\_arn](#output\_security\_group\_arn) | IDs on the AWS Security Groups associated with the instance. |
| <a name="output_security_group_id"></a> [security\_group\_id](#output\_security\_group\_id) | IDs on the AWS Security Groups associated with the instance. |
| <a name="output_security_group_tags"></a> [security\_group\_tags](#output\_security\_group\_tags) | A mapping of public tags to assign to the resource. |

### 💙 Maintained by Rahul Yadav

CEO & CTO at [PrimeOps Technologies](https://primeops.co.in/)  
Helping teams build stable, scalable and consistent cloud and DevOps infrastructure.