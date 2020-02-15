---
title:  "Project: simple-gpu-scheduler - easy scheduling of jobs on multiple GPUs"
date:   2019-10-25
categories: [development]
tags: [development]
---

Our research group has multiple servers each equipped with multiple GPUs.
Unfortunately, these are not connected together in a cluster infrastructure,
but instead, GPUs are assigned to individuals or on a per-project basis. This
makes the execution of many jobs using multiple GPUs difficult.

While it would be possible to connect the servers to a small cluster with
a scheduling system (we are working on it!), this can take a long time until it
is set up. Especially in academia where the maintenance and setup of servers is
often delegated to the departments IT-team, the path to implementing a small
scale cluster is littered with bureaucracy. Questions like: _Who is
responsible for xyz?_, _How are the software installations managed?_,
_Which alterations should be done to have the correct network infrastructure?_
can take ages before they are answered and appropriately implemented. In our
particular case we had the idea of refurbishing the cluster more than a year
ago and are still no where close to having it up and running.

![XKCD comic about networking problems][xkcd-networking]


## The Alternative - `simple-gpu-scheduler`

Driven by the need of having something as a bridge between our current server
setup and the to be beautiful world of our personal cluster I decided to write
a small Python package to do the job. This is how
[simple-gpu-scheduler](https://github.com/ExpectationMax/simple_gpu_scheduler)
was born.

### How it works

Software based on the CUDA library (such as most deep learning frameworks and
many others), can be constrained to only seeing certain GPUs using the
`CUDA_VISIBLE_DEVICES` environment variable. The `simple-gpu-scheduler` accepts
commands and executes them while setting the environment variable to
a currently free GPU. As soon as the job finishes, the GPU is released and the
next job is allocated to it. This allows to always utilize all of the GPUs to
the maximally possible extent [^gnu-parallel].


### Usage

I wanted to make `simple-gpu-scheduler` as simple and flexible as possible and
thus tried to adhere to the [KISS
principle](https://en.wikipedia.org/wiki/KISS_principle). Like many UNIX tools
it thus takes it's input from `stdin` such that it can be combined with other
tools. This allows reading commands from a list, or even from a fifo (first in
first out), such we can build a fully functioning queuing system. For further
reference please consult the [GitHub
page](https://github.com/ExpectationMax/simple_gpu_scheduler) of the project.


### Simple example

Suppose you have a file `gpu_commands.txt` with commands that you would like to
execute on the GPUs 0, 1 and 2 in parallel:

```bash
$ cat gpu_commands.txt
python train_model.py --lr 0.001 --output run_1
python train_model.py --lr 0.0005 --output run_2
python train_model.py --lr 0.0001 --output run_3
```

Then you can do so by simply piping the command into the `simple_gpu_scheduler`
script
```bash
$ simple_gpu_scheduler --gpus 0 1 2 < gpu_commands.txt
Processing command `python train_model.py --lr 0.001 --output run_1` on gpu 2
Processing command `python train_model.py --lr 0.0005 --output run_2` on gpu 1
Processing command `python train_model.py --lr 0.0001 --output run_3` on gpu 0
```

### Hyperparameter search

One of the most common use cases for running many jobs in parallel is
hyperparameter search. For convenience I added a small script
`simple_hypersearch` which generates commands to evaluate a hyperparameter
grid. Here is a small example of how to generate all possible configurations
and execute them in random order:

```bash
simple_hypersearch "python3 train_dnn.py --lr {lr} --batch_size {bs}" -p lr 0.001 0.0005 0.0001 -p bs 32 64 128 | simple_gpu_scheduler --gpus 0,1,2
```


### Final words

I hope some of you find the software useful. Feel free to open issues and
feature requests if you need any further features. See you next time!


[xkcd-networking]: https://imgs.xkcd.com/comics/networking_problems.png "LOOK, THE LATENCY FALLS EVERY TIME YOU CLAP YOUR HANDS AND SAY YOU BELIEVE"

[^gnu-parallel]: [GNU parallel](https://www.gnu.org/software/parallel/)
    can be used to do something similar (see the
    [HN discussion](https://news.ycombinator.com/item?id=21269950)). It is
    significantly more flexible, which IMHO comes at the cost of ease of use.
