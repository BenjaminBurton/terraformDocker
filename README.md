# Build Infrastructure - Terraform Docker

> Create a file to define your infrastructure.

```js
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}
```

# Initialize the directory

> When you create a new configuration — or check out an existing configuration from version control — you need to initialize the directory with `terraform init`.

Initializing a configuration directory downloads and installs the providers defined in the configuration, which in this case is the `docker` provider.

If you did not deploy the Quick Start steps in the previous tutorial, initialize the directory now.

> $`terraform init`

# Format and validate the configuration

> $`terraform fmt`

> We recommend using consistent formatting in all of your configuration files. The `terraform fmt` command automatically updates configurations in the current directory for readability and consistency.

Format your configuration. Terraform will print out the names of the files it modified, if any. In this case, your configuration file was already formatted correctly, so Terraform won't return any file names.

- Validate your configuration. The example configuration provided above is valid, so Terraform will return a success message.

> You can also make sure your configuration is syntactically valid and internally consistent by using the terraform validate command.

> $`terraform validate`

# Create infrastructure

> Apply the configuration now with the `terraform apply` command. Terraform will print output similar to what is shown below. We have truncated some of the output to save space.

> $`terraform apply`

# Inspect state

> nspect the current state using `terraform show`

> $`terraform show`

# Manually Manage State

> Terraform has a built-in command called `terraform state` for advanced state management. Use the `list` subcommand to list of the resources in your project's state.

> $`terraform state list`

# Update configuration

```js
> resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "tutorial"
  hostname = "learn-terraform-docker"
  ports {
    internal = 80
-   external = 8000 Replace
+   external = 8080 Add
  }
}
```

# Apply Changes

> After changing the configuration, run `terraform apply` again to see how Terraform will apply this change to the existing resources.

> $`terraform apply`

# Destroy Infrastructure

> It does not destroy resources running elsewhere that are not managed by the current Terraform project.

> $`terraform destroy`

# Set the container name with a variable
> Create a new file called variables.tf with a block defining a new container_name variable.

```js
variable "container_name" {
  description = "Value of the name for the Docker container"
  type        = string
  default     = "ExampleNginxContainer"
}
```

> In main.tf, update the docker_container resource block to use the new variable. The container_name variable block will default to its default value ("ExampleNginxContainer") unless you declare a different value.

```js
resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
- name  = "tutorial"
+ name  = var.container_name
  ports {
    internal = 80
    external = 8080
  }
}
```

# Apply configuration

> Apply the configuration. Respond to the confirmation prompt with a `yes`.

> $`terraform apply`

> Now apply the configuration again, this time overriding the default container name by passing in a variable using the -var flag. Terraform will update the container's name attribute with the new name. Respond to the confirmation prompt with yes.

> $`terraform apply -var "container_name=YetAnotherName"`

# Query Data with Outputs

> Ensure that your configuration matches this, and that you have initialized your configuration in the learn-terraform-docker-container directory.

> $`terraform init`

> Apply the configuration before continuing this tutorial. Respond to the confirmation prompt with a yes.

> $`terraform apply`

# Output Docker container configuration

> Create a file called outputs.tf in your learn-terraform-docker-container directory.

Add the configuration below to outputs.tf to define outputs for your container's ID and the image ID.

```js
output "container_id" {
  description = "ID of the Docker container"
  value       = docker_container.nginx.id
}

output "image_id" {
  description = "ID of the Docker image"
  value       = docker_image.nginx.id
}
```

# Inspect output values

You must apply this configuration before you can use these output values. Apply your configuration now. Respond to the confirmation prompt with `yes`.

> $`terraform apply`

> Terraform prints output values to the screen when you apply your configuration. Query the outputs with the terraform output command.

> $`terraform output`

> You can use Terraform outputs to connect your Terraform projects with other parts of your infrastructure, or with other Terraform projects

# Destroy infrastructure

> $`terraform destroy`






