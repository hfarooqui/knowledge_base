## Terraform

Terraform v0.11.11

- Wrapper (Cluster generator) for templating terraform
- Wrapper converts *.erb templates to *.tf

### Commands
terraform init
terraform plan
terraform apply --auto-approve
terraform state list | grep aws_instance | xargs -n 1 terraform taint" # Taint all aws_instances
terraform state show aws_instance.jump_host1 | awk '/public_dns/{print $NF}' # Get specific field from node state
terraform destroy --force

Terraform is logically split into two main parts:
- Terraform Core 
- Terraform Plugins. Each plugin exposes an implementation for a specific service, such as the AWS provider or the cloud-init provider. Terraform Plugins are written in Go and are executable binaries, executed as a separate process and communicate with the main Terraform binary over an RPC interface. The network communication and RPC is handled automatically by higher-level Terraform libraries, so developers need only worry about the implementation of their specific Plugin behavior.

The Terraform Plugin SDK currently supports one type of plugin: providers.

### Providers
- Providers are the most common type of Plugin, which expose the features that a specific service offers via its APIs
- Providers define Resources and are responsible for managing their life cycles. 
- E.g. Providers: "Amazon Web Service Provider" or the "Google Cloud Provider"
- E.g. Resources: "aws_instance" and "google_compute_instance"
- Terraform Providers contain all the code needed to authenticate and connect to a service on behalf of the user. 
- Each Resource implements CREATE, READ, UPDATE, and DELETE (CRUD) methods to manage itself
- Terraform Core manages a Resource Graph of all the resources declared in the configuration as well as their current state. 
- Resources remain ignorant of the current state, only responding to method calls from Terraform Core and performing the matching CRUD action.
- Terraform determines the Providers needed by reading and interpolating configuration files. 
- Terraform will dynamically discover and fetch the needed providers from releases.hashicorp.com, or from specific locations on disk.
- Terraform configurations must declare which providers they require, so that Terraform can install and use them. Additionally, some providers require configuration (like endpoint URLs or cloud regions) before they can be used.
--

```
provider "aws" {
  region = "us-east-1"
  profile = "default"
  alias = "dns"
}
```

### State

Terraform must store state about your managed infrastructure and configuration. This state is used by Terraform to map real world resources to your configuration, keep track of metadata, and to improve performance for large infrastructures.

This state is stored by default in a local file named "terraform.tfstate", but it can also be stored remotely, which works better in a team environment.

- "terraform show" - command is used to provide human-readable output from a state or plan file. This can be used to inspect a plan to ensure that the planned operations are expected, or to inspect the current state as Terraform sees it.
```
terraform show -json
```

- "terraform output" - command is used to extract the value of an output variable from the state file.
```
These examples assume the following Terraform output snippet.

output "lb_address" {
  value = "${aws_alb.web.public_dns}"
}

output "instance_ips" {
  value = ["${aws_instance.web.*.public_ip}"]
}

output "password" {
  sensitive = true
  value = ["${var.secret_password}"]
}
```
```
$ terraform output instance_ips
test = [
    54.43.114.12,
    52.122.13.4,
    52.4.116.53
]
```
```
$ terraform output -json instance_ips | jq '.value[0]'
```

### Provisioners
- Provisioners can be used to model specific actions on the local machine or on a remote machine in order to prepare servers or other infrastructure objects for service.
- Provisioners are a Last Resort
- Terraform includes the concept of provisioners as a measure of pragmatism (an approach that assesses the truth of meaning of theories or beliefs in terms of the success of their practical application.), knowing that there will always be certain behaviors that can't be directly represented in Terraform's declarative model.

```
  provisioner "file" {
      source = "resources/enable_swap.sh"
      destination = "/tmp/enable_swap.sh"
  }

  provisioner "remote-exec" {
    inline = [
      "chmod +x /tmp/enable_swap.sh",
      "sudo /tmp/enable_swap.sh",
    ]
  }
  
  provisioner "chef" {
    environment     = "standard_chef_environment"
    attributes_json = <<EOF
{
  "cassandra": {
    "cluster_name": "MobileIron Developer Cluster",
    "seeds": "${aws_instance.cassandra1.private_ip}",
    "listen_address": "${aws_instance.cassandra1.private_ip}",
    "rpc_address": "${aws_instance.cassandra1.private_ip}",
    "datacenter": "datacenter1",
    "rack": "rack1",
    "create_keyspace": true,
    "replication_factor": "2",
    "auth_replication_factor": "2"
  }
}

EOF
    
    run_list        = ["recipe[role_cassandra@69.0.0]"]
    
    secret_key      = "${file("~/.chef/encrypted_data_bag_secret")}"
    node_name       = "cassandra1.hf68.gamma.mobileiron.net"
    server_url      = "https://chefns1.aries.mobileiron.net/organizations/hfarooqui"
    recreate_client = true
    user_name       = "hfarooqui"
    user_key        = "${file("~/.chef/hfarooqui.pem")}"
    skip_install    = true
    disable_reporting = true
  }

  provisioner "local-exec" {
    command = "knife node delete cassandra1.hf68.gamma.mobileiron.net -y -c resources/knife.rb || true"
    when = "destroy"
  }
  
}
```
### Resources
Resources are the most important element in the Terraform language. Each resource block describes one or more infrastructure objects, such as:
- virtual networks (aws_vpc, aws_network_interface)
- compute instances (aws_instance)
- DNS records (aws_route53_record)

