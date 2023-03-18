
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
    
    
In order to test that kedro can load the csv data, we can run in the terminal from the project root directory:
````python
kedro ipython
````
In my case, I prefer to run **kedro jupyter notebook** to see the information in notebooks (for beginners is easier than work directly in kedro): 
 1) If you choose this option, you have to open in jupyter a new notebook with the option kedro(name_of_the_project (in my case spaceflights2)):

<img width="640" alt="kedro_jupyter" src="https://user-images.githubusercontent.com/117999669/224537357-b53057f3-8c37-4a20-9a98-5fd998c3a4f6.PNG">

2)  With the command **catalog.list()**  in jupyter notebook you can see all data sets you have already uploaded in the kedro path **data/01_raw/.**
````python
catalog.list()
#output
['companies', 'reviews', 'shuttles', 'parameters']
````


3) The most important advantage related to work with catalogs is that you dont need to work with csv/excels/datatype commands. You can load the data directly with catalog just with the name of the dataset as relevant information:
````python
data = catalog.load("companies")
data.head(10)
````

## CREATE A DATA PROCESSING PIPELINE

1) In the terminal, run the following command to generate a new pipeline for data processing:
````python
kedro pipeline create data_processing
````
This command generates all the files you need for the pipeline:

 - Two python files within src/kedro_tutorial/pipelines/data_processing:

    - **nodes.py** (for the node functions that form the data processing)

    - **pipeline.py** (to build the pipeline)

 - A yaml file: **conf/base/parameters/data_processing.yml** to define the parameters used when running the pipeline

 - A folder for test code: **src/tests/pipelines/data_processing**

 - **__init__.py** files in the required folders to ensure that Python can import the pipeline


2) In order to learn how to add node functions, open **src/kedro_tutorial/pipelines/data_processing/nodes.py** and add the code below, which provides two functions (preprocess_companies and preprocess_shuttles) that each takes a raw DataFrame as input, convert the data in several columns to different types, and output a DataFrame containing the preprocessed data:

````python
import pandas as pd


def _is_true(x: pd.Series) -> pd.Series:
    return x == "t"


def _parse_percentage(x: pd.Series) -> pd.Series:
    x = x.str.replace("%", "")
    x = x.astype(float) / 100
    return x


def _parse_money(x: pd.Series) -> pd.Series:
    x = x.str.replace("$", "").str.replace(",", "")
    x = x.astype(float)
    return x


def preprocess_companies(companies: pd.DataFrame) -> pd.DataFrame:
    """Preprocesses the data for companies.

    Args:
        companies: Raw data.
    Returns:
        Preprocessed data, with `company_rating` converted to a float and
        `iata_approved` converted to boolean.
    """
    companies["iata_approved"] = _is_true(companies["iata_approved"])
    companies["company_rating"] = _parse_percentage(companies["company_rating"])
    return companies


def preprocess_shuttles(shuttles: pd.DataFrame) -> pd.DataFrame:
    """Preprocesses the data for shuttles.

    Args:
        shuttles: Raw data.
    Returns:
        Preprocessed data, with `price` converted to a float and `d_check_complete`,
        `moon_clearance_complete` converted to boolean.
    """
    shuttles["d_check_complete"] = _is_true(shuttles["d_check_complete"])
    shuttles["moon_clearance_complete"] = _is_true(shuttles["moon_clearance_complete"])
    shuttles["price"] = _parse_money(shuttles["price"])
    return shuttles
````
    
3) The next steps are to create a node for each function and to create a modular pipeline for data processing:

 First, add import statements for your functions by adding them to the beginning of **pipeline.py:**
 ````python
 from kedro.pipeline import Pipeline, node, pipeline

from .nodes import preprocess_companies, preprocess_shuttles
````

4) Next, add the following to **src/kedro_tutorial/pipelines/data_processing/pipeline.py**, so the create_pipeline() function is as follows:
 ````python
 def create_pipeline(**kwargs) -> Pipeline:
    return pipeline(
        [
            node(
                func=preprocess_companies,
                inputs="companies",
                outputs="preprocessed_companies",
                name="preprocess_companies_node",
            ),
            node(
                func=preprocess_shuttles,
                inputs="shuttles",
                outputs="preprocessed_shuttles",
                name="preprocess_shuttles_node",
            ),
        ]
    )
   ````
   
Note that the inputs statements for **companies** and **shuttles** refer to the datasets defined in **conf/base/catalog.yml.** They are inputs to the **preprocess_companies** and **preprocess_shuttles functions.** Kedro uses the named node inputs (and outputs) to determine interdependencies between the nodes, and their execution order.

5) Run the following command in your terminal window to test the node named **preprocess_companies_node:**
````python
kedro run --nodes=preprocess_companies_node
#output
[08/09/22 16:43:10] INFO     Kedro project spaceflights2                                         session.py:346
[08/09/22 16:43:11] INFO     Loading data from 'companies' (CSVDataSet)...                   data_catalog.py:343
                    INFO     Running node: preprocess_companies_node:                                node.py:327
                             preprocess_companies([companies]) -> [preprocessed_companies]
                    INFO     Saving data to 'preprocessed_companies' (MemoryDataSet)...      data_catalog.py:382
                    INFO     Completed 1 out of 1 tasks                                  sequential_runner.py:85
                    INFO     Pipeline execution completed successfully.                             runner.py:89
                    INFO     Loading data from 'preprocessed_companies' (MemoryDataSet)...   data_catalog.py:343
 ````
 
