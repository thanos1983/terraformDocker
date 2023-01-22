# terraformDocker
Minor demo of terraform running an NGINX Dockerfile

Sample of `terraform init`:

```bash
$ terraform init

Initializing the backend...

Initializing provider plugins...
- Finding kreuzwerker/docker versions matching "3.0.1"...
- Installing kreuzwerker/docker v3.0.1...
- Installed kreuzwerker/docker v3.0.1 (self-signed, key ID BD080C4571C6104C)

Partner and community providers are signed by their developers.
If you'd like to know more about provider signing, you can read about it here:
https://www.terraform.io/docs/cli/plugins/signing.html

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

Sample of `terraform plan`:

```bash
$ terraform plan -out=planOutpout

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach                                      = false
      + bridge                                      = (known after apply)
      + command                                     = (known after apply)
      + container_logs                              = (known after apply)
      + container_read_refresh_timeout_milliseconds = 15000
      + entrypoint                                  = (known after apply)
      + env                                         = (known after apply)
      + exit_code                                   = (known after apply)
      + hostname                                    = (known after apply)
      + id                                          = (known after apply)
      + image                                       = (known after apply)
      + init                                        = (known after apply)
      + ipc_mode                                    = (known after apply)
      + log_driver                                  = (known after apply)
      + logs                                        = false
      + must_run                                    = true
      + name                                        = "tutorial"
      + network_data                                = (known after apply)
      + read_only                                   = false
      + remove_volumes                              = true
      + restart                                     = "no"
      + rm                                          = false
      + runtime                                     = (known after apply)
      + security_opts                               = (known after apply)
      + shm_size                                    = (known after apply)
      + start                                       = true
      + stdin_open                                  = false
      + stop_signal                                 = (known after apply)
      + stop_timeout                                = (known after apply)
      + tty                                         = false
      + wait                                        = false
      + wait_timeout                                = 60

      + healthcheck {
          + interval     = (known after apply)
          + retries      = (known after apply)
          + start_period = (known after apply)
          + test         = (known after apply)
          + timeout      = (known after apply)
        }

      + labels {
          + label = (known after apply)
          + value = (known after apply)
        }

      + ports {
          + external = 8000
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id           = (known after apply)
      + image_id     = (known after apply)
      + keep_locally = false
      + name         = "nginx:latest"
      + repo_digest  = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

───────────────────────────────────────────────────────────────────────────────────────────

Saved the plan to: planOutpout

To perform exactly these actions, run the following command to apply:
    terraform apply "planOutpout"
```

Sample of `terraform apply`:

```bash
$ terraform apply "planOutpout"
docker_image.nginx: Creating...
docker_image.nginx: Creation complete after 6s [id=sha256:a99a39d070bfd1cb60fe65c45dea3a33764dc00a9546bf8dc46cb5a11b1b50e9nginx:latest]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 1s [id=ffe9176ff5ab238084df5b456eb0a2027015db0f302623ed805adf77e1250895]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

Sample of files after `terraform apply`:

```bash
$ ls -la
total 68
drwxr-xr-x  4 tinyos tinyos  4096 Jan 22 18:58 .
drwxr-xr-x 15 tinyos tinyos  4096 Jan 22 18:58 ..
drwxr-xr-x  8 tinyos tinyos  4096 Jan 22 18:50 .git
-rw-r--r--  1 tinyos tinyos   716 Jan 22 18:45 .gitignore
drwxr-xr-x  3 tinyos tinyos  4096 Jan 22 18:53 .terraform
-rw-r--r--  1 tinyos tinyos  1334 Jan 22 18:53 .terraform.lock.hcl
-rw-r--r--  1 tinyos tinyos 11357 Jan 22 18:45 LICENSE
-rw-r--r--  1 tinyos tinyos  5586 Jan 22 18:58 README.md
-rw-r--r--  1 tinyos tinyos   243 Jan 22 18:46 main.tf
-rw-r--r--  1 tinyos tinyos  3309 Jan 22 18:56 planOutpout
-rw-r--r--  1 tinyos tinyos    21 Jan 22 18:46 provider.tf
-rw-r--r--  1 tinyos tinyos  4374 Jan 22 18:57 terraform.tfstate
-rw-r--r--  1 tinyos tinyos   123 Jan 22 18:46 versions.tf
```

Sample of curl on `localhost`:

```bash
$ curl localhost:8000 | xmllint --format -
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0   150k      0 --:--:-- --:--:-- --:--:--  150k
<?xml version="1.0"?>
<!DOCTYPE html>
<html>
  <head>
    <title>Welcome to nginx!</title>
    <style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
  </head>
  <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
    <p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>
    <p>
      <em>Thank you for using nginx.</em>
    </p>
  </body>
</html>
```

Sample of `terraform plan destroy`:

```bash
$ terraform plan -destroy -out=destroyOutput
docker_image.nginx: Refreshing state... [id=sha256:a99a39d070bfd1cb60fe65c45dea3a33764dc00a9546bf8dc46cb5a11b1b50e9nginx:latest]
docker_container.nginx: Refreshing state... [id=ffe9176ff5ab238084df5b456eb0a2027015db0f302623ed805adf77e1250895]

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed
  - resource "docker_container" "nginx" {
      - attach                                      = false -> null
      - command                                     = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - container_read_refresh_timeout_milliseconds = 15000 -> null
      - cpu_shares                                  = 0 -> null
      - dns                                         = [] -> null
      - dns_opts                                    = [] -> null
      - dns_search                                  = [] -> null
      - entrypoint                                  = [
          - "/docker-entrypoint.sh",
        ] -> null
      - env                                         = [] -> null
      - group_add                                   = [] -> null
      - hostname                                    = "ffe9176ff5ab" -> null
      - id                                          = "ffe9176ff5ab238084df5b456eb0a2027015db0f302623ed805adf77e1250895" -> null
      - image                                       = "sha256:a99a39d070bfd1cb60fe65c45dea3a33764dc00a9546bf8dc46cb5a11b1b50e9" -> null
      - init                                        = false -> null
      - ipc_mode                                    = "private" -> null
      - log_driver                                  = "json-file" -> null
      - log_opts                                    = {} -> null
      - logs                                        = false -> null
      - max_retry_count                             = 0 -> null
      - memory                                      = 0 -> null
      - memory_swap                                 = 0 -> null
      - must_run                                    = true -> null
      - name                                        = "tutorial" -> null
      - network_data                                = [
          - {
              - gateway                   = "172.17.0.1"
              - global_ipv6_address       = ""
              - global_ipv6_prefix_length = 0
              - ip_address                = "172.17.0.2"
              - ip_prefix_length          = 16
              - ipv6_gateway              = ""
              - network_name              = "bridge"
            },
        ] -> null
      - network_mode                                = "default" -> null
      - privileged                                  = false -> null
      - publish_all_ports                           = false -> null
      - read_only                                   = false -> null
      - remove_volumes                              = true -> null
      - restart                                     = "no" -> null
      - rm                                          = false -> null
      - runtime                                     = "runc" -> null
      - security_opts                               = [] -> null
      - shm_size                                    = 64 -> null
      - start                                       = true -> null
      - stdin_open                                  = false -> null
      - stop_signal                                 = "SIGQUIT" -> null
      - stop_timeout                                = 0 -> null
      - storage_opts                                = {} -> null
      - sysctls                                     = {} -> null
      - tmpfs                                       = {} -> null
      - tty                                         = false -> null
      - wait                                        = false -> null
      - wait_timeout                                = 60 -> null

      - ports {
          - external = 8000 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }

  # docker_image.nginx will be destroyed
  - resource "docker_image" "nginx" {
      - id           = "sha256:a99a39d070bfd1cb60fe65c45dea3a33764dc00a9546bf8dc46cb5a11b1b50e9nginx:latest" -> null
      - image_id     = "sha256:a99a39d070bfd1cb60fe65c45dea3a33764dc00a9546bf8dc46cb5a11b1b50e9" -> null
      - keep_locally = false -> null
      - name         = "nginx:latest" -> null
      - repo_digest  = "nginx@sha256:b8f2383a95879e1ae064940d9a200f67a6c79e710ed82ac42263397367e7cc4e" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

───────────────────────────────────────────────────────────────────────────────────────────

Saved the plan to: destroyOutput

To perform exactly these actions, run the following command to apply:
    terraform apply "destroyOutput"
```

Sample of `terraform destroy`:

```bash
$ terraform apply "destroyOutput"
docker_container.nginx: Destroying... [id=ffe9176ff5ab238084df5b456eb0a2027015db0f302623ed805adf77e1250895]
docker_container.nginx: Destruction complete after 0s
docker_image.nginx: Destroying... [id=sha256:a99a39d070bfd1cb60fe65c45dea3a33764dc00a9546bf8dc46cb5a11b1b50e9nginx:latest]
docker_image.nginx: Destruction complete after 0s

Apply complete! Resources: 0 added, 0 changed, 2 destroyed.
```
