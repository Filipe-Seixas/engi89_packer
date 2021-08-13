# AWS Packer Task

- Packer is a tool that allows us to create AMIs automatically from a JSON or hcl file.

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

- This creates a brand new AMI from a default ubuntu AMI

```bash
packer {
  required_plugins {
    amazon = {
      version = ">= 0.0.2"
      source  = "github.com/hashicorp/amazon"
    }
  }
}

source "amazon-ebs" "ubuntu" {
  ami_name             = "eng89-filipe-packer"
  instance_type        = "t2.micro"
  region               = "eu-west-1"
  source_ami_filter {
    filters = {
      name                = "ubuntu/images/*ubuntu-xenial-16.04-amd64-server-*"
      root-device-type    = "ebs"
      virtualization-type = "hvm"
    }
    most_recent = true
    owners      = ["099720109477"]
  }
  ssh_username = "ubuntu"
}

build {
  name = "eng89-filipe-packer"
  sources = [
    "source.amazon-ebs.ubuntu"
  ]
}
```

- Create AWS access and secret keys env variables:
```bash
sudo nano .bashrc
  export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY
  export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY
source .bashrc
```

- Run commands to validate and build the AMI
```bash
packer init
packer fmt .
packer validate .
packer build aws-ubuntu.pkr.hcl
```