# Containers

The actual code that the workload will run will be inside a [container](https://www.docker.com/resources/what-container/). A container can be specified in a template, an environment, or directly in a workload. HX3 provides several containers by default, including `jupyter/scipy-notebook`, `rocker/rstudio`, and `tensorflow/tensorflow`. Details of these containers can be found on [Docker Hub](https://hub.docker.com/), but there is no restriction on where containers can be loaded from. Other sources of containers include the [Github Container Registry (ghcr.io)](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry) and NVidia's [NGC Catalog](https://ngc.nvidia.com). RunAI often refers to containers as **Images**.

## Specifying a container

When you create a template, environment, or workload, you can specify a container by choosing an Image URL. HX3, by default, assumes a container is available on Docker Hub, so if you specify `rocker/rstudio` your image will come from Docker. To specify a container from a different server, you must use the full URL. For example, Open WebUI makes a container available on the Github Container Registry, so to use it you would set the Image URL to `ghcr.io/open-webui/open-webui`.

## Specifying a container version

By default, an Image URL will pull the most recent version of a particular container. To make sure you are using a specific version of a container consistently, specify the container version by added a colon and the version ID to the image URL. For example, to use version 2.20 of Tensorflow, you would set the Image URL to `tensorflow/tensorflow:2.20.0`

## Building custom containers

You may wish to specify more precisely the code that runs in a container. In this case, you may create your own [Dockerfile](https://docs.docker.com/reference/dockerfile/). A Dockerfile contains all of the commands required to build a container. Once it is built, you can use the `docker push` command to upload your container to a registry of your choosing, which will give you a URL that is suitable for use in the HX3 Image URL field.

See [Writing a Dockerfile](https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/) for more information.
