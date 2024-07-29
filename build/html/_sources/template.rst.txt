Template for cookiecutter
=========================
This sections discusses the `cookiecutter`_ template, that can be found `here`_.

.. _cookiecutter: https://www.cookiecutter.io
.. _here: https://github.com/spatialaudio/cookiecutter_oer_jupyter

What is cookiecutter
--------------------

Cookiecutter is a tool to create project-templates. With these templates (or cookiecutters) you can quickly create a barebones structure for your project, 
including a general folder structure and initial files that can be automatically costumised to fit the project.
It also allows hooks to be executed at several points during the creation. Hooks can be python or shell scripts.

What does this template do?
---------------------------

The template provided here is suposed to give you a head start while creating your OER either from scratch, or from a previously created collection of jupyter notebooks.
It will create a basic folder structure to organize the materials **!TODO: shown in figre...!**. It also adds files providing functionality according to your choice.
Those may include:

    * A :code:`LICENSE` file with your prefered license text.
    * files providing CI functionality through GitHub Actions. These will be stored in :code:`.github/workflows`.
    * A :code:`README.md` file that already has some info about the project in it. It will be displayed by GitHub on the repositories landing page.
    * A :code:`conf.py` used in building a static representation of the collection as html or pdf through sphinx with the nbsphinx extension.        
    * A :code:`.gitignore` 
    * A :code:`requirements.txt` file. This file is generated as a placeholder. You have to fill                                                                                                                                      
    * **!TODO: check for completeness!**                                  
                                     
The template can also set up and initialize a local git repository, as well as a remote GitHub repository, through post gen hooks. You need to have git and GitHub CLI installed for this to work.


Installation         
------------

Apart from a `python 3 installation`_ you also need to install cookiecutter.
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


Or you can tell cookiecutter to find the template on github:

.. code-block::

    cookiecutter gh:https://github.com/spatialaudio/cookiecutter_oer_jupyter

Now you will be prompted to input some values to configure your project. The prompts and what they do is listed below.

directory_name
    The name of the directory that will be created.

project_name
    The name of the project. This will default to the directory name given above.

contact_name
    Your Name

contact_mail
    Your e-mail adresse

year
    The year of release

create_git
    Choose if a local git repository should be initialised. Requires git installation.

create_remote_and_push
    Choose if a remote repository should be created on GitHub. The local initial commit will be pushed. 
    Only works if create_git is answered with Yes. Requires GitHub CLI installation and authentification by running :code:`gh auth login`.
    The :code:`directory_name` is used as the name of the repository.

github_organization
    The name of your GitHub organization. If you dont want to create a repository within an organization, leave it as :code:`None`.

git_visibility
    Choose between public, private and internal for your GitHub repositorys visibility. As our aim is to create OER this should always be public.

push_existing_remote
    If you have already created a GitHub repository beforehand you can choose to push to that instead. 

git_remote
    The link to your pre-existing repository. To avoid conflicts the repository should be empty.

license
    Choose an open license. More on that in the How to choose a license section. A :code:`LICENSE` file will be automatically created inlcuding the choosen licenses text.

include_ci
    Choose to create files for Continiuos Intigriation through GitHub actions.

**!TODO: Options for importing existing notebooks?!**