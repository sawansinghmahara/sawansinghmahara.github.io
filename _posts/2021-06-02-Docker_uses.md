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
