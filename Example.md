```terraform
https://www.terraform.io/docs/cli/install/apt.html
https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html

https://registry.terraform.io/providers/hashicorp/aws/latest/docs

provider "aws" {
  
}

provider "aws" {
  region     = "us-east-1"
  access_key = "AKIA26GCGWIR6P564TBJ"
  secret_key = "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"
}




provider "aws" {
  region     = "us-east-1"
  access_key = "AKIA26GCGWIR6P564TBJ"
  secret_key = "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"
}

variable "firststring" {
   type = string
   default= "This is string value1"

}


output "firstoutput" {
   value = "${var.firststring}"
}

variable "map1"{
   type = map
   default = {
    "us-east"= "ami1"
    "eueast"="ami2"
   }
}

output "mapoutput" {
  value = "${var.map1["us-east"]}"
}



--------Example 2

provider "aws" {
  region     = "ap-south-1"
  access_key = "AKIA26GCGWIRYBK322ER"
  secret_key = "ZoqSe3WX8ZJ+ucgzdg0Cv8XaSufZF/qNIW0k4BOu"
}

variable "mysecuritylist"{
  type = list
  default = ["sg1", "sg2", "sg3"]
}

output "sgout" {
  value = "${ var.mysecuritylist[0]}"
}



-----Example 3

provider "aws" {
  region     = "ap-south-1"
  access_key = "AKIA26GCGWIRYBK322ER"
  secret_key = "ZoqSe3WX8ZJ+ucgzdg0Cv8XaSufZF/qNIW0k4BOu"
}

variable "boolvar"{
  type = bool
  default = true
}

output "out"{

value = "${var.boolvar}"
}


----Example 4--Create Ec2 instance

 1.tf

variable "aws_region" {
  description = "The AWS region to create things in."
  default     = "ap-south-1"
}

variable "key_name" {
  description = " SSH keys to connect to ec2 instance"
  default     =  "intelli"
}

variable "instance_type" {
  description = "instance type for ec2"
  default     =  "t2.micro"
}


main.tf

provider "aws" {
  region = var.aws_region
}


#Create security group with firewall rules
resource "aws_security_group" "security_jenkins_port" {
  name        = "security_jenkins_port"
  description = "security group for jenkins"

  ingress {
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

 ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

 # outbound from jenkis server
  egress {
    from_port   = 0
    to_port     = 65535
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags= {
    Name = "security_jenkins_port"
  }
}

resource "aws_instance" "myFirstInstance" {
  ami           = "ami-04bde106886a53080"
  key_name = var.key_name
  instance_type = var.instance_type
  security_groups= [ "security_jenkins_port"]
  tags= {
    Name = "jenkins_instance"
  }
}



----------Example S3 Bucket

resource "aws_s3_bucket" "b" {
  bucket = "my-tf-test-bucket321"
  acl    = "private"

  tags = {
    Name        = "My bucket"
    Environment = "Dev"
  }
}




```
