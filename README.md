# Testing with Molecule #

1. [Setting up and using the Developement environment](#setting-up-and-using-the-developement-environment)

    a. [Using the Provided Development Container](#using-the-provided-development-container)

    1. [System requirements](#system-requirements)
    2. [Install Docker](#install-Docker)
    3. [Install Visual Studio Code](#install-visual-studio-code)

2. [Understanding the Molecule structure](#understanding-the-molecule-structure)

    a. [About Ansible Molecule](#about-ansible-molecule)

    b. [Molecule structure in an Anisble Playbook](#molecule-structure-in-an-anisble-playbook)

    1. [Playbook Molecule.yml](#playbook-molecule.yml)

        i. [Playbook Molecule.yml Linux](#playbook-molecule.yml-linux)

        ii. [Playbook Molecule.yml Windows](#playbook-molecule.yml-windows)

    2. [Playbook Create.yml](#playbook-create.yml)
    3. [Playbook Destroy.yml](#playbook-destroy.yml)
    4. [Playbook Verify.yml (Windows)](#playbook-verify.yml-(windows))
    5. [Playbook Test_empty.py (Linux)](#playbook-test_empty.py-(linux))

    c. [Molecule structure in an Anisble Role](#molecule-structure-in-an-anisble-role)

    1. [Role Molecule.yml](#role-molecule.yml)

        i. [Role Molecule.yml Linux](#role-molecule.yml-linux)

        ii. [Role Molecule.yml Windows](#role-molecule.yml-windows)

    2. [Role Create.yml](#role-create.yml)
    3. [Role Destroy.yml](#role-destroy.yml)
    4. [Role Verify.yml (Windows)](#role-verify.yml-(windows))
    5. [Role Test_*.py (Linux)](#role-test_*.py-(linux))

3. [Testing a Windows image](#testing-a-windows-image)

    a. [Creating a new Role (Windows)](#creating-a-new-role-(windows))

    b. [Adding Molecule structure an existing Ansible role (Windows)](#adding-molecule-structure-an-existing-ansible-role-(windows))

    c. [Using an already existing Ansible Playbook (Windows)](#using-an-already-existing-ansible-playbook-(windows))

    d. [Writing an Ansible test for a Windows VM](#writing-an-ansible-test-for-a-windows-vm)

    1. [Creating a Molecule test for an Ansible role (Windows)](#creating-a-molecule-test-for-an-ansible-role-(windows))
    2. [Calling Ansible roles' Molecule tests from an Ansible Playbook Molecule testing](#calling-ansible-roles'-molecule-tests-from-an-ansible-playbook-molecule-testing)

    e. [Running Molecule (Windows)](#running-molecule-(windows))

    1. [Login to Azure (Windows)](#login-to-azure-(windows))
    2. [Creating the VM with Molecule create (Windows)](#creating-the-vm-with-molecule-create-(windows))
    3. [Executing the Ansible Playbook/Role with Molecule converge (Windows)](#executing-the-ansible-playbook/role-with-molecule-converge-(windows))
    4. [Verifing/Testing the provisond VM with Molecule verify (Windows)](#verifing/testing-the-provisond-vm-with-molecule-verify-(windows))
    5. [Linting the Ansible code with Molecule lint (Windows)](#linting-the-ansible-code-with-molecule-lint-(windows))
    6. [Tearing down the VM with Molecule destroy (Windows)](#tearing-down-the-vm-with-molecule-destroy-(windows))
    7. [Doing it all with Molecule test (Windows)](#doing-it-all-with-molecule-test-(windows))

4. [Testing a Linux image](#testing-a-linux-image)

    a. [Creating a new Role (Linux)](#creating-a-new-role-(linux))

    b. [Adding Molecule structure an existing Ansible role (Linux)](#adding-molecule-structure-an-existing-ansible-role-(linux))

    c. [Using an already existing Ansible Playbook (Linux)](#using-an-already-existing-ansible-playbook-(linux))

    d. [Writing an Anisble test for a Linux VM](#writing-an-anisble-test-for-a-linux-vm)

    1. [Creating a Molecule test for an Ansible role (Linux)](#creating-a-molecule-test-for-an-ansible-role-(linux))
    2. [Calling Ansible roles' Molecule tests from an Ansible Playbook Molecule testing (Linux)](#calling-ansible-roles'-molecule-tests-from-an-ansible-playbook-molecule-testing-(linux))

    e. [Running Molecule (Linux)](#running-molecule-(linux))

    1. [Login to Azure (Linux)](#login-to-azure-(linux))
    2. [Creating the VM with Molecule create (Linux)](#creating-the-vm-with-molecule-create-(linux))
    3. [Executing the Anisble Playbook/Role with Molecule converge (Linux)](#executing-the-anisble-playbook/role-with-molecule-converge-(linux))
    4. [Verifing/Testing the provisond VM with Molecule verify (Linux)](#verifing/testing-the-provisond-vm-with-molecule-verify-(linux))
    5. [Linting the Anisble code with Molecule lint (Linux)](#linting-the-anisble-code-with-molecule-lint-(linux))
    6. [Tearing down the VM with Molecule destroy (linux)](#tearing-down-the-vm-with-molecule-destroy-(linux))
    7. [Doing it all with Molecule test (Linux)](#doing-it-all-with-molecule-test-(linux))

5. [Overview of provided cookiecutter templates](#overview-of-provided-cookiecutter-templates)

    a. [What is CookieCutter](#what-is-cookiecutter)

    b. [New Ansible Role Templates](#new-ansible-role-templates)

    c. [Existing Ansible Role Templates](#exisiting-ansible-role-templates)

    d. [Existing Anible Playbook Templates](#existing-ansible-playbook-templates)
