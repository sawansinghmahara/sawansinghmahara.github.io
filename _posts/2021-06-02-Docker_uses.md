---
layout: page
title: "Anecdotal Use Cases"
categories: Docker
date:   2021-06-02 10:30:32 +0530
tags:
  - docker
  - computer software
subtitle: Ways I have used docker and some useful commands or procedures
permalink: "/_posts/docker_uses"

---

* Contents
{:toc}
# Making this website!

Working with jekyll is sort of fun and interesting, but getting ruby and gems installed on my computer isn't. Docker came to the rescue and long story short, by running

```python
docker run --rm -it --volume="$PWD:/srv/jekyll" --volume="$PWD/vendor/bundle:/usr/local/bundle" --env JEKYLL_ENV=production jekyll/jekyll:3.8 jekyll build
```

to pull the jekyll docker image if it isn't installed, I was able to build the base website locally.

This was then followed by a simple

```bash
docker run --rm --volume="$PWD:/srv/jekyll" --volume="$PWD/vendor/bundle:/usr/local/bundle" --env JEKYLL_ENV=development -p 4000:4000 jekyll/jekyll:3.8 jekyll serve
```

to serve up the site locally! Now I can make changes and see the effects immediately, rather than wait for Github to load changes.

As a bonus, I don't have to be bothered about umpteen dependencies being installed or be surprised when it works on a Tuesday, but not on a Wednesday!

## Issue

One gripe I have with this is that the usual `ctrl+c` doesn't shut off the server.

When I need to close it, I have to kill the docker container itself, which can be done by running the following commands

* Get the docker container name

  ```bash
  docker ps -a
  ```

* Kill the Jekyll server with

  ``` bash
  docker kill <ContainerName>
  ```

  

<div class="about">
<div class="about__devider">*******</div>
<br>  <br>
</div>
# Removing Selected Docker Containers

If there are many containers cluttering my system, manually typing one command after another to remove each would be tedious. Instead, by typing the following

```bash
{% raw %} docker ps -a --format {{.Names}} > names.txt {% endraw %}
```

I get all the names of the docker images to be stored in a text file `names.txt`.

I then modify the text file to keep or delete some containers, following which the python code below deletes all the containers whose names are present in `name.txt`.

```python
import os
with open('names.txt') as fp:
    for line in fp:
        os.system(f"docker rm {line}")
os.system("rm names.txt")
```

<div class="about">
<div class="about__devider">*******</div>
<br>  <br>
</div>
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
# Running Latex 

Installing latex and their dependencies can sometimes be too much work. Instead, a docker image can contain the latex installation.

First pull the following docker image

```bash
docker pull blang/latex
```

Assuming you have VSCode installed

* Type `ctrl+shift+P` and then search for `Preferences: Open Settings (JSON)`

* Open it and then paste the following in at the end, before the `}`

  ```json
  //Other Settings are here
  //latex
      "latex-workshop.docker.enabled": true,
      "latex-workshop.latex.outDir": "./out",
      "latex-workshop.synctex.afterBuild.enabled": true,
      "latex-workshop.view.pdf.viewer": "tab",
      "latex-workshop.docker.image.latex": "blang/latex",
  //latex
  }
  ```

Now click the green play button on the top to run the `.tex` file. 
You can test it out with this as well

```latex
\documentclass{article}
\begin{document}
A simple and \tiny{tiny} \normalsize \LaTeX \ document.
\end{document}
```



<div class="about">
<div class="about__devider">*******</div>
<br>  <br>
</div>
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
# VSCode and Docker

If you want to develop codes inside any docker container, one way you can do it is with VSCode.

Follow the steps below.

* Install this extension

  ```text
  Id: ms-azuretools.vscode-docker
  Description: Makes it easy to create, manage, and debug containerized applications.
  Version: 1.14.0
  Publisher: Microsoft
  VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker
  ```

* Click the extension on the bar on the left. You can see installed images and containers. Right click on any container you would want to run  --> `Attach Visual Studio Code`

* The container terminal can be exited by clicking the button on the bottom left, `Container <Container name>..` -->`Close Remote Connection`

More information can be found [here.](https://code.visualstudio.com/docs/remote/containers)

This is especially useful if you don't want to be bothered with managing environments, or if you want to build a *ready to go* application, to run on someone else's system with ease.

## Building a non-root user docker image

Normally, when you run a container this way, it would be done as a root user, inside some folder named `/workspace`. This can be a bad thing if you are also mounting a directory from your system in the container as any files created in it would be done as root, which can lead to issues.

VSCode allows for you to create a `devcontainer.json` file to specify setting like what volume to mount, what user mode to run it in and so on and so forth. This can be done, probably. But I have not been able to get it to work, thus I came up with this docker file instead, which helps with this scenario.

  
```dockerfile
FROM ubuntu:latest
#The next command sets root password s 1234 inside the container
RUN echo "root:123456" | chpasswd
# This adds a user named 'nonrootuser' and creates password as 1234 and adds them to sudoers list
RUN useradd -ms /bin/bash nonrootuser
RUN echo "nonrootuser:123456" | chpasswd
#Updates the container.
RUN apt-get update && apt-get install --no-install-recommends --yes
# Gets sudo and adds nonrootuser to the list of sudoers
RUN apt-get install sudo -y
RUN usermod -aG sudo nonrootuser
#Important things to get. You can delete this if you do not need it.
RUN apt-get install wget -y
RUN apt install git -y
USER nonrootuser
RUN mkdir /home/nonrootuser/codes
WORKDIR /home/nonrootuser/codes
```

#### What is this?

It is a docker file to create an image on your system, with certain settings that will be beneficial to develop codes present in a mounted drive, with VSCode inside it.

#### What does it do?

It does the following things

* From the ubuntu base image, it creates a user named `nonrootuser` that has `sudo` access.
* It also installs neccessary packages like `sudo` and `git` and `wget`.
* The user it creates, `nonrootuser`, has a password `123456` (line 6 in the script). The `root` user is also given this password (line 3 in the script)
* Whenever the image is run in interactive mode, it defaults to the user `nonrootuser` and runs in the `/home/nonrootuser/codes` folder.

#### What to do with this?

An ubuntu image can be created on your system with this docker file with the command 

``` bash
docker build . -t <custom_image_name>
```



#### What does this have to do with VSCode?

After the image is created, any container created with a simple `docker run -it -v $PWD:/home/nonrootuser/codes/ <custom_image_name>` will start in the directory `/home/nonrootuser/codes` and with the user `nonrootuser` and mount the current directory of where the command was executed, onto the directory.

There are ways to map your current user onto the container as well, in the run command, but the commands can be tricky and it causes issues with VSCode. This is one way I managed to make it work.

Now, the container created can be accessed in VSCode and run as non-root, and your project will automatically be mounted on `/home/nonrootuser/codes/`

#### GPU Use

If you are developing inside a container and have an Nvidia GPU, you need to first setup the `nvidia-container-toolkit`. You can follow the instructions on [their GitHub page](https://github.com/NVIDIA/nvidia-docker), but basically you have to run the following commands, if you are on a Debian linux system. 

``` bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

This way, whenever you want your GPU to be seen inside any other container, it can now happen with the `--gpus all` flag sent along with the `docker run` command.

For example, if you want to enable gpu access on an ubuntu container created above, execute 

```bash
docker run -it -v --gpus all $PWD:/home/nonrootuser/codes/
```

Now you can develop with VSCode, inside docker containers!
