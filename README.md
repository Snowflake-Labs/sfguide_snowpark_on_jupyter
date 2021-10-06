# Snowtrek V2 - Snowpark on Jupyter and Beyond

![](jpg/stock_small.jpg)

Snowtrek V2 is a project to show how to get started with Jupyter Notebooks on [Snowpark](https://docs.snowflake.com/en/developer-guide/snowpark/index.html), a new product feature announced by Snowflake for [public preview](https://www.snowflake.com/blog/welcome-to-snowpark-new-data-programmability-for-the-data-cloud/) during the 2021 Snowflake Summit. With Snowtrek V2 you will learn how to tackle real world business problems as straightforward as ELT processing but also as diverse as math with rational numbers with unbounded precision, sentiment analysis and machine learning.

Snowpark not only works with Jupyter Notebooks but with a variety of IDEs. Instructions on how to set up your favorite development environment can be found in the Snowpark documentation under [Setting Up Your Development Environment for Snowpark](https://docs.snowflake.com/en/developer-guide/snowpark/setup.html).

Snowpark is a new developer experience that lets developers interact with data without first having to extract it. It's the glue between different programming languages and Snowflake. 

Snowflake has always provided drivers (Python, Spark, JDBC, and many more) to access data in Snowflake from different programming environments. When accessing Snowflake through these drivers, results have to be passed back and forth for processing, which may raise performance and scalability concerns when processing big datasets. Snowflake also has user-defined functions or UDFs, which execute custom code directly within the Snowflake engine but UDFs did not have access to already existing standard libraries and had limited language support. 

With Snowpark, we remove both issues. The current version of Snowpark introduces support for Scala, a well know language for data piplines, ELT/ETL, ML, big data projects and other use cases. Snowpark enables us to take advantage of standard libraries to bring custom features directly to data to create a scalable and easy to use experience in the Data Cloud.

This repo is structured in multiple parts. Each part has a [notebook](notebook) with specific focus areas. All notebooks in this series require a Jupyter Notebook environment with a Scala kernel.  

All notebooks will be fully self contained, meaning that all you need for processing and analyzing datasets is a Snowflake account.  If you do not have a Snowflake account, you can sign up for a [free trial](https://signup.snowflake.com/). It doesn't even require a credit card.

Versions used in this notebook are up-to-date as of August 2021. Please update them as necessary in the Snowtire setup step.

# Tutorial

- [Part 1](notebook/part1/part1.ipynb) 

    The first notebook in this series provides a quick-start guide and an introduction to the Snowpark DataFram API. The notebook explains the steps for setting up the environment (REPL), and how to resolve dependencies to Snowpark. After a simple "Hello World" example you will learn about the Snowflake DataFrame API, projections, filters, and joins.
 

- [Part 2](notebook/part2/part2.ipynb) 

    The second notebook in the series builds on the quick-start of the first part. Using the TPCH dataset in the sample database, it shows how to use aggregations and pivot functions in the Snowpark DataFrame API. Then it introduces UDFs and how to build a stand-alone UDF: a UDF that only uses standard primitives. From there, we will learn how to use third party Scala libraries to perform much more complex tasks like math for numbers with unbounded (unlimited number of significant digits) precision and how to perform sentiment analysis on an arbitrary string.
    
- [Part 3](notebook/part3/part3.ipynb) 

    The third notebook combines what you learned in part 1 and 2. It implements an end-to-end ML use case including data ingestion, ETL/ELT transformations, model training, model scoring, and result visualization.
    
## Running Jupyter locally

The following instructions show how to build a Notebook server using a Docker container.

1. Download and install [Docker](https://docs.docker.com/docker-for-mac/install/).

1. Clone the Snowtrek repo: 

        cd ~
        mkdir DockerImages
        cd DockerImages
        git clone git@github.com:snowflakecorp/snowtrek_V2.git
        
1. Build the Docker container

        cd ~/DockerImages/snowtrek_V2/docker
        docker build -t snowtrek .
        
1. <a name="starting-your-snowtrek-environment">Starting your Snowtrek environment </a>

    Type the following commands to start the Snowtire container and mount the Snowtrek directory to the container. The command below assumes that you have cloned Snowtrek V2 to ~/DockerImages/snowtrek_V2. Adjust the path if necessary. 

        cd ~/DockerImages/snowtrek_V2
        docker run -it --rm -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v "$(pwd)":/home/jovyan/snowtrek --name snowtrek snowtrek
        
    The output should be similar to the following

        To access the server, open this file in a browser:
            file:///home/jovyan/.local/share/jupyter/runtime/jpserver-15-open.html
        Or copy and paste one of these URLs:
            http://162e383e431c:8888/lab?token=bdaead06c9944057a86f9d8a823cebad4ce66799d855be5d
            http://127.0.0.1:8888/lab?token=bdaead06c9944057a86f9d8a823cebad4ce66799d855be5d
            

1. Start a browser session (Safari, Chrome, ...). Paste the line with the local host address (127.0.0.1) printed in **your shell window** into the browser status bar and update the port (8888) to **your port** in case you have changed the port in the step above.

1. Stopping your Snowtrek environment
    
    Type the following command into a new shell window when you want to stop your the tutorial. All changes/work will be saved on your local machine. 
    
        docker stop snowtrek
        
    This command will stop and then delete the container. When you want to restart the tutorial, just run the commands above in [Starting your Snowtrek environment](#starting-your-snowtrek-environment).
        
## Running Jupyter in the cloud

In case you can't install docker on your local machine you could run the tutorial in AWS on an [AWS Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html).

1. Create a Notebook instance

   <img src="jpg/create_notebook.png" width="200" height="200" />

1. Create a Lifecycle Policy.

   <img src="jpg/create_lifecycle_policy.png" width="200" height="200" />

   Open the [lifecycle script](docker/onstart) and paste its content into the editor window.
    
   Creating the Notebook takes about 8 minutes.

1. Upload the tutorial folder (github repo zipfile)

1. Unzip folder

   Open the Launcher, start a termial window and run the command below (substitue <filename> with your filename.

        unzip SageMaker/<filename> -d SageMaker/

After you have set up either your docker or your cloud based notebook environment start with Part1 in the [Tutorial](#Tutorial).
