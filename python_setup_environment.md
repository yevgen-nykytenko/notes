Python setup guide
---


1. Download latest python from https://www.python.org/downloads
After installation check whether binary folder was added to environment PATH variable (user's or system)

2. To ensure installation went sucessfully run following commands:

    ```python --version```
    ```pip --version ```
    ```python -m ensurepip --default-pip```
    
    More info is [here](https://packaging.python.org/tutorials/installing-packages/#requirements-for-installing-packages).

3. Setup [Wheel](https://github.com/pypa/wheel) tool:

    ```python -m pip install --upgrade pip setuptools wheel```

4. Building and deploying packages [guide](https://packaging.python.org/guides/distributing-packages-using-setuptools)

    4.1. Clear **dist** folder content.
    
    4.2. Build package. Run 
    ```python setup.py sdist bdist_wheel```
    command in folder with setup.py file

5. Setup [Twin](https://twine.readthedocs.io/en/latest) tool for uploading packages to test.pypi

   ```python -m pip install --user --upgrade twine```
   
   NOTE: Twine won't be added automatically to PATH environment variable. You should add it manually
**C:\Users\\*UserName*\AppData\Roaming\Python\Python37\Scripts**

6. Install [KeyRing](https://github.com/jaraco/keyring) tool

   ```pip install --user keyring```

7. Create keyring. This command will ask for credentials for the first time.

   ```keyring set https://test.pypi.org/legacy/pypi_username```
   
   or
   
   ```python -m keyring set https://test.pypi.org/legacy/pypi_username```
   
8. Create environment variables

Goto **System Properties** -> **Environment variables** -> **User variables** and add

    | Variable name  | Value |
    | ------------- | ------------- |
    | TWINE_USERNAME  | *pypi_username*  |
    | TWINE_PASSWORD  | *pypi_password*  |

9. Upload package to test
```twine upload --repository-url https://test.pypi.org/legacy/ -u pypi_username dist/*```
Probably you will need restart PowerShell or run command one more time before it will not ask for password next time

Additional information
---
There're more options for packages upload:

1. ```twine upload --repository-url https://test.pypi.org/ -u pypi_username -p pypi_password dist/*```
2. ```twine upload --repository-url https://test.pypi.org/legacy/ dist/*```
3. ```twine upload --repository-url https://test.pypi.org/legacy/ --username pypi_username  --password pypi_password dist/*```
4. ```twine upload --repository-url https://test.pypi.org/legacy/ --config-file config.pypirc dist/*```


Running Unit Tests from separate folder will require to import package or extend system pathes to folder with package sources.
You will need to deploy package to repository and install it locally. Alternative solution is to insert absolute path to folder with package 

```import sys```
```sys.path.insert(0, "/path/to/your/package_or_module")```

10. Install package from test repository

```pip install --index-url https://test.pypi.org/simple  PACKAGE-NAME```

Alternative way to force install
```pip install --upgrade --force-reinstall PACKAGE-NAME```

11. Run all unit tests in folder
```call python -m unittest discover PATH-TO-UNIT-TESTS```
