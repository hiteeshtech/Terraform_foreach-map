# Terraform_foreach-map

In this when when we have to create the resource group to the desired location only then we will use the concept of 
each.key
each.value

resource "azurerm_resource_group" "rg" {
  for_each = var.rg_name
  name     = each.key
  location = each.value
}

rg_name = {
  "hiteshrg" = "East US"
  "pooranrg" = "West US"
  "jamalrg"  = "East US"
  "rakeshrg" = "Central US"
  "amitrg"   = "Centralindia"
}

then the output came is given below 

PS D:\hiteesh_terraform\Terraform_foreach-map> terraform plan 

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_resource_group.rg["amitrg"] will be created
  + resource "azurerm_resource_group" "rg" {
      + id       = (known after apply)
      + location = "centralindia"
      + name     = "amitrg"
    }

  # azurerm_resource_group.rg["hiteshrg"] will be created
  + resource "azurerm_resource_group" "rg" {
      + id       = (known after apply)
      + location = "eastus"
      + name     = "hiteshrg"
    }

  # azurerm_resource_group.rg["jamalrg"] will be created
  + resource "azurerm_resource_group" "rg" {
      + id       = (known after apply)
      + name     = "jamalrg"
    }

  # azurerm_resource_group.rg["pooranrg"] will be created
  + resource "azurerm_resource_group" "rg" {
      + id       = (known after apply)
      + location = "westus"
      + name     = "pooranrg"
    }

  # azurerm_resource_group.rg["rakeshrg"] will be created
  + resource "azurerm_resource_group" "rg" {
      + id       = (known after apply)
      + location = "centralus"
      + name     = "rakeshrg"
    }


────────────────────────────────────────────────────────────────────


now we will do some experiment in this now if I am delete one resource group in it then see what happens 

1. If I am delete jamal rg then see what happens 
rg_name = {
  "hiteshrg" = "East US"
  "pooranrg" = "West US"
  
  "rakeshrg" = "Central US"
  "amitrg"   = "Centralindia"
}

PS D:\hiteesh_terraform\Terraform_foreach-map> terraform plan 
azurerm_resource_group.rg["rakeshrg"]: Refreshing state... [id=/subscriptions/f96f2e3b-2cc8-4e5e-8ed9-4afb664bb5a8/resourceGroups/rakeshrg]
azurerm_resource_group.rg["pooranrg"]: Refreshing state... [id=/subscriptions/f96f2e3b-2cc8-4e5e-8ed9-4afb664bb5a8/resourceGroups/pooranrg]
azurerm_resource_group.rg["hiteshrg"]: Refreshing state... [id=/subscriptions/f96f2e3b-2cc8-4e5e-8ed9-4afb664bb5a8/resourceGroups/hiteshrg]
azurerm_resource_group.rg["jamalrg"]: Refreshing state... [id=/subscriptions/f96f2e3b-2cc8-4e5e-8ed9-4afb664bb5a8/resourceGroups/jamalrg]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # azurerm_resource_group.rg["jamalrg"] will be destroyed
  # (because key ["jamalrg"] is not in for_each map)
  - resource "azurerm_resource_group" "rg" {
      - id         = "/subscriptions/f96f2e3b-2cc8-4e5e-8ed9-4afb664bb5a8/resourceGroups/jamalrg" -> null
      - location   = "eastus" -> null
      - name       = "jamalrg" -> null
        # (1 unchanged attribute hidden)
    }

Plan: 0 to add, 0 to change, 1 to destroy.

Then only jamal rg is destroyed with its location 