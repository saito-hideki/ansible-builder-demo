# Getting started - Ansible Custom Execution Environment

## Build steps with docker

Create build environment:

```shell
$ git clone https://github.com/saito-hideki/ansible-builder-demo.git
$ cd ansible-builder-demo
$ python -m venv builder
$ pip install --upgrade pip
$ pip install ansible-builder
```

Build custom execution environment container image for Ansible 2.9:

```shell
$ cd ee/ansible2.9
$ ansible-builder build --container-runtime docker \
--tag quay.io/<USERNAME>/custom_ee_ansible29:latest -v 3
```
*Note: To publish container image on quay.io `--tag` should be formatted `quay.io/USERNAME/IMAGE_NAME:TAG`*

## Publish steps with [quay.io](https://quay.io/)

Login quay.io:

```shell
$ docker login quay.io
Username: <USERNAME>
Password: <PASSWORD>
```

*Note: you need [quay.io](https://quay.io/) account before publish container images*

Publish custom execution environment container image:

```shell
$ docker images
REPOSITORY                                 TAG       IMAGE ID       CREATED         SIZE
quay.io/<USERNAME>/custom_ee_ansible29   latest    e88209af2cca   2 minutes ago   604MB
<none>                                     <none>    c4cb4c080ded   3 minutes ago   533MB
<none>                                     <none>    4906aa124511   4 minutes ago   601MB
quay.io/ansible/ansible-runner             latest    5620fead8e44   14 hours ago    580MB
quay.io/ansible/ansible-builder            latest    0ab4955e4861   14 hours ago    485MB
$ docker push quay.io/<USERNAME>/custom_ee_ansible29:latest
```

## Appendix
### How to use specific version Ansible 2.9

You can specify the specific version in `execution-environment.yml` like below:

```yaml
...snip...
additional_build_steps:
  prepend: |
    RUN pip install --upgrade pip
    RUN pip install ansible==2.9.12      # HERE (1)
```

If you want to use the latest Z release like 2.9.Z, you just remove (1) like below:

```yaml
...snip...
additional_build_steps:
  prepend: |
    RUN pip install --upgrade pip
```
