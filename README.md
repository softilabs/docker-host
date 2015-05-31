# Docker Host
A Docker extension to manage multiple containers and more

#### The main purpose is for defining and running multi-container applications grouped by environment, this combination is then called configuration.

* Images don't age out on Docker Hub
* No other syntax to learn than Docker
* Naming conventions are intuitive and easy to apply
* Chaining inheritance for images and global container options
* Variables in container options are interpreted
* Export/Import data to remote host relative to home directory
* Switch between configurations like a ninja
* No need to install Docker Host on remote host

### Requirements

[Docker](https://docker.com/) |
[Git](http://git-scm.com/) |
[jq](http://stedolan.github.io/jq/) |
[Rsync](https://rsync.samba.org/) |
[SSH](http://www.openssh.com/) |
[Wget](https://www.gnu.org/software/wget/)

### Installation

```bash
cd /usr/local/bin
sudo wget https://raw.githubusercontent.com/softilabs/docker-host/master/docker-host
sudo chmod +x docker-host
```

### Usages

```bash
docker-host --help
docker-host [COMMAND] --help
```

### Naming conventions

| Name | Convention |
|:---:| --- |
| Configuration dir | ```[BASE_DIR]/Configs/[CONFIG]``` |
| Container dir | ```[BASE_DIR]/Configs/[CONFIG]/[ENV]/[CONTAINER(/)]``` |
| Generated image name | ```[CONFIG]/[ENV]_[CONTAINER(_)]``` |
| Generated container name | ```[CONTAINER(_)]``` |
| Export dir | ```[BASE_DIR]/Configs/[CONFIG]/.export``` |
| Import dir | ```[BASE_DIR]/Configs/[CONFIG]/.import``` |
| Data dir | ```[BASE_DIR]/Data/[CONFIG]``` |
| Data environment dir | ```[BASE_DIR]/Data/[CONFIG]/[ENV]``` |
| Data container dir | ```[BASE_DIR]/Data/[CONFIG]/[ENV]/[CONTAINER(/)]``` |
| Projects dir | ```[BASE_DIR]/Projects``` |

### Specific files

| Filename | Description | Location |
|:---:| --- | --- |
| ```.run``` | Sorted containers list to run | Root of an environment directory |
| ```.options``` | Options list for container | Root of a container directory |
| ```.globaloptions``` | Options list for container and child containers | Root of a container directory |

### Example

**Installation**
```bash
cd
git clone https://github.com/softilabs/docker-host-example vhome
```

**Start configuration**
```bash
docker-host start vhome/Configs/hello/dev
```
Open [http://localhost/](http://localhost/)

**Switch another configuration**
```bash
docker-host start vhome/Configs/hello/prod
```
Refresh [http://localhost/](http://localhost/)

**List all running containers with their interfaces**
```bash
docker-host status
```

**Open a terminal of a running container**
```bash
docker-host login blog
```

**Stop running configuration**
```bash
docker-host stop
```

**Export data on remote host(s)**
```bash
docker-host export vhome/Configs/hello
```
*```[BASE_DIR]/Configs/[CONFIG]/.export/user@example.com``` filename needs update.*

**Start configuration on remote host**
```bash
docker-host start vhome/Configs/hello/prod user@example.com
```

**Open a terminal of a running container on remote host**
```bash
docker-host login blog user@example.com
```

### Best practices

* The use of ```phusion/baseimage``` is highly recommended to build docker images. The various reasons and instructions are explained on this website : [http://phusion.github.io/baseimage-docker/](http://phusion.github.io/baseimage-docker/)
* It is advisable to place ```Configs```, ```Data``` and ```Projects``` folders to the root of your home directory on local and remote hosts.
* Versioning the ```Configs``` folder in a private git repo can be useful for teamwork.
