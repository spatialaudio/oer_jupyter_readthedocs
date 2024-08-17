Step by Step - Creating OER with Jupyter, Git ...
=================================================

Jupyter Notebooks
-----------------

Jupyter notebooks are structured in cells. Cells can have different types. 
Markdown cells use the markdown language to allow you to make comprehensive lecture notes, descriptions and code documentation. 
You can incorporate latex style equations as well as figures. Some html syntax can be used too.
Code cells let you write and execute source code of different programming languages, depending on the chosen kernel.
The output will be displayed right below the cell. Cells can be executed in any order.

.. 
    This tutorial assumes that you are familiar with jupyter notebook and its advantages.

For an introduction to jupyter notebook please refer the `official documentation`_.
**!TODO: refine - maybe link good tutorials...**

.. image:: data/placeholder_jupyter_structure.png
   :height: 200 px
   :width: 200 px
   :align: right 

The general structure used in this tutorial is shown on the right. A notebook called :code:`index.ipynb` provides a table of contents and direct links to the other jupyter notebook representing subsections.
**!TODO: replace placeholder figure?**


Github
-------------------------------

We will use GitHub to make our notebooks accessible and findable. GitHub is based on the `git`_ version control system. It allowes for convinient revision and authorship tracking and provides tools for collaboration.
Users can help revise and refine the materials through GitHub's issue and pull-request mechanisms. You can read more on the basics of GitHub and git `here`_.
There are also plenty of helpful tutorials availible. You can find some of them in this `list`_. There are also `interactive materials`_ available on the topic.

Collaboration and Authorship tracking
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

GitHub's Issues functionality can be used to track issues, answer questions people might have and receive enhancement proposals or feature requests. 
Through the Pull request mechanism other people can contribute their own changes to the code. All changes and who contributed them will be tracked. 
Finally others can fork the repository to change the contents to better fit their needs or remix them with other materials.

Continuous Integration
^^^^^^^^^^^^^^^^^^^^^^

`Continuous Integration (CI)`_  describes the process of integrating source code and automatically building and testing the software throughout development. This can help ensuring the quality and functionaltity of your OER materials.
GitHub provides this functionality through its `Actions`_ feature. 

GitHub Actions uses workflows configured in `YAML`_ files ending on :code:`.yml` or :code:`.yaml`. These files have to be placed in your repository at :code:`.github/workflows/`. They contain information on when to be run, what to run and with what packages and versions. 
They can be executed be `GitHub hosted runners`_ or self hosted runners. 

You can use this to automatically check your project for programming errors whenever you change something. It can also `lint`_ your notebooks at the same time.

A :code:`notebook_ci.yml` workflow file, that runs a `sphinx`_  build with `nbsphinx`_ extension on certain conditions to execute all code in the notebooks could look like this:

.. code-block:: yaml

    name: Sphinx build

    on:
    push:
        branches:
        - main  # Change this to the repository's main branch
    pull_request:
        branches:
        - main  
    
    schedule:
        - cron:  '30 1 1,15 * *' # POSIX cron syntax

This first part specifies the name of the workflow and on which events it should be executed. 
In this case it will be run whenever a push or pull-request is done to the main branch of the repository. 
It is also run by a schedule on every 1. and 15. day of the month at 1:30 o clock.

The next part defines what is done and what the runner needs to run it (e.g. a specific operating system or python version).

.. code-block:: yaml

    jobs:
        linux_py3_12:
            runs-on: ubuntu-latest
        
            steps:
            - uses: actions/checkout@v3
            - uses: actions/setup-python@v3
                with:
                python-version: 3.12
            - run: sudo apt-get install libsndfile1 pandoc
            - run: pip install -r requirements.txt
            - run: pip install -r ci/requirements.txt   
            - run: python -m sphinx . _build/ -b html

As you can see the this workflow will also automatically install all requirements from a :code:`requirements.txt`.
The :code:`notebook_ci.yml`, :code:`requirements.txt`, as well as a :code:`lint_nb.yml` file, that can be used for automatic linting, are included in the provided `cookiecutter template`_. 
You have a choice to include them on project creation. More on that in the cookiecutter template section.
**TBC...**

Referencing
^^^^^^^^^^^

For others to be able to properly reference your OER you should mark a well maintained state of your repository as a release. 
Additionally you can attach a `digital object identifier (DOI)`_ through services like `zenodo`_.

Interacting with Notebooks
--------------------------

Now that the collection of notebooks is accessible, there are quite a few ways to interact with it.

Local
^^^^^
Cloning the repository and using the materials locally provides the best interactivity to the user, enabling them to execute computational examples in the notebooks, make changes and save those changes.
But this also has the highest barrier to entry because of the need of local installations of Jupyter and other packages used.


