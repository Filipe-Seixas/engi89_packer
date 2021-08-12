# AWS Packer Task

## Installing Packer
```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install packer
packer --version
```

## Building an Image

### Creating dir and pkr file

```bash
mkdir packer_man
cd packer_man/
touch aws-ubuntu.pkr.hcl
sudo nano aws-ubuntu.pkr.hcl
```

### aws-ubuntu.pkr.hcl

```bash
{
  "variables": {
    "instance_size": "t2.micro",
    "ami_name": "eng89_filipe_ami_packer_app",
    "base_ami": "ami-046036047eac23dc9",
    "ssh_username": "ubuntu",
    "vpc_id": "vpc-09cd84dde23d9f6a5",
    "subnet_id": "subnet-04320aadc8fa3fac1"
  },
  "builders": [
  {
    "type": "amazon-ebs",
    "region": "eu-west-1",
    "source_ami": "{{user `base_ami`}}",
    "instance_type": "{{user `instance_size`}}",
    "ssh_username": "{{user `ssh_username`}}",
    "ami_name": "{{user `ami_name`}}",
    "ssh_pty" : "true",
    "vpc_id": "{{user `vpc_id`}}",
    "subnet_id": "{{user `subnet_id`}}",
    "tags": {
    "Name": "eng89_filipe_ami_packer_app",
    "BuiltBy": "Packer"
    }
  }
]

```

- Create AWS access and secret keys env variables:
```bash
sudo nano .bashrc
  export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY
  export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY
source .bashrc
```
