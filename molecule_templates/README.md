# Overview of provided cookiecutter templates #

## What is CookieCutter ##

From the [CookieCutter website](https://github.com/cookiecutter/cookiecutter) :

&emsp;*"command-line utility that creates projects from cookiecutters (project
templates), e.g. creating a Python package project from a Python package project
template."*

CookieCutter can be used to build out complex folder and project structure using
JNinja based syntax. In our case we are using them to build Azure-based VM Ansible
testing framework projects, using user input to customize deployment.

## New Ansible Role Templates ##

To help make creating Azure-based VM testing easier, we can use the provided
cookiecutter template to create a completely new Ansible role.

The new_windows_azure_role_template and new_linux_azure_role_template templates
create Windows and Linux based Anisble/Molecule roles. This includes all the folder
structure for the Ansible role and Molecule testing.

Two input variables are defined for the template in the cookiecutter.json file. The
first asks for the new roles name, the second the prefix to be used when creating
the Azure resources.

From the two defined input variables various resource name values are generated and
placed in the Molecule files.

## Existing Ansible Role Templates ##

To make adding Molecule testing to existing Ansible roles, we can use the provided
cookiecutter template to add the Molecule file and directory structure to an already
existing Ansible role.

The exisitng_windows_azure_role_template and the existing_azure_role_template
templates create the Molecule file structure in an already existing Ansible Role.
The template must be called from the roles' head directory and the name input provided
must match the name of the role you are trying to add Molecule in to.

Two input variables are defined for the template in the cookiecutter.json file. The
first asks for the new roles name, the second the prefix to be used when creating
the Azure resources.

From the two defined input variables various resource name values are generated and
placed in the Molecule files.

## Existing Ansible Playbook Templates ##

To make Molecule testing of existing Ansible playbooks, we can use the provided
cookiecutter template. The template adds a new Molecule scenario for the specified
playbook with the needed Molecule files and structure to deploy Azure-based testing
infrastructure.

The exisitng_windows_azure_playbook_template and the existing_azure_playbook_template
templates create the Molecule file structure needed to test the specified Anisble
playbook. The template must be called from the Anible playbooks head directory with
the name input provided matching the name of the playbook you wish to test.

Two input variables are defined for the template in the cookiecutter.json file. The
first asks for the new roles name, the second the prefix to be used when creating
the Azure resources.

From the two defined input variables various resource name values are generated
and placed in the Molecule files.
