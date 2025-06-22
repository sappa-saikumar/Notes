# Terraform Overview

## What's Terraform?

Terraform is the tool developed by HashiCorp that allows users to define infrastructure using a declarative configuration language (HCL).

---

## Terraform init

This command performs different initialization steps in order to prepare the current working directory for use with Terraform.

---

## Terraform plan

1. **Reads the current state** – Checks the existing infrastructure to ensure Terraform's state is up to date.
2. **Compares configurations** – Analyzes the differences between your Terraform configuration and the current state.
3. **Proposes changes** – Generates an execution plan showing what will be **added, modified, or destroyed**.
4. **Does not apply changes** – Only provides a preview; no actual modifications are made.

---

## Terraform apply

Executes the proposed changes and applies them to your infrastructure.

---

## Terraform Variables (Why do we need this? - Reusability)

- **Input Variables:** Serve as a parameter for a Terraform module, so users can customize the behaviour without editing the source.
- **Output Values:** Are like return values for a Terraform module.
- **Local Values:** Are a convenience feature for assigning a short name to an expression.

### Variable Types

- `bool`
- `string`
- `number`
- `list(string)`
- `list(list)`
- `list(map)`
- `set`
- `map(string)`

- You can add validation conditions as well (Need to understand more).
- Setting a variable as **"Ephemeral"**.
- Setting a variable as **Sensitive**:
  - Generally, Terraform will print out on the console the changes that are going to apply on the resource while performing `terraform plan`, `terraform destroy`, and `terraform apply`. So, if you make a Terraform variable as `Sensitive=true` then that variable will not print on the console.

---

## Assigning a value to a variable

Terraform loads all the files in the current directory with the exact name `terraform.tfvars` or `terraform.json` or `.auto.tfvars.json` or `*.auto.tfvars`.  
You can also use the `-var-file` flags to specify other files by name.

Terraform supports interpolation of strings:

```hcl
name = "web-sg-project-alpha-dev"
name = "web-sg-${var.resource_tags["project"]}-${var.resource_tags["environment"]}"
```

---

## Variable Definition of Precedence

Terraform loads variables in the following order, with later sources taking precedence over earlier ones:

1. Environment Variables
2. `terraform.tfvars`, if present
3. `terraform.tfvars.json`, if present
4. Any `*.auto.tfvars` or `*.auto.tfvars.json` files, processed in lexical order of their filenames.
5. Any `-var` and `-var-file` options on the command line, in the order they are provided. (This includes variables set by an HCP Terraform workspace.)

If one variable is assigned with multiple values then Terraform uses the last value it finds.

---

## Terraform local values

A local value assigns a name to an expression so that the same name can be used multiple times within a module instead of repeating the expression.