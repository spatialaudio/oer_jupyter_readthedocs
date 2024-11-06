.. _template:

Template for Cookiecutter
=========================

This sections discusses the `cookiecutter`_ template, that can be found `here`_.

.. _cookiecutter: https://www.cookiecutter.io
.. _here: https://github.com/spatialaudio/cookiecutter_oer_jupyter

What is cookiecutter
--------------------

Cookiecutter is a tool to create project-templates. With these templates (or Cookiecutters) you can quickly create a bare-bones structure for your project, 
including a general folder structure and initial files that can be automatically customized to fit the project.
It also allows hooks to be executed at several points during the creation. Hooks can be python or shell scripts.

What does this template do?
---------------------------

The template provided here is supposed to give you a head start while creating your OER either from scratch, or from a previously created collection of Jupyter notebooks.
It will create a basic folder structure to organize the materials as shown below.  

::

    .
    ├── .binder
    ├── .github
    ├── .gitignore
    │
    ├── ci
    │   ├── README.md
    │   └── requirements.txt
    │
    ├── data
    │   └── example.png
    │
    ├── introduction
    │   └── introduction.ipynb
    │
    ├── conf.py
    ├── index.ipynb
    ├── index.rst
    ├── LICENSE
    ├── README.md
    └── requirements.txt


It also adds files providing functionality according to your choice.
Those may include:

    * A :code:`LICENSE` file with your preferred license text.
    * files providing CI functionality through GitHub Actions. These will be stored in :code:`.github/workflows` and :code:`.ci/`.
    * A :code:`README.md` file that already has some info about the project in it. It will be displayed by GitHub on the repositories landing page.
    * A :code:`conf.py` used in building a static representation of the collection as HTML or PDF through sphinx with the nbsphinx extension. It contains configuration information for sphinx and nbsphinx. A basic version is included with the template. It is highly recommended, that you customize it to your project.
    * A :code:`index.rst` used in building a static representation of the collection as HTML or PDF through sphinx with the nbsphinx extension. It tells sphinx what notebooks to include and how to build a table of contents from them.
    * A :code:`.gitignore` 
    * A :code:`requirements.txt` file. This file is generated as a placeholder. You have to either fill it with your own requirements manually or use a tool like `pipreqs`_ to generate a replacement.

The template can also set up and initialize a local git repository, as well as a remote GitHub repository, through post gen hooks. You need to have git and GitHub CLI installed for this to work.

.. _pipreqs: https://pypi.org/project/pipreqs/

Installation 
------------

Apart from a `python 3 installation`_ you also need to install Cookiecutter.
You can use pip:

.. code-block:: 

    pip install --user cookiecutter


Or conda:

.. code-block::

    conda install -c conda-forge cookiecutter

For further installation advice please refer to the `official documentation`_.


.. _python 3 installation: https://www.python.org/downloads/
.. _official documentation: https://cookiecutter.readthedocs.io/en/2.0.2/installation.html

How to use the template
-----------------------

To use the template to create your new project you have to open a terminal and navigate to the folder you want to create the project folder in.
Now you can either download the template from GitHub and use it from the directory you saved it in:

.. code-block:: 

    cookiecutter <your_template_folder>/cookiecutter_oer_jupyter


Or you can tell Cookiecutter to find the template on GitHub:

.. code-block::

    cookiecutter gh:https://github.com/spatialaudio/cookiecutter_oer_jupyter

Now you will be prompted to input some values to configure your project. The prompts and what they do is listed below.

directory_name
    The name of the directory that will be created.

project_name
    The name of the project. This will default to the directory name given above. It should be changed to a name not containing special character. Otherwise, characters like '_' can lead to unwanted behavior when using latex conversion with sphinx.

project_description 
    A short description of the projects topic and/or contents.

contact_name
    The persons name who is the main contact for the project.

contact_mail
    The corresponding e-mail address.

copyright_name
    The name or names the copyright should be attributed to.

year
    The year of release

import_notebooks
    If you already have a collection of notebooks, you can choose to include them here. 
    They will be copied from the path given for notebook_collection_path.
    They will replace the placeholder/example :code:`index.ipynb` notebook, :code:`index.rst`, :code:`requirements.txt`, :code:`introduction` folder and notebook, as well as the :code:`data` folder.
    If you want to use sphinx, you will have to create your own :code:`index.rst`.

notebook_collection_path
    The path to your collection of Jupyter notebook. Should be a folder containing all necessary materials. The folder itself won't be copied, only its contents.

create_git
    Choose if a local git repository should be initialized. Requires `git installation`_.

create_remote_and_push
    Choose if a remote repository should be created on GitHub. The local initial commit will be pushed. 
    Only works if create_git is answered with Yes. Requires `GitHub CLI installation`_ and authentication by running :code:`gh auth login`.
    The :code:`directory_name` is used as the name of the repository.

github_organization
    The name of your GitHub organization. If you don't want to create a repository within an organization, leave it as :code:`"None"`.

git_visibility
    Choose between public, private and internal for your GitHub repositories visibility. As our aim is to create OER this should always be public.

push_existing_remote
    If you have already created a GitHub repository beforehand you can choose to push to that instead. 

git_remote
    The link to your pre-existing repository. You can leave this as :code:`"None"` if you have chosen to not use an existing remote repository. To avoid conflicts the repository should be empty.

license
    Choose how you want to license your collection. You have the choice to have to separate licenses for code and text with :code:`"Split"` or choose to use a single license set in either :code:`license_code` with :code:`"Code"` or :code:`license_text` with :code:`"Text"`. A :code:`LICENSE` file will be automatically created including the chosen licenses text(s).

license_code
    Choose a license for the source code in your notebooks or utility files.

license_text
    Choose a license for your text and figures in the notebooks.

include_ci
    Choose to create files for Continuous Integration through GitHub actions.
    This includes the following files and folders:

    - :code:`.github/workflows/lint_nb.yml` - linting workflow
    - :code:`.github/workflows/notebooks_ci.yml` - executing all notebook cells with sphinx
    - :code:`requirements.txt` - empty placeholder that you can fill with your requirements
    - :code:`ci/requirements.txt` - empty placeholder that you can fill with your requirements
    - :code:`ci/README.md`


.. _git installation: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
.. _GitHub CLI installation: https://github.com/cli/cli#installation