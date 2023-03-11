
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


https://user-images.githubusercontent.com/117999669/224502516-157fd0ea-d7bf-4888-9f6f-55fa1e5b1616.PNG






## 1) INSTALL KEDRO 

`````python
#Input
X = df[['TotalSF']] #pandasDataFrame
y= df[['SalesPrice']] #pandasSeries