```
resource "aws_security_group_rule" "jump_host_ingress_1" {
    # Creates AWS security group for specified type (ingress/egress), cidr_blocks, from_port, from_port
}

resource "aws_route53_record" "primary_hf68_gamma_mobileiron_net" {
    # Creates route53 record for specified zone_id, type (A, CNAME), ttl and records (loadbalancer_ip)
}

resource "aws_vpc" "mi_vpc_1" {
    # Creates vpc for specified cidr_block and associate tags to it
}

resource "aws_internet_gateway" "net_gateway" {
    # Creates internet_gateway for specified vpc_id and associate tags to it
}

resource "aws_network_interface" "activemq1" {
  # Creates subent and associate security-groups, tags to it
}

resource "null_resource" "activemq1_provisioner" {
    # Not tied to any resources. Typically used with provisioners as "provisioner chef"
}
```
target directory:
- terraform.tfstate
- nodes.tf
```
resource "aws_network_interface" "activemq1" {
  subnet_id = "${aws_subnet.data_0.id}"
  description = "activemq1"
  depends_on = []
  security_groups = ["${aws_security_group.activemq.id}","${aws_security_group.common.id}"]
  tags {
    Name        = "hf68_activemq1"
    Role        = "AMQ"
    Owner       = "hfarooqui@mobileiron.com"
    Project     = "micp"
    Env         = "qa"
    Component   = "nightsky"
    Team        = "agarcia_404"
    Downtime    = "off"
  }
}

resource "aws_instance" "activemq1" {
  ami                         = "ami-032cb20e2fd46895e"
  instance_type               = "t2.small"
  key_name                    = "gamma"
  disable_api_termination     = false
  depends_on                  = ["aws_instance.db_manager1"]
  availability_zone           = "us-east-1a"
  user_data                   = "#!/bin/bash\nntpdate 169.254.169.123"
  network_interface {
    device_index = 0
    network_interface_id = "${aws_network_interface.activemq1.id}"
  }

  root_block_device {
    volume_type = "gp2"
    volume_size = "80"
    delete_on_termination = true
  }
  tags {
    Name        = "hf68_activemq1"
    Role        = "AMQ"
    Owner       = "hfarooqui@mobileiron.com"
    Project     = "micp"
    Env         = "qa"
    Component   = "nightsky"
    Team        = "agarcia_404"
    Downtime    = "off"
  }
  volume_tags {
    Name        = "hf68_activemq1"
    Role        = "AMQ"
    Owner       = "hfarooqui@mobileiron.com"
    Project     = "micp"
    Env         = "qa"
    Component   = "nightsky"
    Team        = "agarcia_404"
    Downtime    = "off"
  }
  connection {
    type                = "ssh"
    host                = "${aws_instance.activemq1.private_ip}"
    bastion_host        = "${aws_instance.jump_host1.public_ip}"
    bastion_user        = "operations"
    bastion_private_key = "${file("/Users/hfarooqui/projects/gamma")}"
    user                = "operations"
    private_key         = "${file("/Users/hfarooqui/projects/gamma")}"
  }
  lifecycle {
    ignore_changes = [
      "tags"
    ]
  }
}


resource "null_resource" "activemq1_provisioner" {
  depends_on = ["null_resource.db_manager1_provisioner"]
  triggers {
    version = "69.0.0"
    instance_id = "${aws_instance.activemq1.id}"
    
    run_list = "[\"recipe[role_activemq@69.0.0]\"]"
    
  }

  provisioner "chef" {
    environment     = "standard_chef_environment"
    attributes_json = <<EOF
    {   
        "mi": {
            "ssl_certs": ["gamma-cert.pem", "gamma-key.pem"],
            "activemq_ha": false
        },
        "amq": {
            "tuning": { "heap_size": 1024},
            "cluster_fqdn": "hf68.gamma.mobileiron.net",
            "server_key": "gamma-key.pem",
            "server_cert": "gamma-cert.pem"
        }
    }
EOF
    run_list        = ["recipe[role_activemq@69.0.0]"]
    secret_key      = "${file("~/.chef/encrypted_data_bag_secret")}"
    node_name       = "activemq1.hf68.gamma.mobileiron.net"
    server_url      = "https://chefns1.aries.mobileiron.net/organizations/hfarooqui"
    recreate_client = true
    user_name       = "hfarooqui"
    user_key        = "${file("~/.chef/hfarooqui.pem")}"
    skip_install    = true
    disable_reporting = true
  }
  provisioner "local-exec" {
    command = "knife node delete activemq1.hf68.gamma.mobileiron.net -y -c resources/knife.rb || true"
    when = "destroy"
  }
  provisioner "local-exec" {
      command = "bash -x resources/connection_check.sh default us-east-1 ${aws_instance.activemq1.id} ${aws_instance.jump_host1.public_ip} /Users/hfarooqui/projects/gamma"
  }
  provisioner "file" {
      source = "resources/enable_swap.sh"
      destination = "/tmp/enable_swap.sh"
  }
  provisioner "remote-exec" {
    inline = [
      "chmod +x /tmp/enable_swap.sh",
      "sudo /tmp/enable_swap.sh",
    ]
  }

  connection {
    type                = "ssh"
    host                = "${aws_instance.activemq1.private_ip}"
    bastion_host        = "${aws_instance.jump_host1.public_ip}"
    bastion_user        = "operations"
    bastion_private_key = "${file("/Users/hfarooqui/projects/gamma")}"
    user                = "operations"
    private_key         = "${file("/Users/hfarooqui/projects/gamma")}"
  }

}
```

