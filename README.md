# =======
llm-lab
=======


## Quick Summary


## Configuration

**1.0 Using conda env**

There are many ways to create virtual environment for python.
This project uses [conda](https://www.anaconda.com/products/distribution) for virtual environment management.

``bash

conda create --prefix .venv python=3.10
conda activate ./.venv
conda install -c conda-forge poetry==1.8.3
poetry install

## pre-commit
pre-commit install &&
    pre-commit autoupdate &&
    pre-commit run -a -v

poetry build

```

**1.1 Data Versioning with DVC**


All models and data used in this project are versioned using [DVC website](https://dvc.org/doc/start/data-management/data-pipelines) | [DVC repository](https://github.com/iterative/dvc).
Please follow the steps bellow concerning the configuration about the remote repository access.

```bash
# you SHOULD do this at least once to set up your aws credentials locally
dvc remote add prod s3://xxxx/release/models/xxx/prod/
dvc remote modify prod credentialpath ~/.aws/credentials --local

# Retrieving data and model from remote repository.
dvc pull -r prod

# Pushing data and model to remote repository.
git add . # adding dvc.lock , dvc.yaml and all code files.
git commit -m "new model"
git push
dvc push -r prod
```

More info [DVC - Data Version Control](https://parrotanalytics.atlassian.net/wiki/spaces/DAT/pages/2373582850/DVC+-+Data+Version+Control)


**1.2 Store config in the environment**


An app’s config is everything that is likely to vary between deploys (staging, production, developer environments, etc).
This application follows the [twelve-factor](https://12factor.net/) metodology, thus, this application stores config in environment variables (often shortened to env vars or env). Env vars are easy to change between deploys without changing any code; unlike config files, there is little chance of them being checked into the code repo accidentally; and unlike custom config files, or other config mechanisms such as Java System Properties, they are a language- and OS-agnostic standard.

This application follows [twelve-factor](https://12factor.net/) aspects, so create the .env file with the following environment variables for development purposes.
Fill the username and passwords with your own.

``` bash

## .env
APP_DATABASE_HOST=<HOST>
APP_DATABASE_PORT=<PORT>
APP_DATABASE_USERNAME=<USERNAME>
APP_DATABASE_PASSWORD=<PASSWORD>
APP_DATABASE=<DATABASE>
```

Run the following command to set up the envinoment variables from .env file created.

```bash
set -a
source .env
set +a
```

More info [Store config in the environment](https://12factor.net/config)

**1.3 Conteinerized environment**


---
## How to run

**2.0 Train model pipeline**


**2.1 Model validation**

**2.2 Model versioning**

With DVC properly configured, the models ready for production can be versioned following the steps bellow.
Make sure that every new model and data pushed to production has a correspondent git tag.

Steps:
1. Commit the new model/data using dvc and git.
  a) model and data: use `dvc push -r`
  b) code: use `git push`
2. Bump the version number located in /conf/params.yml
3. Open a pull request to master branch.
4. Merge master branch with new code and versioned model
5. Create a new tag derived from master branch.


```bash
git add .
git commit -m "chore: Added mymodel.joblib model to dvc"


git push
# Pushing changes to REMOTE
dvc push -r prod

```

**After merge with master branch.**

```bash
git tag -a X.Y.Z -m "message"
git push -f origin X.Y.Z
```


**2.3 Prediction pipeline**

The prediction pipeline was designed as the steps bellow.

---
## Deployment instructions


This project is deployed through <MY CI/CD TOOL>. Every git tag pushed to remote repository triggers the CI/CD pipeline.
The CI/CD pipeline runs the following steps.

> ! ALWAYS REMEMBER TO VERSION AND PUSH THE MODEL TO DVC REPOSITORY BEFORE GENERATING A NEW GIT TAG

More info [12factor - build release run](https://12factor.net/build-release-run)

---
### Who do I talk to? ###

* Repo owner or admin:
  - John Doe <john.doe@company.com>

* Other community or team contact
  - Foo Bar <foo.bar@company.com>
