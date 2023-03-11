
# kedro
This is a repo to take notes about how to work with kedro, explainig all the steps required for it.

## PREREQUISITES
1) **Python** is needed. In order to know the version (if Python is already started), run in the console (in my case I work with **Visual Studio Code): 
````python
python --version
#Output
Python 3.10.9
````
2) **Git** is needed. To check if it is install, run in the console:
````python
git -v
#Output
git version 2.39.1.windows.1
````
3) **Create a new Python virtual environment.** To create a new virtual environment called spaceflights2 using conda:
````python
conda create --name spaceflights2 python=3.10 -y
````
4) Activate the new virtual environment created in step 3):
````python
conda activate spaceflights2
````
## INSTALL KEDRO
 ````python
pip install kedro
````

To check that Kedro is installed:
````python
kedro info
````
You should see an ASCII art graphic and the Kedro version number: 

<img width="294" alt="kedro_info" src="https://user-images.githubusercontent.com/117999669/224502516-157fd0ea-d7bf-4888-9f6f-55fa1e5b1616.PNG">

## STANDARD DEVELOPMENT WORKFLOW

When you build a Kedro project, you will typically follow a standard development workflow:

1) Set up the project template

  -Create a new project and install project dependencies (dependencies are libraries).

  -Configure credentials and any other sensitive/personal content, and logging

2) Set up the data

  -Add data to the data folder

  -Reference all datasets for the project

3) Create the pipeline

  -Construct nodes to make up the pipeline

  -Choose how to run the pipeline: sequentially or in parallel

4) Package the project

  -Build the project documentation

  -Package the project for distribution
  
## WORK WITH KEDRO IN A PROJECT

After kedro installation and environment creation (and activation with **conda activate spaceflights2),** we need to create a project in kedro. 

1) For that purpose, in the terminal window, we need to navigate to the folder you want to store the project and type the following to create an empty project:
 ````python
kedro new
````
2) Kedro projects have a **requirements.txt** file to specify their dependencies and enable sharable projects by ensuring consistency across Python packages and versions.

The generic project template bundles some typical dependencies in **src/requirements.txt.** Other typical dependencies to install, just to show an example, could be the following ones:

````python
kedro==0.18.6
kedro-datasets[pandas.CSVDataSet, pandas.ExcelDataSet, pandas.ParquetDataSet]~=1.0.2  # Specify Kedro-Datasets dependencies
kedro-viz~=5.0                                                                 # Visualise your pipelines
scikit-learn~=1.0                                                              # For modelling in the data science pipeline
````

In that case, you should add the previous lines to **src/requirements.txt file.**

3) After step 2), we need to install the new dependencies introduced in the requirements.txt file:
 ````python
pip install -r src/requirements.txt
````
## SET UP THE DATA
1) Data need to be saved in **data/01_raw** path for the project. In this case, since we are followed spaceflights tutorial for the kedro official website, the data files are companies.csv, reviews.csv y shuttles.xlsx.

2) All Kedro projects have a conf/base/catalog.yml file, and you register each dataset by adding a named entry into the .yml file that includes the following:

 - File location (path)

 - Parameters for the given dataset

 - Type of data

 - Versioning

In this example, the registration needed in calatog.yml is:

companies:
  type: pandas.CSVDataSet
  filepath: data/01_raw/companies.csv

reviews:
  type: pandas.CSVDataSet
  filepath: data/01_raw/reviews.csv
  
 shuttles:
  type: pandas.ExcelDataSet
  filepath: data/01_raw/shuttles.xlsx
  load_args:
    engine: openpyxl # Use modern Excel engine (the default since Kedro 0.18.0)

