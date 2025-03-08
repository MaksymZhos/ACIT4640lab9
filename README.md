# AWS NGINX Deployment with Packer and Terraform

## To deploy the AWS EC2 instance using the custom Nginx configurations, run the following commands for quick deployment:

1. Generate SSH keys
```bash
ssh-keygen -t ed25519 -f .ssh/aws-4640
```

2. Import public key to AWS
```bash
sh ./import_lab_key.sh .ssh/aws-4640.pub
```

3. Build the AMI
```bash
packer init packer/ansible-web.pkr.hcl
AWS_PROFILE="profile" packer build packer/

```

4. Create the EC2 instances
```bash
terraform init terraform/
terraform apply terraform/
```

5. At the end of the terraform execution, you should receive a DNS name and an IP address. Use either one to access the NGINX website:
```
http://<public_ip>
```
or
```
http://<dns_name>
```
![Screenshot_20250307_221857](https://github.com/user-attachments/assets/b48a6320-1e5f-4f04-95f1-7a455e9593eb)


## Project Components

### Packer Configuration

The Packer configuration builds an Ubuntu 24.04 AMI with NGINX installed using Ansible:
- Uses AWS and Ansible plugins
- Configures a t2.micro instance in us-west-2 region
- Applies the Ansible playbook to install and configure NGINX
- Names the resulting AMI "packer-ansible-nginx"

### Terraform Configuration

The Terraform configuration creates:
- A VPC with a public subnet in us-west-2a
- Internet gateway and route table
- Security groups allowing SSH and HTTP traffic
- EC2 instance using the custom AMI built by Packer

## Cleanup

When you're finished with the infrastructure, run:
```bash
terraform destroy
```

This will remove all AWS resources created by Terraform.
