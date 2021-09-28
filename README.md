#STEPS:
##STEP 01: Create a empty remote repository

##STEP 02: intialize a git local repository and connect to remote repository
*open and project folder in VS code then follow below command -
echo "# dvc-ML-demo-AIOps" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/samruddhijp05/dvc-ML-demo-AIOps.git
git push -u origin main

##STEP 02: create and activate conda environment
'''bash
conda create -n dvc-ml python=3.7 -y
conda activate dvc-ml

##STEP 03 create setup file
touch setup.py
paste the below content in the setup.py file and make the necessary changes as per your user ID-
from setuptools import setup

with open("README.md", "r", encoding="utf-8") as f:
    long_description = f.read()

setup(
    name="src",
    version="0.0.1",
    author="USER_NAME",
    description="A small package for dvc ml pipeline demo",
    long_description=long_description,
    long_description_content_type="text/markdown",
    url="https://github.com/c17hawke/dvc-ML-demo-AIOps",
    author_email="sunny.c17hawke@gmail.com",
    packages=["src"],
    python_requires=">=3.7",
    install_requires=[
        'dvc',
        'pandas',
        'scikit-learn'
    ]
)

##STEP 05: create requirements file and install dependencies
'''bash
touch requirements.txt
pip install -r requirements.txt

##STEP 06 initialize dvc
'''bash
dvc init

##STEP 07 create the basic directory structure
'''bash
mkdir -p src/utils config

##STEP 08 create the config file 
touch config/config.yml
 content of config.yml-
 
data_source: http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-red.csv

artifacts: 
  artifacts_dir: artifacts
  raw_local_dir: raw_local_dir
  raw_local_file: data.csv


##STEP 09: create the stage 01 python file and all_utils file:
touch src/stage_01_load_save.py src/utils/all_utils.py

##STEP 10: create the dvc.yaml file and add the stage 01:
touch dvc.yaml
 content of dvc.yaml file:
 stages:
  load_data:
    cmd: python src/stage_01_load_save.py --config=config/config.yaml
    deps:
      - src/stage_01_load_save.py
      - src/utils/all_utils.py
      - config/config.yaml
    outs:
      - artifacts/raw_local_dir/data.csv

  ##STEP 11: run the dvc repro command
  dvc repo

  ##STEP 12: push the changes to remote repository
  git add .
git commit -m "stage 01 added"
git push origin main
