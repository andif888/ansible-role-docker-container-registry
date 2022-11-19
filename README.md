# ansible-role-docker-container-registry

Role to run docker registy in a docker container

## Table of content

- [Default Variables](#default-variables)
  - [docker_container_registry_basic_auth_enable](#docker_container_registry_basic_auth_enable)
  - [docker_container_registry_basic_auth_password](#docker_container_registry_basic_auth_password)
  - [docker_container_registry_basic_auth_username](#docker_container_registry_basic_auth_username)
  - [docker_container_registry_env](#docker_container_registry_env)
  - [docker_container_registry_image](#docker_container_registry_image)
  - [docker_container_registry_labels](#docker_container_registry_labels)
  - [docker_container_registry_name](#docker_container_registry_name)
  - [docker_container_registry_networks](#docker_container_registry_networks)
  - [docker_container_registry_ports](#docker_container_registry_ports)
  - [docker_container_registry_restic_enable](#docker_container_registry_restic_enable)
  - [docker_container_registry_restic_retention](#docker_container_registry_restic_retention)
  - [docker_container_registry_restic_s3_bucket_name](#docker_container_registry_restic_s3_bucket_name)
  - [docker_container_registry_restic_s3_endpoint](#docker_container_registry_restic_s3_endpoint)
  - [docker_container_registry_restic_s3_repo](#docker_container_registry_restic_s3_repo)
  - [docker_container_registry_restic_s3_repo_access_key](#docker_container_registry_restic_s3_repo_access_key)
  - [docker_container_registry_restic_s3_repo_password](#docker_container_registry_restic_s3_repo_password)
  - [docker_container_registry_restic_s3_repo_secret_key](#docker_container_registry_restic_s3_repo_secret_key)
  - [docker_container_registry_restic_tag](#docker_container_registry_restic_tag)
  - [docker_container_registry_volume_dir](#docker_container_registry_volume_dir)
  - [docker_container_registry_volumes](#docker_container_registry_volumes)
  - [docker_image_registry_name](#docker_image_registry_name)
  - [docker_image_registry_pull](#docker_image_registry_pull)
  - [docker_network_registry_name](#docker_network_registry_name)
- [Discovered Tags](#discovered-tags)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Default Variables

### docker_container_registry_basic_auth_enable

Enable basic authentication for docker registry.

#### Default value

```YAML
docker_container_registry_basic_auth_enable: false
```

### docker_container_registry_basic_auth_password

Password for basic authentication for docker registry.

#### Default value

```YAML
docker_container_registry_basic_auth_password: somesecret
```

### docker_container_registry_basic_auth_username

Username for basic authentication for docker registry.

#### Default value

```YAML
docker_container_registry_basic_auth_username: sampleuser
```

### docker_container_registry_env

Dictionery of key,value pairs for docker environment variables to configure registry.

Example:

```yaml

docker_container_registry_env:

registry__mailer__ENABLED: "false"

```

#### Default value

```YAML
docker_container_registry_env: {}
```

### docker_container_registry_image

Repository path and tag used to create the container. If an image is not found or pull is true, the image will be pulled from the registry. If no tag is included, latest will be used.

#### Default value

```YAML
docker_container_registry_image: '{{ docker_image_registry_name }}'
```

### docker_container_registry_labels

Dictionary of key value pairs for container labels.

Example:

```yaml

docker_container_registry_labels:

traefik.enable: "true"

```

#### Default value

```YAML
docker_container_registry_labels: {}
```

### docker_container_registry_name

Name for the container

#### Default value

```YAML
docker_container_registry_name: registry
```

### docker_container_registry_networks

List of networks the container belongs to.

#### Default value

```YAML
docker_container_registry_networks:
  - name: '{{ docker_network_registry_name }}'
```

### docker_container_registry_ports

List of ports to publish from the container to the host.

#### Default value

```YAML
docker_container_registry_ports:
  - 5000:5000
```

### docker_container_registry_restic_enable

Enable restic backup for the container's mounted volumes.

#### Default value

```YAML
docker_container_registry_restic_enable: false
```

### docker_container_registry_restic_retention

Retention settions for `restic forget` after the `restic backup`.

#### Default value

```YAML
docker_container_registry_restic_retention:
  keep_last: 1
  keep_daily: 7
  keep_weekly: 4
```

### docker_container_registry_restic_s3_bucket_name

Minio S3 bucket name for restic backup storage.

#### Default value

```YAML
docker_container_registry_restic_s3_bucket_name: restic-{{ docker_container_registry_name
  }}
```

### docker_container_registry_restic_s3_endpoint

Minio S3 endpoint for restic backup storage.

Example:

```yaml

docker_container__base__restic_s3_endpoint: "https://minio.{{ dns_domain }}"

docker_container_registry_restic_s3_endpoint: "{{ docker_container__base__restic_s3_endpoint }}"

```

#### Default value

```YAML
docker_container_registry_restic_s3_endpoint: '{{ docker_container__base__restic_s3_endpoint
  }}'
```

### docker_container_registry_restic_s3_repo

Minio S3 repo URL for restic backup storage.

#### Default value

```YAML
docker_container_registry_restic_s3_repo: s3:{{ docker_container_registry_restic_s3_endpoint
  }}/{{ docker_container_registry_restic_s3_bucket_name }}
```

### docker_container_registry_restic_s3_repo_access_key

Minio S3 repo access key for restic backup storage.

#### Default value

```YAML
docker_container_registry_restic_s3_repo_access_key: '{{ docker_container__base__restic_s3_repo_access_key
  }}'
```

### docker_container_registry_restic_s3_repo_password

Minio S3 repo password for restic backup storage.

#### Default value

```YAML
docker_container_registry_restic_s3_repo_password: '{{ docker_container__base__restic_s3_repo_password
  }}'
```

### docker_container_registry_restic_s3_repo_secret_key

Minio S3 repo secret key for restic backup storage.

#### Default value

```YAML
docker_container_registry_restic_s3_repo_secret_key: '{{ docker_container__base__restic_s3_repo_secret_key
  }}'
```

### docker_container_registry_restic_tag

Tag for the `restic backup` command

#### Default value

```YAML
docker_container_registry_restic_tag: '{{ docker_container_registry_name }}'
```

### docker_container_registry_volume_dir

Volume mount host directory, where Treafik config files are stored.

#### Default value

```YAML
docker_container_registry_volume_dir: '{{ docker_container__base__volume_dir }}/{{
  docker_container_registry_name }}'
```

### docker_container_registry_volumes

List of volumes to mount within the container.

#### Default value

```YAML
docker_container_registry_volumes:
  - '{{ docker_container_registry_volume_dir }}/registry:/var/lib/registry'
```

### docker_image_registry_name

Repository path and tag for the container image.

#### Default value

```YAML
docker_image_registry_name: registry:2
```

### docker_image_registry_pull

Indicate to always pull the docker image.

#### Default value

```YAML
docker_image_registry_pull: false
```

### docker_network_registry_name

Name of the docker network created for registry.

#### Default value

```YAML
docker_network_registry_name: '{{ docker_container_registry_name }}_backend'
```

## Discovered Tags

**_docker-container-backup-all_**\
&emsp;Backup all containers' volume mounts.

**_docker-container-backup-init-all_**\
&emsp;Run init backup task for all container.

**_docker-container-backup-init-registry_**\
&emsp;Run init backup task for registry if restic is enabled.

**_docker-container-backup-list-all_**\
&emsp;List all containers' backups.

**_docker-container-backup-list-registry_**\
&emsp;List registry backups.

**_docker-container-backup-registry_**\
&emsp;Backup registry volume mounts.

**_docker-container-prereq-all_**\
&emsp;Ensure all pre-requisites are installed

**_docker-container-prereq-registry_**\
&emsp;Ensure all pre-requisites for registry are installed

**_docker-container-purge-all_**\
&emsp;Remove all containers and delete volume mounts.

**_docker-container-purge-registry_**\
&emsp;Remove registry and delete volume mounts.

**_docker-container-remove-all_**\
&emsp;Remove all containers.

**_docker-container-remove-registry_**\
&emsp;Remove registry.

**_docker-container-restore-all_**\
&emsp;Run restic restore for all restic enabled containers.

**_docker-container-restore-registry_**\
&emsp;Run restic restore for registry if restic is enabled.

**_docker-container-setup-all_**\
&emsp;Run setup task for all containers.

**_docker-container-setup-registry_**\
&emsp;Run setup task for registry.

**_never_**


## Dependencies

None.

## License

license (GPL-2.0-or-later, MIT, etc)

## Author

andif888
