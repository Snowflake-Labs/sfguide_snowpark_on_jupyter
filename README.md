# Snowtrek V2 - Snowpark on Jupyter and Beyond

![](jpg/stock_small.jpg)

Snowtrek V2 is a project to show how to get started with Jupyter Notebooks on [Snowpark](https://docs.snowflake.com/en/developer-guide/snowpark/index.html), a new product feature announced by Snowflake for [public preview](https://www.snowflake.com/blog/welcome-to-snowpark-new-data-programmability-for-the-data-cloud/) during the 2021 Snowflake Summit. With Snowtrek V2 you will learn how to tackle real world business problems as straightforward as ELT processing but also as diverse as math with rational numbers with unbounded precision, sentiment analysis and machine learning.

Snowpark not only works with Jupyter Notebooks but with a variety of IDEs. Instructions on how to set up your favorite development environment can be found in the Snowpark documentation under [Setting Up Your Development Environment for Snowpark](https://docs.snowflake.com/en/developer-guide/snowpark/setup.html).

Snowpark is a new developer experience that lets developers interact with data without first having to extract it. It's the glue between different programming languages and Snowflake. 

Snowflake has always provided drivers (Python, Spark, JDBC, and many more) to access data in Snowflake from different programming environments. When accessing Snowflake through these drivers, results have to be passed back and forth for processing, which may raise performance and scalability concerns when processing big datasets. Snowflake also has user-defined functions or UDFs, which execute custom code directly within the Snowflake engine but UDFs did not have access to already existing standard libraries and had limited language support. 

With Snowpark, we remove both issues. The current version of Snowpark introduces support for Scala, a well know language for data piplines, ELT/ETL, ML, big data projects and other use cases. Snowpark enables us to take advantage of standard libraries to bring custom features directly to data to create a scalable and easy to use experience in the Data Cloud.

This repo is structured in multiple parts. Each part has a [notebook](notebook) with specific focus areas. All notebooks in this series require a Jupyter Notebook environment with a Scala kernel.  If you do not already have access to that type of environment, I would highly recommend that you use [Snowtire V2](https://github.com/zoharsan/snowtire_v2) and this excellent post [From Zero to Snowpark in 5 minutes](https://medium.com/snowflake/from-zero-to-snowpark-in-5-minutes-72c5f8ec0b55). Please note that Snowtire is not officially supported by Snowflake, and is provided as-is. Additional instructions on how to set up the environment with the latest versions can be found in the README file in this repo. 

All notebooks will be fully self contained, meaning that all you need for processing and analyzing datasets is a Snowflake account.  If you do not have a Snowflake account, you can sign up for a [free trial](https://signup.snowflake.com/). It doesn't even require a credit card.

Versions used in this notebook are up-to-date as of August 2021. Please update them as necessary in the Snowtire setup step.


- [Part 1](notebook/part1/part1.ipynb) 

    The first notebook in this series provides a quick-start guide and an introduction to the Snowpark DataFram API. The notebook explains the steps for setting up the environment (REPL), and how to resolve dependencies to Snowpark. After a simple "Hello World" example you will learn about the Snowflake DataFrame API, projections, filters, and joins.
 

- [Part 2](notebook/part2/part2.ipynb) 

    The second notebook in the series builds on the quick-start of the first part. Using the TPCH dataset in the sample database, it shows how to use aggregations and pivot functions in the Snowpark DataFrame API. Then it introduces UDFs and how to build a stand-alone UDF: a UDF that only uses standard primitives. From there, we will learn how to use third party Scala libraries to perform much more complex tasks like math for numbers with unbounded (unlimited number of significant digits) precision and how to perform sentiment analysis on an arbitrary string.
    
- [Part 3](notebook/part3/part3.ipynb) 

    The third notebook combines what you learned in part 1 and 2. It implements an end-to-end ML use case including data ingestion, ETL/ELT transformations, model training, model scoring, and result visualization.
    
## Running Jupyter on Snowtire V2

The following instructions show how to build a Notebook server using a Docker container. After the base install of Snowtire V2, we will a add few parameters for *[nbextensions](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/install.html)*.

1. Download and install [Docker](https://docs.docker.com/docker-for-mac/install/).

1. Clone the Snowtire repo: 

        cd ~
        mkdir DockerImages
        cd DockerImages
        git clone https://github.com/zoharsan/snowtire_v2.git
        cd snowtire_v2/

1. Build the image and get the latest kernel and drivers:

        docker build --pull -t snowtire . \
        --build-arg odbc_version=2.23.2 \
        --build-arg jdbc_version=3.13.4 \
        --build-arg spark_version=2.9.1-spark_3.1 \
        --build-arg snowsql_version=1.2.14 \
        --build-arg almond_version=0.11.1 \
        --build-arg scala_kernel_version=2.12.12
        
1. Clone the Snowtrek: 

        cd ~/DockerImages
        git clone git@github.com:snowflakecorp/snowtrek_V2.git
        cd snowtrek
        
1. Start the Snowtire container and mount the Snowtrek directory to the container. The command below assumes that you have cloned Snowtrek V2 to ~/DockerImages/snowtrek_V2. Adjust the path if necessary. 

        docker run -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v ~/DockerImages/snowtrek_V2:/home/jovyan/snowtrek_V2 --name snowtrek_v2 snowtire:latest
        
    The output should be similar to the following

        [I 2021-08-09 20:42:43.745 LabApp] JupyterLab extension loaded from /opt/conda/lib/python3.9/site-packages/jupyterlab
        [I 2021-08-09 20:42:43.745 LabApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
        [I 20:42:43.751 NotebookApp] Serving notebooks from local directory: /home/jovyan
        [I 20:42:43.751 NotebookApp] Jupyter Notebook 6.4.0 is running at:
        [I 20:42:43.751 NotebookApp] http://7f4d1922ad40:8888/?token=c3223df0b33e6232bbb06f3e403d481da4bfe515f23873f2
        [I 20:42:43.751 NotebookApp]  or http://127.0.0.1:8888/?token=c3223df0b33e6232bbb06f3e403d481da4bfe515f23873f2
        [I 20:42:43.751 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).

1. Start a browser session (Safari, Chrome, ...). Paste the line with the local host address (127.0.0.1) printed in **your shell window** into the browser status bar and update the port (8888) to **your port** in case you have changed the port in the step above.

1. Use Docker commands to start and stop the container: 

        docker [start/stop/restart] snowtrek_v2
    
    After starting/restaring the container you can get the security token by running this command
    
        docker logs snowflake_v2
