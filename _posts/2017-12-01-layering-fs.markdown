---
layout: post
title:  "Layering Filesystems - Linux"
date:   2017-12-01
author: "@lalyos"
tags: [linux, operations, developer]
categories: others
terms: 1
---

In this lab, we will look at how layering filesystems work. Its no magic at all ... 

> **Difficulty**: Beginner (assumes no familiarity with Docker)

> **Time**: Approximately 5 minutes

> **Tasks**:
>

> * [Task 0: Prerequisites](#Task_0)
> * [Task 1: Play with alpine](#Task_1)
> * [Task 2: Learn Chroot](#Task_2)

## <a name="task0"></a>Task 0: Prerequisites

First lets create some directories, and files:
```.term1
mkdir -p  /var/lib/docker/overlay2/hack
cd $_
mkdir -p lower upper work merge
echo original text > lower/one.txt
alias ll='ls -la'
```

Lets mount an overlay fs:
```.term1
mount -v -t overlay -o lowerdir=$PWD/lower,upperdir=$PWD/upper,workdir=$PWD/work  none $PWD/merge
find .
```

Unmount
```.term1
umount $PWD/merge
````


### Make sure you have a DockerID

If you do not have a DockerID (a free login used to access Docker Cloud, Docker Store, and Docker Hub), please visit [Docker Cloud](https://cloud.docker.com) and register for one. You will need this for later steps.

## <a name="Task_1"></a>Task 1: Play with alpine

1. Run the following command in your Linux console, to pull alpine image

```.term1
docker pull alpine
```

Lets see whats in an image:
```.term1
docker save alpine | tar -xv
```

Extract the alpine FS into a dir:

```.term1
mkdir alpine
cd $_
tar -xf ../*/layer.tar
cd ..
```

Lest mount lower+alpine into merge:

```.term1
mount -v -t overlay -o lowerdir=$PWD/lower:$PWD/alpine,upperdir=$PWD/upper,workdir=$PWD/work  none $PWD/merge
ll merge
```
## <a name="Task_2"></a>Task 2: Learn chroot

In this step you'll learn how to use chroot

1. "Change Root" to the merged dir: 

```.term1
PS1='[chroot] \w \# ' SHELL=/bin/sh chroot merge 
```

2. Exit from chroot

```.term1
exit
````


---
