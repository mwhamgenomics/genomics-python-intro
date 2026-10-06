---
title: Setup
---

This course provides an introduction to programming in Python, with a focus on manipulating and visualising genomic data. As such,
we'll need:

- a Python installation
- some example data to expertiment with

## Example data

This course is based on the same example data as the [official Carpentries lesson on R for Genomics](https://github.com/datacarpentry/genomics-r-intro). The main file we'll be using is [combined_tidy_vcf.csv](../episodes/data/combined_tidy_vcf.csv)



## Software Setup

::::::::::::::::::::::::::::::::::::::: discussion

### Details

Python can be installed in one of several forms:

- A managed JupyterHub instance at your organisation
- A JupyterLab instance on your local machine
- A local Python interpreter

Jupyter is a powerful way of saving your progress and displaying your work in Python, and is the recommended way of following
through this course.

:::::::::::::::::::::::::::::::::::::::::  callout

### Noteable at the University of Edinburgh

University of Edinburgh staff and students have access to Noteable at https://noteable.edina.ac.uk/login. If you wish to use
this, log in here and proceed to the setup instructions for JupyterHub.

::::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: solution

### JupyterHub

Once logged in, select ‘New -> Folder’ in the top right menu. This will create a new folder called ‘Untitled’. To rename it,
check the box next to it, select ‘Rename’ in the top left and enter a suitable name for the folder. Navigate into it by selecting
its name in the navigator.

Now that you’re in a sensible location, you can use the ‘Upload’ button in the top right to select and upload the test data to this space.

In the folder you’ve created, select ‘New’ -> ‘Python 3 (ipykernel)’. This will open a Jupyter notebook in a new browser tab.

:::::::::::::::::::::::::

:::::::::::::::: solution

### JupyerLab

If you don’t have access to a JupyterHub instance but still want to use Jupyter, you can install it locally either with Conda:

    conda install -c conda-forge jupyterlab

Or with pip:

    pip install jupyterlab

Once installed, start Jupyter with:

    jupyter-lab

This will start the server process and open JupyterLab in a new browser window. Follow the same GUI or graphical folder setup as for JupyterHub above.

You may need to install some extra packages, such as pandas and matplotlib. To do this, exit Jupyter and in the same terminal window, run:

    pip install pandas matplotlib

Or in Conda:

    conda install pandas matplotlib

:::::::::::::::::::::::::


:::::::::::::::: solution

### Local Python interpreter

If you wish to use Python on its own, then that is also possible, although you will need to install the dependencies yourself.

It's almost always a good idea to install packages into a
[virtual environment](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#creating-a-virtual-environment)
such as those created by Python's `venv` module:

    python -m venv ./new_python_environment
    source ./new_python_environment/bin/activate
    pip install pandas matplotlib

Or if you prefer Conda:

    conda create -n new_conda_environment -c conda-forge python pandas matplotlib
    conda activate new_conda_environment


Then to start up Python, run the command:

    python

:::::::::::::::::::::::::
