[![status-badge](https://ci.codeberg.org/api/badges/15636/status.svg)](https://ci.codeberg.org/repos/15636)
# Zagreus docs

A centralized place for storing various configurations and docker compose files for the various apps I run in my server. The documentation will also serve as a sort of wiki if you are looking to setup these apps on your own.

## Development

If you wish to fork this repo and modify the source code you may do so by following these steps:

1. As this project is made with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/). You will need to have python installed and have created a virtual environment.

To see if you have python installed to your host machine, use this command:
```bash
python --version
```
If you get an error, please go to [https://www.python.org/downloads/](https://www.python.org/downloads/) to download the latest version of python.

After installing python, create a virtual environment:
```bash
python -m venv <directory>
```
Then cd into it:
```bash
cd <directory> 
```
or
```bash
cd !$
```
Then you need to run the activation script in order to start the environment. This will depend on what shell you are running. 

If you are on linux you should be able to run the `activate` script with no file extenion:

```bash
./bin/activate
```
I run the fish shell on my machine and will instead need to use `source bin/activate.fish` in order to activate the environment.


2. Clone the repo into your virtual environment.

```bash
git clone https://codeberg.org/D1noD3v/zagreus-docs.git
```

3. Install all the necessary packages required for this project.
```bash
pip install -r requirements.txt
```

4. You are now able to server the site locally!
Run the command `mkdocs serve` in order to serve the website locally from your host. It will listen on `http://127.0.0.1:8000`.

If you wanna know more on how to change the look of the website I recommend going to [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and looking at their in-depth documentation.

## Problems?
If you have any problems with getting the website running you may submit an issue to this repo or go to the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) for help.

# License
This software is licensed under the MIT license. If you wish to know more about this license, please visit [https://en.wikipedia.org/wiki/MIT_License](https://en.wikipedia.org/wiki/MIT_License)
