# SnowTrek - Snowpark on Jupyter and beyond

Snowtrek in a jupyter notebook utilizing the Weka ML library and Snowpark.

## Snowtrek on Snowtire
Snowtrek has been tested with [Snowtire](https://community.snowflake.com/s/article/Snowtire-A-Data-Science-Sandbox-for-Snowflake). All steps necessary to build the environment for Snowtrek are included in this repo but please review the [Snowtire Documentation](https://github.com/zoharsan/snowtire_v2) documentation since it includes valuable info on how to troubleshoot your docker image.  

1. Download and install [Docker](https://docs.docker.com/docker-for-mac/install/)

1. Clone the Snowtire repo via 

        cd ~
        mkdir DockerImages
        cd DockerImages
        git clone https://github.com/zoharsan/snowtire_v2.git
        cd snowtire_v2/

1. Add the following lines at the end of file **Dockerfile** , e.g. using vi

        RUN sudo -u jovyan /opt/conda/bin/python -m pip install --upgrade nbformat
        RUN sudo -u jovyan /opt/conda/bin/python -m pip install jupyter_contrib_nbextensions
        RUN sudo -u jovyan /opt/conda/bin/jupyter contrib nbextension install --user
        RUN sudo -u jovyan /opt/conda/bin/jupyter nbextensions_configurator enable --user
        RUN sudo -u jovyan /opt/conda/bin/jupyter nbextension enable collapsible_headings/main
        RUN sudo -u jovyan /opt/conda/bin/jupyter nbextension enable execute_time/ExecuteTime
        RUN sudo -u jovyan /opt/conda/bin/jupyter nbextension enable codefolding/main

1. Build the image and get the latest kernel and drivers:

        docker build --pull -t snowtire . \
        --build-arg odbc_version=2.23.2 \
        --build-arg jdbc_version=3.13.4 \
        --build-arg spark_version=2.9.1-spark_3.1 \
        --build-arg snowsql_version=1.2.14 \
        --build-arg almond_version=0.11.1 \
        --build-arg scala_kernel_version=2.12.12
        
1. Clone the Snowtrek repo via 

        cd ~/DockerImages
        git@github.com:snowflakecorp/snowtrek.git
        cd snowtrek
        
1. Start den Snowtire container and mount the snowtrek directory to the container. The command below assumes that you cloned snowtrek to ~/DockerImages/snowtrek. Adjust the path accordingly if necessary. 

        docker run -p 8888:8888 -v ~/DockerImages/snowtrek:/home/jovyan/snowtrek --name spare-0 snowtire:latest
        
    The output should be similar to the following

        [I 2021-08-09 20:42:43.745 LabApp] JupyterLab extension loaded from /opt/conda/lib/python3.9/site-packages/jupyterlab
        [I 2021-08-09 20:42:43.745 LabApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
        [I 20:42:43.751 NotebookApp] Serving notebooks from local directory: /home/jovyan
        [I 20:42:43.751 NotebookApp] Jupyter Notebook 6.4.0 is running at:
        [I 20:42:43.751 NotebookApp] http://7f4d1922ad40:8888/?token=c3223df0b33e6232bbb06f3e403d481da4bfe515f23873f2
        [I 20:42:43.751 NotebookApp]  or http://127.0.0.1:8888/?token=c3223df0b33e6232bbb06f3e403d481da4bfe515f23873f2
        [I 20:42:43.751 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).

1. Start a browser session (Safari, Chrome, ...). Paste the line with the local host address (127.0.0.1) printed in **your shell window** into the browser status bar and update the port (8888) to **your port** in case you have changed the port in the step above.
