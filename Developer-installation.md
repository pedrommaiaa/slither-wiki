Use [virtualenv](https://virtualenvwrapper.readthedocs.io/en/latest/) to install a developer version of Slither (up-to-date with `master`):
```
pip3 install virtualenvwrapper
source /usr/local/bin/virtualenvwrapper.sh
mkvirtualenv --python=`which python3` slither-dev
git clone https://github.com/trailofbits/slither
cd slither
python setup.py develop
```

Start a shell with the Slither virtual environment by running:
```
workon slither-dev
```

Update Slither by running `git pull` from the slither directory.


If you are having problems insatlling on the macos m1, follow the next steps:

1. Download Python 3.9.x from python.org
2. Open your project with VS code
3. On Terminal run `python3 -m venv env` to create virtual env
4. Then run `source env/bin/activate`
5. In VS Code, open the Command Palette (View > Command Palette or (⇧⌘P)). Then select the Python: Select Interpreter command:
6. Choose the python version i.e. `Python 3.9.x 64-bit ('.venv':venv)` you downloaded
7. On Terminal run `python -m pip install --upgrade pip`
8. Then run `python -m pip install slither-analyzer`
9. Finally run `slither .`
