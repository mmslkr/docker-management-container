# Docker Management Container

The Docker Management Container includes a set of plugins and services that make it easier to build, test, deploy and manage infrastructure.

<picture>
  <source 
    srcset="https://user-images.githubusercontent.com/3198961/186105564-3901aded-21f1-4191-b323-e943f49ea5ed.png" 
    media="(prefers-color-scheme: dark)"
    width="30%" height="30%">
  <img width="30%" height="30%" src="https://user-images.githubusercontent.com/3198961/186105473-eab8c428-4a19-474d-b391-e2410383d05d.png">
</picture>

## Setup

Set service-specific tools, versions and configuration:

``` bash
vi .docker_build
```

## Image and Packages

### Image

| ARG                | Required | Default |
| ------------------ | -------- | ------- |
| ROCKYLINUX_VERSION | no       | 8       |

### Image Config

python version can be defined

| ARG              | Required | Default |
| ---------------- | -------- | ------- |
| PYTHON_VERSION   | no       | 3       |

Further packages to install can be specified

| ARG              | Required | Default |
| ---------------- | -------- | ------- |
| PACKAGES         | no       |         |

## Tools

Install needed Tools within Management Container

| ARG               | Required | Default | Description                      |
| ----------------- | -------- | ------- | -------------------------------- |
| ANSIBLE_VERSION   | no       | latest  |                                  |
| DOCKER_VERSION    | no       | latest  |                                  |
| NOMAD_VERSION     | no       | latest  |                                  |
| CONSUL_VERSION    | no       | latest  |                                  |
| KUBECTL_VERSION   | no       | latest  |                                  |
| HELM_VERSION      | no       | latest  |                                  |
| TERRAFORM_VERSION | no       | latest  |                                  |
| AZ_CLI_VERSION    | no       | latest  |                                  |
| AWS_CLI_VERSION   | no       | latest  |                                  |
| GCLOUD_VERSION    | no       | latest  |                                  |
| GITHUB_BINARIES   | no       | latest  | List of packages to be installed, format is [package binary name]:[package binary format] |

## Tool Configuration / Add Specific extension

| ARG                  | Required | Default | Description                                                              |
| -------------------- | -------- | ------- | ------------------------------------------------------------------------ |
| PIP_REQUIREMENTS     | no       |         | File which includes a list of pip packages to be installed               |
| ANSIBLE_REQUIREMENTS | no       |         | File which includes a list of ansible-galaxy collections to be installed |
| AZ_CLI_EXTENSIONS    | no       |         | List of azure-cli extenstions to be installed                            |

### System Configuration

## create Dockerfile from template with .docker_build configuration

``` bash
sh render.sh .
```

## create container image

``` bash
export SERVICE=NAME
export TAG=1.0.0

docker build -t ${SERVICE}-mgmt:${TAG} .
```

## Pipelines

Examples for build within Pipelines can be found under [examples/pipeline]().

## Run Docker Container on local system

Install Docker Desktop, see [installation instructions](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly).

### run mgmt container

``` bash
docker run -ti ${SERVICE}-mgmt:${TAG}
```

## Mount locals into Container

### Mount Volumes

Configure WSL for correct usage of volumes, see [https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly], chapter: Create and modify the new WSL configuration file.

``` bash
/etc/wsl.conf

[automount]
root = /
options = "metadata"
```

#### Mount specific folder

``` bash
su root
mkdir /c

mount --bind /mnt/c /c
```

##### start container with local mount

``` bash
docker run -ti --env-file .docker_env -v /c/Users/USER/GITRepo:/root -w /root -ti ${SERVICE}-mgmt:${TAG}
```

behind proxy

``` bash
docker run -ti --env-file .docker_proxy_env -v /c/Users/USER/GITRepo:/root -w /root -ti ${SERVICE}-mgmt:${TAG}
```