You can help your users by providing a :code:`requirements.txt` file or :code:`environment.yml` file, that can be used with `pip`_ or `conda`_ respectivly to install all needed packages or create an environment with them included.
You can use pip to automatically create a :code:`requirements.txt` by running:
.. code-block:: 

    pip freeze > requirements.txt

This will list all packages installed in your environment with :code:`pip install`. If you just want to list all the packages used in the project, based on imports, you can use the `pipreqs`_ package.
.. code-block:: 

    pip install pipreqs
    # Run in your project directory
    python -m  pipreqs.pipreqs .



(my)Binder
^^^^^^^^^^

With binder you can make your notebooks avaible to be interacted with within an online environment. You can use `mybinder.org`_ to host the notebooks for you with no futher requirements than your already available puplic repository.
Binder allows for the notebooks to be used and executed like you would with a local installation. The only downside is, that you are unable to save your changes.
You simply need to enter the link to your public GitHub repository, specifiy which branch or commit to use and optionally provide a link to the notebook that should be used as a landing page (for example your :code:`index.ipynb`).
Mybinder will then generate a shareable link to your interactive binder session.

You can also set up your own binder server, if the ressources provided by `mybinder.org`_ are not sufficient. Read more on that in the `binder documentation`_.



nbviewer
^^^^^^^^

The nbviewer service provides static versions of your notebook collection. 
Similar to the way `mybinder.org`_ works, all you have to do is enter the link to your public repository on `nbviewer.org`_ and you will get a link to static renderings of your notebooks. 
This is convinient for having a first look at the materials without any prior knowlege of jupyter but it doesn't allow for any interactivity.



Export with nbsphinx
^^^^^^^^^^^^^^^^^^^^

With `nbsphinx`_, a `sphinx`_ extension, you can produce HTML or LaTeX output from your collection of notebooks. 
Please refer to the nbsphinx `installation page`_ to install all necessary packages. 

For nbshinx to render the table of contents correctly you need to use the :code:`nbsphinx-toctree` tag as metadata for the :code:`index.ipynb` file's Markdown cells.
You can also use the tag to specify cells that should not be rendered in the final output. 
To edit a cells metadata in jupyter notebook you have to toggle the Cell Toolbar to Edit Metadata through the View tab: View -> Cell Toolbar -> Edit Metadata.
Or, if your using JupyterLab, the cell metadata is available through the Property Inspector tab (marked by two small cogwheels in the top right corner) under advanced tools.
The :code:`nbsphinx-toctree` metadata tag should be added to all individual cells containing a list of links to other notebooks.
An example of what to add is shown below. 

.. image:: data/metadata_jupyter_toc.png
    :width: 80%
    :align: center


If you have general information about the collection or copyright in every notebook as a header or footnote, you can hide these when rendering to pdf or html, so they dont appear every chapter. 
This is done with the :code:`"nbsphinx": "hidden"` tag in the cells metadata.

You can find more information on this in the `nbsphinx documentation`_.

.. _git: https://git-scm.com
.. _official documentation: https://jupyter-notebook.readthedocs.io/en/latest/
.. _here: https://docs.github.com/en/get-started
.. _list: https://gist.github.com/jaseemabid/1321592
.. _interactive materials: https://learngitbranching.js.org/?locale=en_US
.. _continuous Integration (CI): https://en.wikipedia.org/wiki/Continuous_integration
.. _Actions: https://docs.github.com/en/actions
.. _YAML: https://yaml.org
.. _GitHub hosted runners: https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners
.. _lint: https://en.wikipedia.org/wiki/Lint_(software)
.. _pip: https://pip.pypa.io/en/stable/
.. _conda: https://docs.conda.io/projects/conda/en/stable/
.. _pipreqs: https://github.com/bndr/pipreqs
.. _mybinder.org: https://mybinder.org
.. _binder documentation: https://mybinder.readthedocs.io/en/latest/
.. _nbviewer.org: https://nbviewer.org
.. _nbsphinx: https://nbsphinx.readthedocs.io/
.. _sphinx: https://www.sphinx-doc.org/en/master/
.. _installation page: https://nbsphinx.readthedocs.io/en/0.9.4/installation.html
.. _nbsphinx documentation: https://nbsphinx.readthedocs.io/en/0.9.4/subdir/toctree.html
.. _cookiecutter template: https://github.com/spatialaudio/cookiecutter_oer_jupyter
.. _digital object identifier (DOI) : https://www.doi.org/the-identifier/what-is-a-doi/
.. _zenodo: https://zenodo.org
