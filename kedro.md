
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






