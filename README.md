# Simple blueprint example

This is a simple example to demonstrate the capabilities of IBM Cloud Schematics blueprints to deploy solutions using modules to link two Terraform configs. No costs are incurred by this example. Only Lite service plans are used. 


Following cloud resources are deployed or referenced:
- Resource Group
- COS instance and bucket

The user must have IAM access permissions to read resources in the Default resource group and create [Cloud Object Storage](https://test.cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-iam) instances. Also permissions to create Schematics [Workspaces and Blueprints](https://test.cloud.ibm.com/docs/schematics?topic=schematics-access). 


## Blueprint template - basic-blueprint.yaml

This blueprint demonstrates linking two Terraform configs as modules to reference the existing Default resource group and create Cloud Object Storage (COS) resources. 

The blueprint consists of two Modules:
- basic-resource-group
- basic-cos-storage

The TF configs used in this blueprint are sourced from the repo https://github.com/Cloud-Schematics/blueprint-example-modules/
```
Blueprint file: basic-blueprint.yaml
├── basic-resource-group
|    └── source: github.com/Cloud-Schematics/blueprint-example-modules/IBM-DefaultResourceGroup
└── basic-cos-storage
     └── source: github.com/Cloud-Schematics/blueprint-example-modules/IBM-Storage
```

### Blueprint template inputs 
The basic-blueprint.yaml template file accepts the following inputs:

| Name | Type | Value | Description |
|------|------|------|----------------|
| cos_instance_name | string | null | Name for COS instance |
| cos_storage_plan | string | null | Service plan type for COS instance |

### Blueprint outputs
The basic-blueprint.yaml template creates the following outputs:

| Name | Type | Value | Description |
|------|------|------|----------------|
| cos_id | string |  | ResourceID/CRN of COS instance |


## Blueprint input file basic-input.yaml
The input file defines the variable names and values for the required blueprint template inputs. 

| Name | Type | Value | Description |
|------|------|------|----------------|
| cos_instance_name | string | blueprint-basic  | Name for COS instance |
| cos_storage_plan | string | lite | Service plan type for COS instance |

### CLI input values
No inputs are required at create time


## Prerequisites
1. Install the Schematics CLI plugin by follow the instructions in the [documentation](https://cloud.ibm.com/docs/schematics?topic=schematics-setup-cli).
2. Configure [IAM access permissions](https://cloud.ibm.com/docs/schematics?topic=schematics-access) for the Schematics blueprints service. 
3. Configure [Cloud Object Storage IAM permissions](https://test.cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-iam) to create COS instances.
4. Set Schematics Target Region. The region can be set with the `ibmcloud target -r` command. The Schematics blueprint instance is determined by the IBM Cloud CLI target region.


## Usage 
To work with the broadest range of accounts and scenarios, the example command input here requires the minimum rights to create new resources. It assumes that the user has access to the default resource group, has been granted Schematics access permissions and COS access permissions to this group. 


The following parameters are used for the `blueprint config create` configuration. 
- Name of the blueprint: `blueprint_basic`
- Schematics management resource group: \<default-resource-group-name\>
- Blueprint URL: `https://github.com/Cloud-Schematics/blueprint-basic-example`
- Blueprint file: `basic-blueprint.yaml`
- Input file URL: `https://github.com/Cloud-Schematics/blueprint-basic-example`
- Input file: `basic-input.yaml` 

The name of default resource group for the account is retrieved with the command:
```
ibmcloud resource groups --default

Retrieving default resource group under account 12345678901234567890123456789012 as anon@anon.com...
OK
Name      ID                                 Default Group   State
Default   aac37f57b20142dba1a435c70aeb12df   true            ACTIVE
```



```
$ ibmcloud target -r <region>

$ ibmcloud resource groups --default

$ ibmcloud schematics blueprint config create -name blueprint_Basic -resource-group <default-resource-group-name> -bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-file basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-file basic-input.yaml 

$ ibmcloud schematics blueprint run apply -id blueprint_id

$ ibmcloud schematics blueprint job list -id blueprint_id

$ ibmcloud schematics blueprint get -id blueprint_id -profile outputs

$ ibmcloud schematics blueprint run destroy -id blueprint_id

$ ibmcloud schematics blueprint config delete -id blueprint_id
```

Refer to the [Schematics FAQ](https://cloud.ibm.com/docs/schematics?topic=schematics-blueprints-faq&interface=ui#faqs-bp-basic-example) documentation for diagnosing and resolving the typical configuration errors with this example and their resolution.

## Next Steps

Looking for more samples? Check out the [IBM Cloud Schematics GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint). 



