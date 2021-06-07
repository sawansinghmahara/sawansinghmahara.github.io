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

When I need to close it, one way I found to do this is  

1. Run `htop` ([linux task manager](https://htop.dev/)) and sort all the processes according to the time they were running for

1. Search through the processes running and look for something along the lines of `ruby /usr/gem/bin/jekyll serve -H 0.0.0.0` under the commands, around the time I think it has been running for. 

2. I then kill it either by

   * Highlighting it using the arrow keys

   * Typing `c`$$\to$$ `F9`$$\to$$`9` and then `enter`

   or by getting its process ID in the first column `PID`, say `23081` and typing

   * sudo kill -9  23081

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