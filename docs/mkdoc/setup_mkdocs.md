# Setup mkdocs in MacOS with theme and extension

## Setup Python enviroment

Since the mkdocs is coded by Python, so you have to set up the python env first. Currently the mkdocs's verison is 2.16, it is supproted by python 3.8+.

It's so diffcult to a C deeloper.

- First, you should install python in MacOS.
    - It's not friendly in MacOS, since it was already have one python but with a lower version.
    - you could install the new version by `Brew`, you could retrie the detail by [Brew offical](https://brew.sh/).
- Second, after installed the python by brew, you could check it by command `brew list | grep python`.
    - And you could also find the detail by `brew list python`. which will display the install path.

```text
python
python-setuptools
python@3.11
python@3.13
```

- After that, unfounture, you still saw the old version python by MacOS. this is because the `PATH` can't get the new python. You could replace the old version python by system `alias` in your bash profile.

I added this in the `.bash_profile`, which will be executed when terminate opened.

```text
alias python=python3
alias pip=pip3
```

- You could check your python version, which will display the newest vesion which you installed by brew.

## Setup python virtual env.

- Install the pipenv by brew. This step is simple.

### Setup mkdocs.

We will install the mkdocs by python virtual env, which will isolate the python package for every project.

- Create a new directory for your new project. like `mkdir testPrj`.
- Enter this new directory, and then initliazation the python virtual env by command `cd testPrj & pipenv shell`.
- Install mkdocs in virtual env by command `pipenv install mkdocs`.

#### Initlization the `mkdocs` project.

- Run command `mkdocs new .`. the `.` means current directory.
- Run command `mkdocs serve`, then you could see the output that new project is running. Open the browser to check the wiki page. The url you could get by the output, like `http://127.0.0.1:8000/`.

#### Install theme.

- Go to [mkdocs-material offical](https://squidfunk.github.io/mkdocs-material/getting-started/), you could get the `install`. but don't forget to replace the `pip` command by `pipenv`.
- Run `pipenv install mkdocs-material`.
- Config the `mkdocs.yml`.

#### Install markdown_krokis

- Same with `theme`, you MUST install the `markdown_kroki` first. I use this extension since i want to test the `plantuml` by it.

## Example 

```plantuml
aa -> bb
```