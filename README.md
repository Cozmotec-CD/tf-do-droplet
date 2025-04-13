

# tf-do-cozmotec-droplet
Provides a DigitalOcean Droplet resource. This can be used to create, modify, and delete Droplets.

## Examples

```hcl
terraform {
  required_providers {
    digitalocean = {
      source  = "digitalocean/digitalocean"
      version = "2.46.0"
    }
  }
}

provider "digitalocean" {}

locals {
  name        = "app"
  environment = "test"
  region      = "blr1"
}

##------------------------------------------------
## VPC module call
##------------------------------------------------
module "vpc" {
  source      = "terraform-do-modules/vpc/digitalocean"
  version     = "1.0.0"
  name        = local.name
  environment = local.environment
  region      = local.region
  ip_range    = "10.10.0.0/16"
}

##------------------------------------------------
## Droplet module call
##------------------------------------------------
module "droplet" {
  source      = "./../../"
  name        = local.name
  environment = local.environment
  region      = local.region
  vpc_uuid    = module.vpc.id
  user_data   = file("user-data.sh")
  ####firewall
  inbound_rules = [
    {
      allowed_ip    = ["10.10.0.0/16"]
      allowed_ports = "22"
    },
    {
      allowed_ip    = ["0.0.0.0/0"]
      allowed_ports = "80"
    }
  ]
}
```

## Providers

