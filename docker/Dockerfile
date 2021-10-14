#
#  This Dockerfile creates a docker image from the base image https://hub.docker.com/r/almondsh/almond 
#
ARG almond_version=0.10.9
ARG scala_kernel_version=2.12.12
FROM almondsh/almond:$almond_version-scala-$scala_kernel_version
USER root
RUN apt-get update && apt-get upgrade -y && apt-get install -y openjdk-8-jdk
USER jovyan
RUN pip install Jupyterlab==3.1.17 
RUN jupyter labextension install jupyterlab-plotly@5.3.1
