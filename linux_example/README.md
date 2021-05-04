## Testing a Linux image ##


When looking to test the Ansible provisioning steps that create a Linux  image, there are three ways to include the Molecule structure:


1. Create a brand new Ansible role with the Molecule structure.

2. Add the Molecule structure to an existing Ansible role.

3. The last way is to add the Molecule structure for an already existing Ansible Playbook.



### Creating a new Role (Linux) ###


Creating a new Ansible role, including the Molecule configuration needed to test the role, is very simple when using the provided cookiecutter template. The cookiecutter template will configure Molecule to use an Azure-based Linux VM as the testing platform.  For more information on the cookiecutter template itself, please see the Overview of provided templates section.



To create a new Ansible role using the cookiecutter template:



In a shell prompt, cd to the directory that the roles are stored. In the example case, this would be the **_ansible/roles_** directory.



![cd roles](../images/cdrole.png)



Next we run the following command to generate the new Ansible role:


```bash
cookiecutter "../../molecule-templates/new_linux_azure_playbook_template/"
```


You then will be asked to provide a role name, enter the name you wish to call the role:



![linux roles name](../images/linuxrolename.png)



Then you will be asked to provide a prefix for naming the Azure resource group, VM, and other components. Having a prefix set like this is useful for quickly identifying resources and services in the Azure portal. The default is to use ECD-Molecule as the prefix and can be selected by pressing enter, or you can provide a different prefix here instead:



![linux roles prefix](../images/linuxroledir.png)



Once completed, you will now see the new role created in the directory structure:


![linux roles dir](../images/linuxroleprefix.png)


### Adding Molecule structure an existing Ansible role (Linux) ###


Suppose you already have an existing Ansible role and would like to add Molecule testing. In that case, you can use the provided cookiecutter template to add the needed files and directory structure quickly. The cookiecutter template will create an Azure-based Linux VM to test the Ansible role against. 



To use the template, first in a shell, cd to the existing role in the roles directory. In our example case, that would be the ansible/roles directory.



![cd roles](../images/cdrole.png)



Then we call the cookiecutter template to add the directory and files to our existing role:


```bash
cookiecutter "../../molecule-templates/existing_linux_azure_role_template/"
```


When ran the template will ask us a few questions. The first is the name of the existing role name. Enter the name of the role you want to add the Molecule structure. The name must match the folder name of the existing role. In our example case that is existing_linux_role



![linux name](../images/linuxexistingname.png)



Next, the template will ask for a resource name prefix. The prefix will be used for naming the Azure resource group, VM, and other components. Having a prefix set like this is useful for quickly identifying resources and services in the Azure portal. The default is to use ECD-Molecule as the prefix and can be selected by pressing enter, or you can provide a different prefix here instead:



![linux prefix](../images/linuxexisitingprefix.png)



Once completed, you will now see the new Molecule directory created in the existing roles'  directory structure:



![linux role dir](../images/linuxexisitingroledir.png)



### Using an already existing Ansible Playbook (Linux) ###



If you have an existing Ansible Playbook and want to add Molecule testing, you can use the provided Cookiecutter template to add the needed files and directory structure quickly. The provided template will create an Azure-based Linux VM to run and test the Ansible roles defined in the playbook. To use the template, follow these steps:



First, in a shell, cd to the directory where the Ansible playbooks are stored. In our example case, that is the **_ansible/playbooks_** directory:



![cd playbook](../images/playbookcd.png)



Next, we will call the template to create a Molecule scenario for the existing Ansible playbook:


```bash
cookiecutter "../../molecule-templates/exisiting_linux_azure_playbook_template/"
```


When executed, the template asks us a few questions. The first is the name of the scenario, which is the name of the playbook (see the Molecule Scenario section for more information). Enter the name of the playbook (scenario) we want to add Molecule testing. In our example case, it is **_existing_playbook_**:



![linux playbook name](../images/linuxplaybookname.png)



The last question asked is for the prefix to use when naming resources, such as the Azure resource group, VM, and other components. Having a prefix set like this is helpful for quickly identifying resources and services in the Azure portal. The default is to use ECD-Molecule as the prefix and can be selected by pressing enter, or you can provide a different prefix here instead:



![linux playbook prefix](../images/linuxplaybookprefix.png)



Once completed, you will now see the new Molecule scenario directory created in the Ansible playbooks' directory structure:



![linux playbook dir](../images/linuxplaybookdir.png)


### Writing an Ansible test for a Linux VM ###



When testing Linux-based images, Python pytest-based Testinfra tests can be implemented. Tests defined in the Molecule test directory with the file prefix of test_ will automatically be picked up and ran during the Molecule verify step. 



#### Creating a Molecule test for an Ansible role (Linux) ####


To create a Molecule test that will verify Ansible roles' output results, we can use Python to write Testinfra tests. Using Testinfra/pytest allows us to follow many conventional practices when developing our Ansible tests. While not all pytest modules are available, the simple command / assert structure is standard. 


The following is an example Python Testinfra-based test. This test will verify the version of Java running on the VM.



