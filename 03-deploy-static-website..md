---
id: deploy-static-website
title: Deploy Static Website with Terraform
description: Step-by-step guide to initialize, deploy, verify, and clean up the Azure Static Website using Terraform.
---

# Deploy Static Website Using Terraform

This section walks through how to deploy the static website using **Terraform commands**, upload content, verify the deployment, and destroy resources.


## Initialize and Apply Terraform

Run the following commands inside your project directory:

```bash
# Initialize Terraform (downloads providers and sets up backend)
terraform init

# Validate configuration (checks syntax and correctness)
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Apply changes (provisions Azure resources)
terraform apply -auto-approve
```

![Terraform commands](./img/tf%20cmd.PNG)

## Upload Static Content

After provisioning the infrastructure:

1. Open the **Azure Portal**.  

2. Navigate to the **Storage Account** created by Terraform.  

![Storage Account](./img/storage%20account.PNG)

3. Under **Data storage → Containers**, open the `$web` container (auto-created by the `static_website` feature).  

![Container](./img/container.PNG)

4. Upload your static site files:  
   - `error.html`
   - `index.html` 

![Upload](./img/web.PNG)


## Verify Deployment

1. To access the website, go to the **Storage Account**, navigate to the **Data Management** blade and click on **Static Website**. 

![Endpoint](./img/endpoint.PNG)

2. Copy the **Primary endpoint** URL.  

`https://staticwebsitealrurv.z13.web.core.windows.net/`

To confirm the error page:  
`https://staticwebsitealrurv.z13.web.core.windows.net/error`

3. Paste the endpoint into your browser, you should see **`index.html`** and **`error.html`** render.  

![Static page](./img/static%20page.PNG)

![Error Page](./img/error%20page.PNG)


## Destroy and Clean-Up

When you are done, remove the resources and Terraform state:

```bash
# Destroy all resources created by Terraform
terraform destroy -auto-approve

# Delete Terraform state files (.terraform/ directory, .terraform.lock.hcl file, and terraform.tfstate* files)
rm -rf .terraform* terraform.tfstate* 
OR
rm -Recurse -Force .terraform*, terraform.tfstate*

```


