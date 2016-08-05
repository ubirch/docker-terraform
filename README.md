# ubirch/docker-terraform (derived from ubirch/docker-terraform)

[![Docker Hub](https://img.shields.io/badge/docker-ready-blue.svg)](https://registry.hub.docker.com/u/ubirch/docker-terraform/)
[![Docker Hub](http://img.shields.io/docker/pulls/ubirch/docker-terraform.svg)](https://registry.hub.docker.com/u/ubirch/docker-terraform/)

## What is terraform

[TerraForm][1] provides a common configuration to launch infrastructure from physical and virtual servers to email and DNS providers. Once launched, [TerraForm][1] safely and efficiently changes infrastructure as the configuration is evolved.

Simple file based configuration gives you a single view of your entire infrastructure.

http://www.terraform.io/

## Dockerfile

This Docker image is based on the official [Alpine][2] 3.4 base image.

## Terraform configuration files

This container expects the user to mount in a directory, which will be mapped to the `/data` directory inside the container.  This is the directory from which [Terraform][1] is configured to read the configuration files referenced by the commands you call.

## How to run this image

For most terraform commands, the run command is as simple as:

```
docker run -it --rm ubirch/docker-terraform [--version] [--help] <command> [<args>]
```

Some, however, require higher network privileges and SSL certificates to function correctly, which need to be mapped into the `/etc/ssl/certs` directory in the container, similar to:

```
docker run -it --rm -v /etc/ssl/certs:/etc/ssl/certs:ro --net=host ubirch/docker-terraform [--version] [--help] <command> [<args>]
```

Therefore, the repository used to build this container also contains a usedful `terraform.sh` script that will handle most of this so that the commands can be much shorter.  All that is required is filling in a `config.sh` script so that the script will know where the `/data` directory is located as well as the path to the SSL certificates.  The script will then determine whether each individual run requires the certificates as well as things like `sudo`, etc.  Therefore, for the commands below, you could substitute the `terraform.sh` script for everything but the command and options.  For example, if you have the `config.sh` correctly configured, you could do the following to run the **apply** command:

```
./terraform.sh apply [options]
```

### terraform apply

```
docker run -it --rm -v /data:/data -v /etc/ssl/certs:/etc/ssl/certs:ro --net=host ubirch/docker-terraform apply [options]
```

### terraform destroy

```
docker run -it --rm -v /data:/data ubirch/docker-terraform destroy [options] [DIR]
```

### terraform get

```
docker run -it --rm -v /data:/data ubirch/docker-terraform get [options] PATH
```

### terraform graph

```
docker run -it --rm -v /data:/data ubirch/docker-terraform graph [options]
```

### terraform init

```
docker run -it --rm -v /data:/data ubirch/docker-terraform init [options] SOURCE [PATH]
```

### terraform output

```
docker run -it --rm -v /data:/data ubirch/docker-terraform output [options] NAME
```

### terraform plan

```
docker run -it --rm -v /data:/data -v /etc/ssl/certs:/etc/ssl/certs:ro --net=host ubirch/docker-terraform plan [options]
```

### terraform push

```
docker run -it --rm -v /data:/data -v /etc/ssl/certs:/etc/ssl/certs:ro --net=host ubirch/docker-terraform push [options]
```

### terraform refresh

```
docker run -it --rm -v /data:/data -v /etc/ssl/certs:/etc/ssl/certs:ro --net=host ubirch/docker-terraform refresh [options]
```

### terraform remote

```
docker run -it --rm -v /data:/data -v /etc/ssl/certs:/etc/ssl/certs:ro --net=host ubirch/docker-terraform remote [options]
```

### terraform show

```
docker run -it --rm -v /data:/data ubirch/docker-terraform show terraform.tfstate [options]
```

### terraform taint

```
docker run -it --rm -v /data:/data -v /etc/ssl/certs:/etc/ssl/certs:ro --net=host ubirch/docker-terraform taint [options] name
```

### terraform version

```
docker run -it --rm ubirch/docker-terraform version
```

[1]: http://www.terraform.io/ "TerraForm"
[2]: https://registry.hub.docker.com/_/alpine "Alpine"