Open the test_default.py file located in the roles molecule directory. In our example case, this is **_ansible/roles/example_linux_role/molecule/default/tests/test_default.py_**



![linux name](../images/linuxroletest.png)



In the file, add the following:

```bash

def test_java_version(host):


  """ Check if java is installed, in path, and the right version """


  command = host.run("java -version")


  assert 'java version "1.8.0_202"' in command.stderr


```

Now whenever the Molecule verify step is called on this role, the test will be executed and the results displayed in the shell output:



<< linux test run pic>>



#### Calling Ansible roles' Molecule tests from an Ansible Playbook Molecule testing (Linux) #### 


To test an Ansible playbook with Molecule, each of the Ansible roles defined in the playbook has its respective tests called. To achieve this, we need to specify the tests we want to call in the playbooks Molecule scenarios molecule.yml verifier section. As a interesting quirk we need to have a blank test_default.py file located in the scenarios test directory
For example:

![linux playbook dir](../images/linuxplaybooktest.png)

To demonstrate, let's look at an example playbook. In our example Ansible playbook folder, we have a Molecule directory with our example scenario:


![linux playbook dir](../images/linuxplaybookdir.png)



In the scenario folder, we have a **_moleculte.yml_** file. In the files verifier, we define the roles tests we wish to call as part of the playbook testing step:

```bash
verifier:
  name: testinfra
  directory: "../../../roles/install_linux_client/molecule/default/tests"
  additional_files_or_dirs:
    - "../../../../install_linux_other_client/molecule/default/tests/test_*"
```



When Molecules' verify command is executed for this playbook, the defined roles' tests will, in turn, be executed, and the output displayed in the shell window:



<< playbook test output pic>>


### Running Molecule (Linux) ###


Now let's go through the steps of running the Molecule testing suite. We will cover the individual steps and how to call them separately. We will also look at how to call all the testing suite steps for all the playbooks using one command.



#### Log in to Azure (Linux) ####



The first step before we call any Molecule commands, we must log in to our Azure account. To do this, we will use the install Azure CLI tool.



From a shell, call the following command:

```bash
az login
```

The Molecule commands use your current, active Azure subscription. To see the subscription you are currently using, we can run the az account show command:
```bash
az account show --output table
```


To switch to a different subscription, use az account set with the subscription ID or name you want to switch to.
```bash
az account set --subscription "My Subscription"
```


Your subscriptions have both a name and an ID, which is a GUID. You can use either of these for the commands. If you use a name that includes spaces, use quotation marks.


We are now signed in to Azure and have our active subscription set!
 
### Creating the VM with Molecule create (Linux) ###



To create the Azure-based VM to test the Ansible tasks on, we use the Molecule create command. The Molecule create command will provision a VM based on the settings and parameters specified in the molecule.yml file (see the Understanding the Molecule structure section for more information).


It is important to note a slight difference in how we call Molecule create for an Ansible role and an Ansible playbook.



For an Ansible playbook, we must specify the name of the scenario we are using. For example, if we wanted to call create on our example playbook, we would do the following:



First in a shell, we cd to the Ansible playbooks' directory:


![cd playbook](../images/playbookcd.png)



Then we call the Molecule create command specifying the scenario with the -s flag:

```bash
molecule create -s example
```


For an Ansible role, it is a little simpler. We do not need to specify the scenario by default. We only need to call the create command from the Ansible roles' directory. For our example role, the steps would be as follows:



First, in a shell, we cd to the Ansible role directory we want to create:


![cd roles](../images/cdrole.png)



Then we can call the Molecule create command for the role:

```bash
molecule create
```


Note the first run will take some time to complete.



Note once the infrastructure is created, it must be destroyed when not needed. See the Tearing down the VM with Molecule destroy section.



### Executing the Ansible Playbook/Role with Molecule converge (Linux) ###



To run the Ansible tasks on our newly provisioned VM, we use the Molecule converge command. The Molecule converge command will call the role/playbook specified in the converge.yml file (see the Understanding the Molecule structure section for more information).


It is important to note a slight difference in how we call Molecule converge for an Ansible role and an Ansible playbook.



For an Ansible playbook, we must specify the scenario we want to use when calling the converge command. For example, if we wanted to call converge on our example playbook, we would do the following: 



First in a shell, we cd to the Ansible playbooks' directory:


![cd playbook](../images/playbookcd.png)



Then we call the Molecule converge command specifying the scenario with the -s flag:

```bash
molecule converge -s example
```


For an Ansible role, it is a little simpler. We do not need to specify the scenario by default. We only need to call the converge command from the Ansible roles' directory. For our example role, the steps would be as follows:



First, in a shell, we cd to the Ansible role directory we want to create:


![cd roles](../images/cdrole.png)



Then we can call the Molecule converge command for the role:

```bash
molecule create
```

Once completed, the VM is now fully provisioned and can be tested against. In the next section, we will look at calling tests to verify the Ansible steps output was performed correctly.



### Verifying/Testing the provisioned VM with Molecule verify (Linux) ###



