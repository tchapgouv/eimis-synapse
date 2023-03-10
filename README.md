# Synapse on Kubernetes
Make an Synapse/Matrix server work on a managed kubernetes server hosted by OVH

The Matrix-synapse stack is based on the work done by [Alexander Olofsson](https://gitlab.com/ananace) : 
https://gitlab.com/ananace/charts/-/tree/master/charts/matrix-synapse

## Prerequisites

- an account in OVH hosting provider and its credentials 
(application key, application secret, consumer secret and endpoint)
- to store Terraform state files : an S3 object storage with the credentials to connect to 
(access key, secret key, endpoint and region) 
- a valid domain name to reach the future synapse homeserver

## Provisioning
- Create a local.env.sh file copying the script/local.env.template.sh file 
and fill it with all the environment variables values needed.   

    Then source this file : 
    ```bash
    source local.env.sh
    ```
- Generate a terraform.tfvars file based on values previouly set : 
    ```bash
    sh scripts/generate_tfvars_file.sh
    ```
- Initialize the Terraform workspace
    ```bash
    terraform init
    ```
- Create the Terraform execution plan to validate that everything is ok
    ```bash
    terraform plan
    ```
- Apply the Terraform plan
    ```bash
    terraform apply
    ```
  
This will lead to the creation of a kubernetes cluster with:
- 1 control plane node
- 2 workers nodes
the kubeconfig file needed to connect to the cluster can be generated with : 
```bash
sh scripts/generate_kubeconfig_file.sh
```

## Configuration
The configuration part will be done with Ansible and is quite independant
from the provisioning part.  
For this you just have to execute : 
```bash
sh scripts/ansible_configuration.sh
```