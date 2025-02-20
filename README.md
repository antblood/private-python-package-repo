# Setting Up a Local Private PyPI Server

This guide provides detailed instructions on how to set up a local private PyPI server using `pypi-server`. This allows you to host and manage Python packages locally, which is useful for testing, development, or internal distribution of packages.

## Components Involved

1. **Python**: The programming language used to create and run the packages.
2. **pip**: Python's package installer, used to install and manage Python packages.
3. **pypi-server**: A minimal, easy-to-install PyPI compatible server for hosting your own packages.
4. **twine**: A utility for securely uploading Python packages to a PyPI server.
5. **setuptools**: A library that helps in packaging Python projects by providing the `setup.py` file functionality.
6. **Local Directory**: A directory on your machine where packages will be stored and served.

## Prerequisites

- Python installed on your machine
- `pip` installed for package management
- Basic understanding of Python packaging

## Step-by-Step Instructions

### Step 1: Install `pypi-server`

First, ensure that you have Python and pip installed on your machine. Then, install `pypi-server`:

```bash
pip install pypi-server
```

### Step 2: Create a Directory for Packages

Create a directory to store the packages you want to host:

```bash
mkdir ~/local_pypi_packages
```

### Step 3: Run pypi-server

Launch the server to start serving packages from the specified directory. This will listen on port 8080 by default:

```bash
pypi-server -p 8080 ~/local_pypi_packages
```

This command starts a local HTTP server at http://localhost:8080.

### Step 4: Create a Python Package

Create a simple Python package with the following structure:

```bash
my_package/
├── my_module.py
└── setup.py
my_module.py: Contains your Python code.
```

```python

def hello():
    print("Hello from my_package!")
```

setup.py: Defines the package metadata and dependencies.

```python

from setuptools import setup, find_packages

setup(
    name='my_package',
    version='0.1',
    packages=find_packages(),
    install_requires=[],
)
```

### Step 5: Build the Package

Navigate to your package directory and create a source distribution:

```bash
cd my_package
python setup.py sdist
```

This will create a dist directory containing the package archive, e.g., my_package-0.1.tar.gz.

### Step 6: Upload the Package

Install twine if it's not already installed, and then upload your package to the local PyPI server:

```bash
pip install twine
twine upload --repository-url http://localhost:8080/ dist/*
```

### Step 7: Configure pip to Use Your Local PyPI

Modify the pip configuration file to use your local PyPI server:

Linux/MacOS: ~/.pip/pip.conf
Windows: %APPDATA%\pip\pip.ini
Add the following configuration:

```ini

[global]
index-url = http://localhost:8080/simple
```

Optionally, you can add a fallback to the public PyPI:

```ini

[global]
index-url = http://localhost:8080/simple
extra-index-url = https://pypi.org/simple
```

### Step 8: Test the Setup

Install the Package:

Test installing your package from the local server:

```bash
pip install my-package
```

Verify the Installation:

Ensure the package is installed and functioning:

```python

import my_module

my_module.hello()  # This should print: "Hello from my_package!"
```

## Conclusion

By following these steps, you have successfully set up a local private PyPI server, created and uploaded a package, and configured your environment to install from this server. This setup is ideal for testing and internal package distribution.