./cluster -o overrides.yml -a create  -t -q

### Provisioning

qe_cluster_config.yml:  defines nodetype, security groups, chef-roles for each node
```
default:
  - name: activemq1
    depends_on:
      - db_manager1
    instance_type: t2.small
    role_id: AMQ
    run_list:
      - role_activemq
    security_groups:
      - activemq
      - common
    tier: data
    volume_size: 80
    zone: 0
    tag: "AMQ1"
    heap_size: 1024
```

std_security_groups.yml: defined ingress and egress rules
```
security_groups:
  common:
    ingress:
      tcp:
        jump_host: [22]
      udp:
        0.0.0.0/0: [123]
      icmp:
        0.0.0.0/0: [-1]
    egress:
      tcp:
        0.0.0.0/0: [80, 443, 2525, 1984, 8883]
        10.8.1.10/32: [25]
      udp:
        0.0.0.0/0: [123]

  activemq:
    ingress:
      tcp:
        lb: [8884, 61616]
        activemq: [61616]
        10.96.0.0/16: [60000, 8883, 8884]
    egress:
      tcp:
        db: [5432]
        activemq: [61616]
```

spec_generator.rb
 - create_json


 ## Packer
Packer is an automated build system to manage the creation of images for containers and virtual machines. It outputs an image that you can then take and run on the platform you require.

For v1.1 this includes - Alicloud ECS, Amazon EC2, Azure, CloudStack, DigitalOcean, Docker, Google Cloud, Hyper-V, LXC, LXD, 1&1, OpenStack, Oracle OCI, Parallels, ProfitBricks, QEMU, Triton, VirtualBox, VMware

Amazon EC2 AMI builder that ships with Packer builds an EBS-backed AMI by launching a source AMI, provisioning on top of that, and re-packaging it into a new AMI.
E.g. Create redis-server AMI basesd on ubuntu
```
{
  "variables": {
    "aws_access_key": "",
    "aws_secret_key": "",
    "aws_secret_key": ""
  },
  "builders": [
    {
      "type": "amazon-ebs",
      "access_key": "{{user `aws_access_key`}}",
      "secret_key": "{{user `aws_secret_key`}}",
      "region": "{{user `aws_region`}}",
      "source_ami_filter": {
        "filters": {
          "virtualization-type": "hvm",
          "name": "ubuntu/images/*ubuntu-xenial-16.04-amd64-server-*",
          "root-device-type": "ebs"
        },
        "owners": ["099720109477"],
        "most_recent": true
      },
      "instance_type": "t2.micro",
      "ssh_username": "ubuntu",
      "ami_name": "packer-example {{timestamp}}"
    }
  ]
  "provisioners": [
    {
      "type": "shell",
      "inline": [
        "sleep 30",
        "sudo apt-get update",
        "sudo apt-get install -y redis-server"
      ]
    }
  ]
}
```

```
$ packer validate example.json
Template validated successfully.
```

```
$ packer build \
    -var 'aws_access_key=YOUR ACCESS KEY' \
    -var 'aws_secret_key=YOUR SECRET KEY' \
    example.json

==> amazon-ebs: amazon-ebs output will be in this color.

==> amazon-ebs: Creating temporary keypair for this instance...
==> amazon-ebs: Creating temporary security group for this instance...
==> amazon-ebs: Authorizing SSH access on the temporary security group...
==> amazon-ebs: Launching a source AWS instance...
==> amazon-ebs: Waiting for instance to become ready...
==> amazon-ebs: Connecting to the instance via SSH...
==> amazon-ebs: Stopping the source instance...
==> amazon-ebs: Waiting for the instance to stop...
==> amazon-ebs: Creating the AMI: packer-example 1371856345
==> amazon-ebs: AMI: ami-19601070
==> amazon-ebs: Waiting for AMI to become ready...
==> amazon-ebs: Terminating the source AWS instance...
==> amazon-ebs: Deleting temporary security group...
==> amazon-ebs: Deleting temporary keypair...
==> amazon-ebs: Build finished.

==> Builds finished. The artifacts of successful builds are:
--> amazon-ebs: AMIs were created:

us-east-1: ami-19601070
```