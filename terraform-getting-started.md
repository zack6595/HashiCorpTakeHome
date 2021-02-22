# Getting Started with Terraform

Terraform is our tool for defining and provisioning infrastructure as code (IaC). Terraform enables you to build and manage your infrastructure in a safe, low-risk, and repeatable way.


## Prerequisites

- [Docker](https://www.docker.com/products/docker-desktop)

## Install Terraform

To install Terraform, please download the [proper package](https://www.terraform.io/downloads.html) for your system.

Next, unzip the Terraform binary and move it to a directory included in your system's [PATH](https://superuser.com/questions/284342/what-are-path-and-other-environment-variables-and-how-can-i-set-or-use-them). This process will differ depending on your operating system.

## Build Infrastructure

We recommend creating a new directory on your machine to build your Terraform configuration code.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Next, create a file for your Terraform configuration code. In Mac or Linux you can do so with the touch command.

```shell
$ touch main.tf
```

In Windows command line please use the pipe operation.

```shell
type nul > main.tf
```

Paste the following lines into the file.

```hcl

provider "docker" {
    host = "unix:///var/run/docker.sock"
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

## Initialize Terraform

Initialize Terraform with the `init` command. The docker provider will be installed.

```shell
$ terraform init
```
<details>
<summary>Click here for expected output</summary>

```
Initializing the backend...

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "docker" (terraform-providers/docker) 2.7.2...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.docker: version = "~> 2.7"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```
</details>
<br>

You shoud check for any errors. If you get a command not found error remember to move terraform binary to your [PATH](https://superuser.com/questions/284342/what-are-path-and-other-environment-variables-and-how-can-i-set-or-use-them).

## Apply Terraform

If init ran successfully, provision the resource with the `apply` command.

```shell
$ terraform apply
```

The command may take up to a few minutes to run.

<details>
<summary>Click here for expected output</summary>

```
docker_image.nginx: Refreshing state... [id=sha256:35c43ace9216212c0f0e546a65eec93fa9fc8e96b25880ee222b7ed2ca1d2151nginx:latest]
docker_container.nginx: Refreshing state... [id=6df0ffca88ebb5be423141a9f3f0c0d8fe8a27e6919bfd6ff34f5fedcb5d7f87]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach           = false
      + bridge           = (known after apply)
      + command          = (known after apply)
      + container_logs   = (known after apply)
      + dns              = (known after apply)
      + dns_opts         = (known after apply)
      + entrypoint       = (known after apply)
      + exit_code        = (known after apply)
      + gateway          = (known after apply)
      + hostname         = (known after apply)
      + id               = (known after apply)
      + image            = "sha256:35c43ace9216212c0f0e546a65eec93fa9fc8e96b25880ee222b7ed2ca1d2151"
      + ip_address       = (known after apply)
      + ip_prefix_length = (known after apply)
      + ipc_mode         = (known after apply)
      + log_driver       = (known after apply)
      + log_opts         = (known after apply)
      + logs             = false
      + must_run         = true
      + name             = "training"
      + network_data     = (known after apply)
      + read_only        = false
      + restart          = "no"
      + rm               = false
      + shm_size         = (known after apply)
      + start            = true
      + user             = (known after apply)
      + working_dir      = (known after apply)

      + ports {
          + external = 80
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value:
```
</details>
<br>


Next, type "yes" and hit ENTER. Terraform will create the NGINX webserver.

<details>
<summary>Click here for expected output</summary>

```
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 1s [id=5cf67155605bc1171a5c479757e46dc1cfc8921f167f908dd382295e15725372]


Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

</details>\

## Destroy Terraform

Finally, destroy the infrastructure.

```shell
$ terraform destroy
```

<details>
<summary>Click here for expected output</summary>

```
docker_image.nginx: Refreshing state... [id=sha256:35c43ace9216212c0f0e546a65eec93fa9fc8e96b25880ee222b7ed2ca1d2151nginx:latest]
docker_container.nginx: Refreshing state... [id=5cf67155605bc1171a5c479757e46dc1cfc8921f167f908dd382295e15725372]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed
  - resource "docker_container" "nginx" {
      - attach            = false -> null
      - command           = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - cpu_shares        = 0 -> null
      - dns               = [] -> null
      - dns_opts          = [] -> null
      - dns_search        = [] -> null
      - entrypoint        = [
          - "/docker-entrypoint.sh",
        ] -> null
      - gateway           = "172.17.0.1" -> null
      - group_add         = [] -> null
      - hostname          = "5cf67155605b" -> null
      - id                = "5cf67155605bc1171a5c479757e46dc1cfc8921f167f908dd382295e15725372" -> null
      - image             = "sha256:35c43ace9216212c0f0e546a65eec93fa9fc8e96b25880ee222b7ed2ca1d2151" -> null
      - ip_address        = "172.17.0.2" -> null
      - ip_prefix_length  = 16 -> null
      - ipc_mode          = "private" -> null
      - links             = [] -> null
      - log_driver        = "json-file" -> null
      - log_opts          = {} -> null
      - logs              = false -> null
      - max_retry_count   = 0 -> null
      - memory            = 0 -> null
      - memory_swap       = 0 -> null
      - must_run          = true -> null
      - name              = "training" -> null
      - network_data      = [
          - {
              - gateway          = "172.17.0.1"
              - ip_address       = "172.17.0.2"
              - ip_prefix_length = 16
              - network_name     = "bridge"
            },
        ] -> null
      - network_mode      = "default" -> null
      - privileged        = false -> null
      - publish_all_ports = false -> null
      - read_only         = false -> null
      - restart           = "no" -> null
      - rm                = false -> null
      - shm_size          = 64 -> null
      - start             = true -> null
      - sysctls           = {} -> null
      - tmpfs             = {} -> null

      - ports {
          - external = 80 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }

  # docker_image.nginx will be destroyed
  - resource "docker_image" "nginx" {
      - id     = "sha256:35c43ace9216212c0f0e546a65eec93fa9fc8e96b25880ee222b7ed2ca1d2151nginx:latest" -> null
      - latest = "sha256:35c43ace9216212c0f0e546a65eec93fa9fc8e96b25880ee222b7ed2ca1d2151" -> null
      - name   = "nginx:latest" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value:
```

</details>
<br>

Next, type "yes" and hit ENTER. Terraform will destroy the resources it had created earlier. \

<details>
<summary>Click here for expected output</summary>

```
docker_container.nginx: Destroying... [id=5cf67155605bc1171a5c479757e46dc1cfc8921f167f908dd382295e15725372]
docker_container.nginx: Destruction complete after 1s
docker_image.nginx: Destroying... [id=sha256:35c43ace9216212c0f0e546a65eec93fa9fc8e96b25880ee222b7ed2ca1d2151nginx:latest]
docker_image.nginx: Destruction complete after 1s

Destroy complete! Resources: 2 destroyed.
```

</details>

## Next steps

You've now learned how to install Terraform, provision an NGINX webserver, and destroy that webserver using Terraform.

Next you will create infrastructure in the cloud platform you prefer.

[Amazon Web Services (AWS)](https://learn.hashicorp.com/tutorials/terraform/aws-build)

[Google Cloud Platform (GCP)](https://learn.hashicorp.com/tutorials/terraform/google-cloud-platform-build)

[Azure](https://learn.hashicorp.com/tutorials/terraform/azure-build)
