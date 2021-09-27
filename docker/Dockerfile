#
#  This Dockerfile creates a docker image from the base image jupyter/minimal-notebook (https://hub.docker.com/r/jupyter/minimal-notebook)
#  

FROM jupyter/minimal-notebook:33add21fab64
USER root
ARG almond_version=0.11.1
ARG scala_kernel_version=2.12.12
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
RUN apt-get update &&\
    apt-get install -y apt-utils && \
    apt-get install -y vim && \
    apt-get install -y nodejs && \
    apt-get install -y openjdk-11-jdk
RUN update-alternatives --set java /usr/lib/jvm/java-11-openjdk-amd64/bin/java
RUN sudo -u jovyan curl -Lo coursier https://git.io/coursier-cli
RUN chown -R jovyan:users /home/jovyan/coursier && chmod +x /home/jovyan/coursier
RUN sudo -u jovyan /home/jovyan/coursier launch --fork almond:$almond_version --scala $scala_kernel_version -- --install
RUN sudo -u jovyan /opt/conda/bin/jupyter labextension install jupyterlab-plotly
RUN sudo -u jovyan rm -f coursier

EXPOSE 8888
