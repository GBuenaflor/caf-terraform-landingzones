# Getting stated with Azure Cloud Adoption Framework landing zones for Terraform

## Prerequisites

In order to start deploying your with CAF landing zones, you need an Azure subscription (Trial, MSDN, etc.) and you need to install the following components on your machine:

- [Visual Studio Code](https://code.visualstudio.com/)
- [Docker Desktop](https://docs.docker.com/docker-for-windows/install/)
- [Git](https://git-scm.com/downloads)

You can deploy it easily on Windows and MacOS with the following software managers:

| MacOS  | Windows |  
| ------ | ------- |
|```brew cask install visual-studio-code docker``` </br> ```brew install git ``` | Install Chocolatey (https://chocolatey.org/docs/installation) </br> ``` choco install git vscode docker-desktop ``` |

Once installed, open **Visual Studio Code** and install "**Remote Development**" extension as follow: ![RemoteDevelopment](../../_pictures/caf_setup_remotedev.png)

## Cloning the repository

Cloning your first repository:

```bash
git clone https://github.com/Azure/caf-terraform-landingzones.git
```

## Open the repository in Visual Studio Code

Open the repository you've just cloned in Visual Studio Code, click on the lower bar, green sign and in the palette opening on the top of Visual Studio Code Window, select **"Open Folder in container"** or **"Reopen in container"**

![RemoteDevelopment](../../_pictures/caf_remote_dev.png)

This should take a while, in the meantime, feel free to click on Details to see the container being downloaded from the registry and being connected to yur local environment:

![SetupContainer](../../_pictures/caf_setup_container.png)

You will have to accept local mapping to your filesystem when Docker prompts you (here's [how you reconfigure Docker Desktop to allow fileshares](../../_pictures/caf_setup_docker_fileshares.png) ), so that you can access your files from your container.

![Ready](../../_pictures/caf_dev_ready.png)

After a while, your environment is ready, note on the lower left part of Visual Studio Code, that you are now in your Azure CAF rover, which is your environment to use Azure landing zones.

## Deploying your first landing zone

You must be authenticated first:
For that we will rely on Azure authentication as completed by Azure Cli, via browser method:

```bash
rover login
```

We recommend that you verify the output of the login and make sure the subscription selected by default is the one you want to work on. If not, you can use the following switch:

```bash
az account set --subscription <subscription_GUID>
```

On the first run, you need to use the launchpad to create the foundations for Terraform environment:

```bash
rover /tf/caf/landingzones/launchpad apply -launchpad
```

This command will interactively prompt you for *var.location*, asking for the name of a supported Azure region **where you want to deploy the Terraform state and dependencies**. You can specify that in the argument as in the following example:  

```bash
rover /tf/caf/landingzones/launchpad apply -launchpad -var 'location=westus'
```

You can then launch your first landing zone!

Please note that each landing zone come with its own deployment settings, which may deploy resources in different region than where you set the foundations.  

You are ready to start:

```bash
rover /tf/caf/landingzones/landingzone_caf_foundations plan
```

```bash
rover /tf/caf/landingzones/landingzone_caf_foundations apply
```

```bash
rover /tf/caf/landingzones/landingzone_caf_foundations destroy
```

## Updating your development environment

If you are using previous version of Azure landing zones (v1.0.1912), since we migrated to use new version of the rover, which uses non-root containers, you will have to re-create your volumes.
You can achieve that running the following commands:

```bash
# To list all Dev Container volumes
docker volume ls -f label=caf

# To cleanup de Dev Container volumes make sure there is not running or stopped containers
docker ps
docker ps -a

# To cleanup a specific Dev Container
docker volume rm -f $(docker volume ls -f label=com.docker.compose.project=landingzones_devcontainer)

# To cleanup all Dev Containers
docker volume rm -f $(docker volume ls -f label=caf)
```

You can also purge Docker cache running the following command:

```bash
docker system prune -a
```

Happy deployment with Azure landing zones, let us know your feedback and how you need it to evolve!