6) To test both nodes together as the complete data processing pipeline, you can write in your terminal:
````python
kedro run
````

7) Each of the nodes outputs a new dataset (**preprocessed_companies** and **preprocessed_shuttles).**

When Kedro runs the pipeline, it determines that neither dataset is registered in the data catalog, so it stores these as temporary datasets in memory as Python objects using the MemoryDataSet class. Once all nodes that depend on an temporary dataset have executed, the dataset is cleared and the Python garbage collector releases the memory.

If you prefer to save the preprocessed data to file, add the following to the end of **conf/base/catalog.yml:**
````python
preprocessed_companies:
  type: pandas.ParquetDataSet
  filepath: data/02_intermediate/preprocessed_companies.parquet

preprocessed_shuttles:
  type: pandas.ParquetDataSet
  filepath: data/02_intermediate/preprocessed_shuttles.parquet
````
8) Now, we are going to add another node for a function that joins together the three datasets into a single model input table. You’ll add some code for a function and node called **create_model_input_table**, which Kedro processes as follows:

  - Kedro uses the **preprocessed_shuttles, preprocessed_companies** and **reviews datasets** as **INPUTS**.
  - Kedro saves the **OUTPUT** as a dataset called **model_input_table**.

9) But first we need to add the **create_model_input_table() function** to **src/kedro_tutorial/pipelines/data_processing/nodes.py**:
````python
def create_model_input_table(
    shuttles: pd.DataFrame, companies: pd.DataFrame, reviews: pd.DataFrame
) -> pd.DataFrame:
    """Combines all data to create a model input table.

    Args:
        shuttles: Preprocessed data for shuttles.
        companies: Preprocessed data for companies.
        reviews: Raw data for reviews.
    Returns:
        model input table.

    """
    rated_shuttles = shuttles.merge(reviews, left_on="id", right_on="shuttle_id")
    model_input_table = rated_shuttles.merge(
        companies, left_on="company_id", right_on="id"
    )
    model_input_table = model_input_table.dropna()
    return model_input_table
````

10) Also we need to add an import statement for **create_model_input_table** at the top of **src/kedro_tutorial/pipelines/data_processing/pipeline.py:**
````python
from .nodes import create_model_input_table, preprocess_companies, preprocess_shuttles
````

11) Add the code below to include the new node in the pipeline:
````python
node(
    func=create_model_input_table,
    inputs=["preprocessed_shuttles", "preprocessed_companies", "reviews"],
    outputs="model_input_table",
    name="create_model_input_table_node",
),
````
## GENERATE A NEW PIPELINE TEMPLATE

1) Run the following command to create the **data_science pipeline:** (dont forget activate first the appropiate environment created previously to work with kedro: command **conda activate spaceflights2).**

````python
kedro pipeline create data_science
````

2) Add the following code to the **src/kedro_tutorial/pipelines/data_science/nodes.py** file:
````python
import logging
from typing import Dict, Tuple

import pandas as pd
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score
from sklearn.model_selection import train_test_split


def split_data(data: pd.DataFrame, parameters: Dict) -> Tuple:
    """Splits data into features and targets training and test sets.

    Args:
        data: Data containing features and target.
        parameters: Parameters defined in parameters/data_science.yml.
    Returns:
        Split data.
    """
    X = data[parameters["features"]]
    y = data["price"]
    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=parameters["test_size"], random_state=parameters["random_state"]
    )
    return X_train, X_test, y_train, y_test


def train_model(X_train: pd.DataFrame, y_train: pd.Series) -> LinearRegression:
    """Trains the linear regression model.

    Args:
        X_train: Training data of independent features.
        y_train: Training data for price.

    Returns:
        Trained model.
    """
    regressor = LinearRegression()
    regressor.fit(X_train, y_train)
    return regressor


def evaluate_model(
    regressor: LinearRegression, X_test: pd.DataFrame, y_test: pd.Series
):
    """Calculates and logs the coefficient of determination.

    Args:
        regressor: Trained model.
        X_test: Testing data of independent features.
        y_test: Testing data for price.
    """
    y_pred = regressor.predict(X_test)
    score = r2_score(y_test, y_pred)
    logger = logging.getLogger(__name__)
    logger.info("Model has a coefficient R^2 of %.3f on test data.", score)
````

3) You now need to add some parameters that are used by the DataCatalog when the pipeline executes. Add the following to **conf/base/parameters/data_science.yml:**
````python
model_options:
  test_size: 0.2
  random_state: 3
  features:
    - engines
    - passenger_capacity
    - crew
    - d_check_complete
    - moon_clearance_complete
    - iata_approved
    - company_rating
    - review_scores_rating
````

Here, the parameters **test_size** and **random_state** are used as part of the **train-test split**, and features gives the names of columns in the model input table to use as features.

------------------------------------------------------------------------------------------------------------
**GOOD PRACTICE**: if you run **black src/**, the output is a rewritten code, since **black PEP 8**  is a way to follow PEP 8 standards to create codes.
Output example:
````python
black src/*       
#output:
error: cannot format src\requirements.txt: Cannot parse: 1:5: black~=22.0
reformatted src\setup.py
reformatted src\spaceflights2\pipelines\data_processing\pipeline.py
reformatted src\spaceflights2\pipelines\data_science\nodes.py
reformatted src\spaceflights2\pipelines\data_processing\nodes.py

Oh no! 💥 💔 💥
4 files reformatted, 15 files left unchanged, 1 file failed to reformat.
````
---------------------------------------------------------------------------------------------------------------------------
