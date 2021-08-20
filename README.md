# SnowTrek V2 - Snowpark on Jupyter and beyond

![](jpg/stock_small.jpg)

Snowtrek V2 is a project to show how to get started with Jupyter Notebooks on [Snowpark](https://docs.snowflake.com/en/developer-guide/snowpark/index.html), new Product feature announced by Snowflake for [public preview](https://www.snowflake.com/blog/welcome-to-snowpark-new-data-programmability-for-the-data-cloud/)  during the 2021 Snowflake Summit. With Snowtrek V2 you will learn how to tackle real world business problems as straight forward as ELT processing but also as diverse as math with rational numbers with unbound precision, sentiment analysis or Machine Learning.

Snowpark not only works with Jupypter Notebooks but with a variaty of IDE's. Instructions on how to set up your favorite development environment can be found [here](https://docs.snowflake.com/en/developer-guide/snowpark/setup.html). 

So what is Snowpark? Snowpark is the glue between different programmaing languages and Snowflake. In the past we had drivers (Python, Spark, JDBC, and many more) to access data in Snowflake from different programming environments. When accessing Snowflake through these drivers, results had to be passed back and force for processing which may raise performance and scalability concerns when processing big datasets. We also had User Defined Functions or UDFs, which execute custom code directly within the Snowflake engine but UDFs didnt have access to already existing standard libraries and had limited language support. With Snowpark we remove both issues. The current version of Snowpark introduces support for Scala and more lanuage support to follow. Snowpark enables us to take advantage of standard libraries to bring custom features directly to data in Snowflake to create a scalable and easy to use experience in the Data Cloud. 

This repo is structure in multiple parts. Each part has a [notebook](notebook) with a different focus. All notebooks in this series require a Jupyter notebook environment with a Scala kernel. If you do not already have access to that type of environment I would higly recommend to use [Snowtire V2](https://github.com/zoharsan/snowtire_v2) and this excellent [post](https://medium.com/snowflake/from-zero-to-snowpark-in-5-minutes-72c5f8ec0b55). Instructions on how to build the Snowtire V2 environment for Snowtrek V2 can be found below. 

All notebooks will be fully self contained, meaning all you need for processing and analyzing datasets with billions of rows is a Snowflake account. If you do not already have a snowflake account, just follow the steps in [sign up](https://signup.snowflake.com/). A time limited  account for trial purposes is free and doesn't even require a credit card. 

Versions used in this notebook are up-to-date as of August 2021. Please update them as necessary in the Snowtire setup step.


- [Part 1](notebook/part1/part1.ipynb) 

    The first notebook in this series provides a quick-start guide and an introduction to the Snowpark dataframe API. The notebook explains the steps 
    for  setting up the environment (REPL), and how to resolve dependencies to Snowpark. After a simple "Hello World" example you will learn about the Snowflake dataframe API, projections, filters, and joins. 

- [Part 2](notebook/part1/part2.ipynb) 

    The second notebook in the series builds on the quick-start of the first part. Using the TPCH datset in the sample database, it shows how to use aggregations and pivot functions in the Snowpark dataframe API. Then it introduces UDFs and how to build a stand-alone UDF, i.e. a UDF that only uses standard primitives. From there we will learn how to use thrid party Scala libraries to perform much more complex tasks like math for numbers with unbound (unlimited number of significant digits) precision or how to perform sentiment analysis on an arbitrary string.
    
- [Part 3](notebook/part1/part3.ipynb) 

    The first notebook combines the learnings from part 1 & 2. It implements an end-to-end ML use-case including data ingestion, ETL/ELT transformations, Model training, Model scoring, and finally result visualization.
    
## Running Jupyter via Snowtire V2

The following instructions show to to build a Notebook server using a docker container. After the base install of Snowtire V2, we will add few parameters for *[nbextensions](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/install.html)*.

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