| Name | Version |
|------|---------|
| <a name="provider_digitalocean"></a> [digitalocean](#provider\_digitalocean) | 2.46.0 |
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_digitalocean"></a> [digitalocean](#requirement\_digitalocean) | 2.46.0 |
## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_labels"></a> [labels](#module\_labels) | github.com/Cozmotec-CD/tf-do-labels | n/a |
## Resources

| Name | Type |
|------|------|
| [digitalocean_droplet.main](https://registry.terraform.io/providers/digitalocean/digitalocean/2.46.0/docs/resources/droplet) | resource |
| [digitalocean_firewall.default](https://registry.terraform.io/providers/digitalocean/digitalocean/2.46.0/docs/resources/firewall) | resource |
| [digitalocean_reserved_ip.this](https://registry.terraform.io/providers/digitalocean/digitalocean/2.46.0/docs/resources/reserved_ip) | resource |
| [digitalocean_reserved_ip_assignment.ip_assignment](https://registry.terraform.io/providers/digitalocean/digitalocean/2.46.0/docs/resources/reserved_ip_assignment) | resource |
| [digitalocean_ssh_key.ssh_keys](https://registry.terraform.io/providers/digitalocean/digitalocean/2.46.0/docs/resources/ssh_key) | resource |
| [digitalocean_volume.main](https://registry.terraform.io/providers/digitalocean/digitalocean/2.46.0/docs/resources/volume) | resource |
| [digitalocean_volume_attachment.main](https://registry.terraform.io/providers/digitalocean/digitalocean/2.46.0/docs/resources/volume_attachment) | resource |
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_backups"></a> [backups](#input\_backups) | Boolean controlling if backups are made. Defaults to false. | `bool` | `false` | no |
| <a name="input_block_storage_filesystem_label"></a> [block\_storage\_filesystem\_label](#input\_block\_storage\_filesystem\_label) | Initial filesystem label for the block storage volume. | `string` | `"data"` | no |
| <a name="input_block_storage_filesystem_type"></a> [block\_storage\_filesystem\_type](#input\_block\_storage\_filesystem\_type) | Initial filesystem type (xfs or ext4) for the block storage volume. | `string` | `null` | no |
| <a name="input_block_storage_size"></a> [block\_storage\_size](#input\_block\_storage\_size) | (Required) The size of the block storage volume in GiB. If updated, can only be expanded. | `number` | `5` | no |
| <a name="input_droplet_agent"></a> [droplet\_agent](#input\_droplet\_agent) | A boolean indicating whether to install the DigitalOcean agent used for providing access to the Droplet web console in the control panel. By default, the agent is installed on new Droplets but installation errors (i.e. OS not supported) are ignored. To prevent it from being installed, set to false. To make installation errors fatal, explicitly set it to true. | `bool` | `false` | no |
| <a name="input_droplet_count"></a> [droplet\_count](#input\_droplet\_count) | The number of droplets / other resources to create | `number` | `1` | no |
| <a name="input_droplet_size"></a> [droplet\_size](#input\_droplet\_size) | the size slug of a droplet size | `string` | `"s-1vcpu-1gb"` | no |
| <a name="input_enable_firewall"></a> [enable\_firewall](#input\_enable\_firewall) | Enable default Security Group with only Egress traffic allowed. | `bool` | `true` | no |
| <a name="input_enabled"></a> [enabled](#input\_enabled) | Flag to control the droplet creation. | `bool` | `true` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| <a name="input_floating_ip"></a> [floating\_ip](#input\_floating\_ip) | (Optional) Boolean to control whether floating IPs should be created. | `bool` | `false` | no |
| <a name="input_graceful_shutdown"></a> [graceful\_shutdown](#input\_graceful\_shutdown) | A boolean indicating whether the droplet should be gracefully shut down before it is deleted. | `bool` | `false` | no |
| <a name="input_image_name"></a> [image\_name](#input\_image\_name) | The image name or slug to lookup. | `string` | `"ubuntu-22-04-x64"` | no |
| <a name="input_inbound_rules"></a> [inbound\_rules](#input\_inbound\_rules) | List of objects that represent the configuration of each inbound rule. | `any` | `[]` | no |
| <a name="input_ipv6"></a> [ipv6](#input\_ipv6) | (Optional) Boolean controlling if IPv6 is enabled. Defaults to false. | `bool` | `false` | no |
| <a name="input_key_name"></a> [key\_name](#input\_key\_name) | Name  (e.g. `it-admin` or `devops`). | `string` | `""` | no |
| <a name="input_key_path"></a> [key\_path](#input\_key\_path) | Name  (e.g. `~/.ssh/id_rsa.pub` or `ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD3F6tyPEFEzV0LX3X8BsXdMsQ`). | `string` | `""` | no |
| <a name="input_label_order"></a> [label\_order](#input\_label\_order) | Label order, e.g. `name`,`application`. | `list(any)` | <pre>[<br/>  "name",<br/>  "environment"<br/>]</pre> | no |
| <a name="input_managedby"></a> [managedby](#input\_managedby) | ManagedBy, eg 'terraform-do-modules' or 'hello@clouddrove.com' | `string` | `"terraform-do-modules"` | no |
| <a name="input_monitoring"></a> [monitoring](#input\_monitoring) | (Optional) Boolean controlling whether monitoring agent is installed. Defaults to false. | `bool` | `false` | no |
| <a name="input_name"></a> [name](#input\_name) | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| <a name="input_outbound_rule"></a> [outbound\_rule](#input\_outbound\_rule) | List of objects that represent the configuration of each outbound rule. | <pre>list(object({<br/>    protocol              = string<br/>    port_range            = string<br/>    destination_addresses = list(string)<br/>  }))</pre> | <pre>[<br/>  {<br/>    "destination_addresses": [<br/>      "0.0.0.0/0",<br/>      "::/0"<br/>    ],<br/>    "port_range": "1-65535",<br/>    "protocol": "tcp"<br/>  },<br/>  {<br/>    "destination_addresses": [<br/>      "0.0.0.0/0",<br/>      "::/0"<br/>    ],<br/>    "port_range": "1-65535",<br/>    "protocol": "udp"<br/>  }<br/>]</pre> | no |
| <a name="input_region"></a> [region](#input\_region) | The region to create VPC, like `blr1` | `string` | `"blr1"` | no |
| <a name="input_resize_disk"></a> [resize\_disk](#input\_resize\_disk) | (Optional) Boolean controlling whether to increase the disk size when resizing a Droplet. It defaults to true. When set to false, only the Droplet's RAM and CPU will be resized. Increasing a Droplet's disk size is a permanent change. Increasing only RAM and CPU is reversible. | `bool` | `true` | no |
| <a name="input_ssh_keys"></a> [ssh\_keys](#input\_ssh\_keys) | SSH keys to be created | <pre>map(object({<br/>    name       = optional(string)<br/>    public_key = optional(string)<br/>  }))</pre> | `{}` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | A list of the tags to be applied to this Droplet. | `list(any)` | `[]` | no |
| <a name="input_user_data"></a> [user\_data](#input\_user\_data) | (Optional) A string of the desired User Data for the Droplet. | `string` | `null` | no |
| <a name="input_vpc_uuid"></a> [vpc\_uuid](#input\_vpc\_uuid) | The ID of the VPC where the Droplet will be located. | `string` | `""` | no |
## Outputs

| Name | Description |
|------|-------------|
| <a name="output_disk"></a> [disk](#output\_disk) | The size of the instance's disk in GB. |
| <a name="output_id"></a> [id](#output\_id) | The ID of the Droplet. |
| <a name="output_ipv4_address"></a> [ipv4\_address](#output\_ipv4\_address) | The IPv4 address. |
| <a name="output_ipv4_address_private"></a> [ipv4\_address\_private](#output\_ipv4\_address\_private) | The private networking IPv4 address. |
| <a name="output_ipv6"></a> [ipv6](#output\_ipv6) | Is IPv6 enabled. |
| <a name="output_ipv6_address"></a> [ipv6\_address](#output\_ipv6\_address) | The IPv6 address. |
| <a name="output_locked"></a> [locked](#output\_locked) | Is the Droplet locked. |
| <a name="output_name"></a> [name](#output\_name) | The name of the Droplet. |
| <a name="output_price_hourly"></a> [price\_hourly](#output\_price\_hourly) | Droplet hourly price. |
| <a name="output_price_monthly"></a> [price\_monthly](#output\_price\_monthly) | Droplet hourly price. |
| <a name="output_public_ip_address"></a> [public\_ip\_address](#output\_public\_ip\_address) | The IP Address of the resource |
| <a name="output_region"></a> [region](#output\_region) | The region of the Droplet. |
| <a name="output_size"></a> [size](#output\_size) | The instance size. |
| <a name="output_ssh_keys"></a> [ssh\_keys](#output\_ssh\_keys) | SSH keys created in DigitalOcean |
| <a name="output_status"></a> [status](#output\_status) | The status of the Droplet. |
| <a name="output_tags"></a> [tags](#output\_tags) | The tags associated with the Droplet. |
| <a name="output_urn"></a> [urn](#output\_urn) | The uniform resource name of the Droplet. |
| <a name="output_vcpus"></a> [vcpus](#output\_vcpus) | The number of the instance's virtual CPUs. |