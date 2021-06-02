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

<div class="about">
<div class="about__devider">*******</div>
<br>  <br>
</div>
# Removing Selected Docker Containers

Type the following

```bash
{% raw %} docker ps -a --format {{.Names}} > names.txt {% endraw %}
```

to get all the names of the docker images, stored in the text file `names.txt`.

* Add, delete or keep all the entries in the text file the same.
* The following code deletes all the containers whose names are present in `name.txt`, and then deletes it.

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
