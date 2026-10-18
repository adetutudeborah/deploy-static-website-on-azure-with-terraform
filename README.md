---
id: project-overview
title: Project Overview
description: Deploy a Static Website on Azure using Terraform with a reusable module approach.
---

# Project Overview

This project demonstrates how to deploy a **Static Website on Azure Storage** with **Terraform** using a **Reusable Terraform Module** approach. It provisions Azure resources, enables static website hosting, uploads static files, and makes the site accessible via a public endpoint.

## Table of Contents

1. [Project Overview](./README.md)  
2. [Root Configuration Files](./01-root-configuration-files.md)  
3. [Reusable Module Files](./02-reusable-module-files.md)  
4. [Deploy the Static Website](./03-deploy-static-website..md)  


## Aim

- Provision an Azure Resource Group and Storage Account using Terraform (via a reusable module)
- Enable the Static Website feature on the Storage Account
- Upload index.html and error.html into the $web container.
- Access the hosted static website via its primary endpoint.
- Destroy and clean up resources using Terraform when no longer needed.


## Prerequisites

- An **Azure Subscription**.
- **Terraform** (>= 3.0.0) installed and configured.
- **Azure CLI** installed (`az login` required).
- Basic knowledge of **Terraform modules**.
- A project folder containing:
  - `error.html` 
  - `index.html`  


## Terraform File Structure

```bash
├─ modules/
│   ├─ azure-static-website/
│   │   ├─ main.tf      # Module resources
│   │   ├─ variables.tf # Module variables
│   │   ├─ outputs.tf   # Module outputs
│   │   └─ versions.tf  # Provider & Terraform version constraints
├─ static-content/
│   ├─ error.html 
│   ├─ index.html
├─ main.tf              # Root configuration calling the module
├─ outputs.tf           # Root-level outputs
├─ versions.tf           
├─ variables.tf         # Root-level variables (optional)
└─ README.md
```

