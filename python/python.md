# Python

## Virtual Environment
Make sure you have virtualenv installed
```
pip install virtualenv
python -m pip install --user virtualenv
```

Rather than storing all libaries in a global folder, we store them locally based on the project.

To create a virtualenv:
```
virtualenv venv
# or
python3 -m venv venv
```
This is create a folder venv where the virtual environment will be.

To active:
```
cd venv
source bin/activate
```

The good way to use virtual env
```
# Create project folder
mkdir myproject && cd myproject

# create venv
pyvenv venv

# ignore venv
echo 'venv' > .gitignore

# activate venv
source venv/bin/activate

pip install XYZABC

# write dependencies to file
pip freeze > requirements.txt

# to install dependencies in file
pip install -r requirements.txt
```
