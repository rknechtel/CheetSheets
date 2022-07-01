
# Bandit  

**Overview**  

Bandit is a tool designed to find common security issues in Python code. To do this Bandit processes each file, builds an AST from it, and runs appropriate plugins against the AST nodes.  
Once Bandit has finished scanning all the files it generates a report.  
Bandit was originally developed within the OpenStack Security Project and later rehomed to PyCQA.  


**Install Bandit**  

**Create a virtual environment (optional)**  

```bash
python3 -m venv bandit-env
```

**OR**  

```
virtualenv bandit-env
```

**Then activate it**  

```bash
source bandit-env/bin/activate
```

**Install Bandit**  

```
pip3 install bandit
```

**Run Bandit**  

```bash
bandit -r path/to/your/code
```

**Reference**  

[Github URL](https://github.com/PyCQA/bandit/ 'bandit Github URL')