To test the outcomes of the Ansible tasks ran during the Molecule converge step, we use the Molecule verify command. The Molecule verify command will call the tests specified in the verify.yml file (see the Understanding the Molecule structure section for more information).


It is important to note a slight difference in how we call Molecule verify for an Ansible role and an Ansible playbook.



For an Ansible playbook, we must specify the scenario we want to use when calling the verify command. For example, if we wanted to call verify on our example playbook, we would do the following: 



First in a shell, we cd to the Ansible playbooks' directory:


![cd playbook](../images/playbookcd.png)



Then we call the Molecule verify command specifying the scenario with the -s flag:
```bash
molecule verify -s example
```


For an Ansible role, it is a little simpler. We do not need to specify the scenario by default. We only need to call the verify command from the Ansible roles' directory. For our example role, the steps would be as follows:



First, in a shell, we cd to the Ansible role directory we want to create:

![cd roles](../images/cdrole.png)



Then we can call the Molecule verify command for the role:

```bash
molecule verify
```


When the verify command is called, the tests defined will execute, and the results will be printed out in the shell output:



<< linux test run output pic >>


### Linting the Ansible code with Molecule lint (Linux) ###



One good step to periodically run locally is to lint your code. Linting the Ansible code will check syntax and structure to ensure the code base stays clean and follows best practice guidelines. To lint the Ansible roles and playbooks, we use the Molecule lint command. The Molecule lint command will call the linting steps defined in the molecule.yml files' linting section (see the Understanding the Molecule structure section for more information). It will use the specific tools default rules, unless otherwise defined in an exception file, for example, the yamllint and ansible-lint files, respectively.


It is important to note a slight difference in how we call Molecule lint for an Ansible role and an Ansible playbook.



For an Ansible playbook, we must specify the scenario we want to use when calling the lint command. For example, if we wanted to call lint on our example playbook, we would do the following: 



First in a shell, we cd to the Ansible playbooks' directory:


![cd playbook](../images/playbookcd.png)



Then we call the Molecule lint command specifying the scenario with the -s flag:
```bash
molecule lint -s example
```


For an Ansible role, it is a little simpler. We do not need to specify the scenario by default. We only need to call the verify command from the Ansible roles' directory. For our example role, the steps would be as follows:



First, in a shell, we cd to the Ansible role directory we want to create:

![cd roles](../images/cdrole.png)

Then we can call the Molecule lint command for the role:

```bash
molecule lint
```

When executed, the linting tools will check all needed files and print out any issues found to the shell output window:



<< linting output pic >>


### Tearing down the VM with Molecule destroy (Linux) ###



Once all your testing is done, it is essential to tear down the Azure-based testing infrastructure we have created. Tearing down the infrastructure when we are done with it will ensure no extra charges are incurred and will keep our resources tidy. To tear down the Molecule testing infrastructure, we use the Molecule destroy command. The Molecule destroy command is essentially the opposite of the create command and will remove resources based on the destroy.yml file (see the Understanding the Molecule structure section for more information). 


It is important to note a slight difference in how we call Molecule lint for an Ansible role and an Ansible playbook.



For an Ansible playbook, we must specify the scenario we want to use when calling the destroy command. For example, if we wanted to call destroy on our example playbook, we would do the following: 



First in a shell, we cd to the Ansible playbooks' directory:


![cd playbook](../images/playbookcd.png)



Then we call the Molecule destroy command specifying the scenario with the -s flag:

```bash
molecule destroy -s example
```


For an Ansible role, it is a little simpler. We do not need to specify the scenario by default. We only need to call the verify command from the Ansible roles' directory. For our example role, the steps would be as follows:



First, in a shell, we cd to the Ansible role directory we want to create:

![cd roles](../images/cdrole.png)

Then we can call the Molecule destroy command for the role:
```bash
molecule destroy
```


When executed, the destroy command will tear down any created resources and displays the output in the shell window:



<< destroy output pic >>


### Doing it all with Molecule test (Linux) ###



The steps in the proceeding sections detailed how to run the various Molecule steps individually. While this is great when testing locally, sometimes you might want to run all the Molecule steps at once, enter the Molecule test command. The Molecule test command runs all the previously mentioned steps in order. For example, it lints, creates, converges, verifies, and finally destroys all with one command.  


Once again, it is essential to note a slight difference in how we call Molecule lint for an Ansible role and an Ansible playbook.



For an Ansible playbook, we must specify the scenario we want to use when calling the test command. For example, if we wanted to call test on our example playbook, we would do the following: 



First in a shell, we cd to the Ansible playbooks' directory:


![cd playbook](../images/playbookcd.png)



Then we call the Molecule test command specifying the scenario with the -s flag:
```bash
molecule test -s example
```


For an Ansible role, it is a little simpler. We do not need to specify the scenario by default. We only need to call the test command from the Ansible roles' directory. For our example role, the steps would be as follows:



First, in a shell, we cd to the Ansible role directory we want to create:

![cd roles](../images/cdrole.png)


Then we can call the Molecule test command for the role:

```bash
molecule test
```


When executed, the test command will run through all the Molecule steps and print each of their outputs in the shell window as they complete.



<< test output pic >>