---
id: root-config
title: Root Configuration Files
description: Define Terraform root-level configuration, providers, module calls, and outputs.
---

# Root Configuration Files

The **root configuration** is responsible for setting up the Terraform environment, calling the reusable module, and exposing outputs.  


## Step 1: Define Providers and Terraform Version

We start by declaring the **Terraform version** and required **providers**. This ensures compatibility and defines the Azure provider we will use.

**File: `versions.tf`**

```hcl
# Terraform Block
terraform {
  required_version = ">= 1.0.0"
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = ">= 4.0.0"
    }
  }
}

# Provider Block
provider "azurerm" {
  features {}
}
```


## Step 2: Call the Custom Terraform Module

Next, we invoke our custom Terraform module that provisions the Azure Static Website resources. This is the root main.tf which references the module we created earlier.

**File: `main.tf`**

```hcl
module "azure_static_website" {
  source = "./modules/azure-static-website"

  # Resource Group
  location            = "eastus"
  resource_group_name = "myrg1"

  # Storage Account
  storage_account_name             = "staticwebsite"
  storage_account_tier             = "Standard"
  storage_account_replication_type = "LRS"
  storage_account_kind             = "StorageV2"
  static_website_index_document    = "index.html"
  static_website_error_document    = "error.html"
}
```

## Step 3: Define Outputs

Finally, we declare output variables to make important information (like resource IDs, names, and endpoints) easily accessible after deployment.

**File: `outputs.tf`**

```hcl
# Output variable definitions
output "root_resource_group_id" {
  description = "resource group id"
  value       = module.azure_static_website.resource_group_id
}
output "root_resource_group_name" {
  description = "The name of the resource group"
  value       = module.azure_static_website.resource_group_name
}
output "root_resource_group_location" {
  description = "resource group location"
  value       = module.azure_static_website.resource_group_location
}
output "root_storage_account_id" {
  description = "storage account id"
  value       = module.azure_static_website.storage_account_id
}
output "root_storage_account_name" {
  description = "storage account name"
  value       = module.azure_static_website.storage_account_name
}
```