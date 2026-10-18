---
id: reusable-module
title: Reusable Module Files
description: Define reusable Terraform module for provisioning Azure Static Website resources.
---

# Reusable Module Files

The **reusable Terraform module** defines the actual Azure resources (Resource Group + Storage Account) with variables, outputs, and provider constraints.  This module can be called from any root configuration, ensuring consistency and reusability.


## Step 1: Define Resources

This is the module’s `main.tf`, where we provision a **Resource Group**, **Storage Account**, and enable the **Static Website** feature.

**File: `main.tf`**

```hcl
# Provider Block
provider "azurerm" {
  features {}
  subscription_id = "e08f61ac-0811-430a-9fc0-02d628400b48"
}


# Random String Resource
resource "random_string" "myrandom" {
  length  = 6
  upper   = false
  special = false
  numeric = false
}

# Create Resource Group
resource "azurerm_resource_group" "resource_group" {
  name     = var.resource_group_name
  location = var.location
}

# Create Azure Storage account
resource "azurerm_storage_account" "storage_account" {
  name                = "${var.storage_account_name}${random_string.myrandom.id}"
  resource_group_name = azurerm_resource_group.resource_group.name

  location                 = var.location
  account_tier             = var.storage_account_tier
  account_replication_type = var.storage_account_replication_type
  account_kind             = var.storage_account_kind
} 

# Enable Static Website (new resource in azurerm v4+)
resource "azurerm_storage_account_static_website" "static_site" {
  storage_account_id = azurerm_storage_account.storage_account.id
  index_document     = var.static_website_index_document
  error_404_document = var.static_website_error_document 
}
```

## Step 2: Define Outputs

We expose key attributes like resource group details and storage account info so that the root configuration can access them.

**File: `outputs.tf`**

```hcl
# Output variable definitions
output "resource_group_id" {
  description = "resource group id"
  value       = azurerm_resource_group.resource_group.id
}
output "resource_group_name" {
  description = "The name of the resource group"
  value       = azurerm_resource_group.resource_group.name
}
output "resource_group_location" {
  description = "resource group location"
  value       = azurerm_resource_group.resource_group.location
}
output "storage_account_id" {
  description = "storage account id"
  value       = azurerm_storage_account.storage_account.id
}
output "storage_account_name" {
  description = "storage account name"
  value       = azurerm_storage_account.storage_account.name
}
```


## Step 3: Define Variables

The module accepts input variables so users can customize names, tiers, and configurations when calling it.

**File: `variables.tf`**

```hcl
# Input variable definitions
variable "location" {
  description = "The Azure Region in which all resources groups should be created."
  type        = string
}
variable "resource_group_name" {
  description = "The name of the resource group"
  type        = string
}
variable "storage_account_name" {
  description = "The name of the storage account"
  type        = string
}
variable "storage_account_tier" {
  description = "Storage Account Tier"
  type        = string
}
variable "storage_account_replication_type" {
  description = "Storage Account Replication Type"
  type        = string
}
variable "storage_account_kind" {
  description = "Storage Account Kind"
  type        = string
}
variable "static_website_index_document" {
  description = "static website index document"
  type        = string
}

variable "static_website_error_document" {
  description = "Static website error document"
  type        = string
  default     = "error.html" 
}
```


## Step 4: Define Provider Versions

Finally, we lock Terraform and provider versions to ensure compatibility.

**File: `versions.tf`**

```hcl
terraform {
  required_version = ">= 1.0.0"
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = ">= 4.0.0"
    }
    random = {
      source  = "hashicorp/random"
      version = ">= 3.0.0"
    }
  }
}
```