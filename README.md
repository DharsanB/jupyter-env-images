# JupyterHub single user for DS

## Project overview
> This is a custom image created for Data Science purposes fixing existing issues with the official docker image of JupyterHub.
>
> This image is built on top of jupyterhub/singleuser docker image.
>
> This image will come preinstalled with most used data science packages in their latest version.

## Configuration
1. docker
2. JupyterHub
3. Rest of the prerequisites and instructions will be added as we progress

## Pulling image from dockerhub
### To pull the latest docker image, use
[![docker pulls](https://img.shields.io/docker/pulls/dharsanb/jupyter.svg)](https://hub.docker.com/r/dharsanb/jupyter) [![docker build status](https://img.shields.io/docker/cloud/build/dharsanb/jupyter)](https://hub.docker.com/r/dharsanb/jupyter/builds)
```console
foo@bar:~$ docker pull dharsanb/jupyter
```
***
## Prerequisites
This image was created to be used with JupyterHub and dockerspawner

## How to use this image
Add the following configuration to jupyterhub_config.py
```python
c.JupyterHub.spawner_class = 'dockerspawner.DockerSpawner'
c.DockerSpawner.image = 'dharsanb/jupyterhub-singleuser'
```

***
## Completed items
- [x] Installing R Kernel
- [x] Adding conda PATH to `root` user
- [x] Fixed default terminal path to working directory
- [x] Installing RStudio for JupyterHub
- [x] Adding NB extension and configurator
- [ ] Enabling configurator by default
- [ ] Pre-installing frequently used data science packages
- 1. [ ] R
- 2. [ ] Python
***
