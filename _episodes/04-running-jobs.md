---
title: "Running jobs on the ARCHER2 data analysis nodes"
teaching: 30
exercises: 0
questions:
- "What is a scheduler?"
- "What is a job?"
- "How do I run jobs?"
objectives:
- "Write a correct job script."
- "Submit a job to the queue and retrieve the results."
keypoints:
- "Access to resources on a cluster is often governed by a scheduler like Slurm."
- "Batch systems try to provide fair access to resources to users on the system."
- "A job is run automatically once it is run on available resources."
---

## The Slurm batch system

We've spoken about the layout of ARCHER2. Its structure is broadly typical for a
cluster: there is a frontend, the login nodes, which we connect to via SSH and
where we prepare data and work, and then the backend, made up of the compute
nodes which will actually do the work. So far though we've only interacted with
the login nodes.

Access to the backend is governed by a 'scheduler'. Again, this is typical for a
cluster. A scheduler is a piece of software running in the background. Users
submit jobs to it, which are requests for resources to be used (typically a
number of CPU cores or nodes and perhaps an amount of memory) over a certain
period of time, and the scheduler determines when to reserve those resources for
that job. This may sound simple, but there can be many hundreds or thousands of
jobs either running or waiting to run at any given time, and the scheduler must
determine when to run what as fairly as possible for everyone.

Slurm is the name of the scheduler used on ARCHER2. If you've used a cluster
before, you might be familiar with it or with alternatives like PBS or SGE. They
are all used in a similar manner, but the commands used to interact with each of
these schedulers differ.

Try running the `squeue` command now. This will produce a list of all the jobs
in the system. It will probably be quite long.

```
auser@ln01:~> squeue
```
{: .language-bash}

If you want to see what jobs `auser` has in the system, you can use

```
auser@ln01:~> squeue -u auser
```
{: .language-bash}

You can use this to look at your own jobs, but it can be quicker and easier to
run

```
auser@ln01:~> squeue --me
```
{: .language-bash}

Of course, we haven't actually run any jobs yet, so you'll see an empty list if
you check yourself with `squeue`.

For any jobs listes, you'll see its ID, its name, the length of time it's been
running and its status. Most jobs will have the status `R`, meaning 'running',
or `PD`, meaning 'pending'. Running jobs will also list the names of the nodes
they're using, while pending jobs will list the reason they haven't run yet.

Slurm tries to be fair when determining when the pending jobs should be started.
If you're interested it uses what is called a backfill algorithm, but the gist
of it is that it schedules the largest pending jobs first, taking into account
how many resources they each need and for how long, and then it starts fitting
smaller jobs into the gaps between the big jobs.

You might still be wondering 'what is a job, though?' At the highest level, a
job is a set of resources, CPU cores and memory, on the backend of the cluster
that has been reserved for **you and your work**. In most cases, users are
charged for their jobs from a budget they have access to. Resources on ARCHER2
are in units of 'CU' where 1 CU is equal to one node hour. One node is the use
of one compute node for one hour, so using two nodes for four hours would cost 8
CU, for example. If a user's budget runs out, they can no longer run jobs from
it. Most jobs do cost CUs to run, though, as we'll see below, we'll be using the
special data analysis or serial nodes which are free to use.

## Job scripts

Resources by themselves don't do anything. What we call an interactive
job allows you, the user, to directly work on the backend in a shell. On
ARCHER2, you might for example submit a job asking for four nodes. When the job
starts, you would move onto the first node in the job and work directly on it.

Most of the time, though, people will submit job scripts to the scheduler
instead. These are simply shell scripts, with some extra syntax at the beginning
saying what resources are needed, that go in order through the steps needed to
complete the work.

The job script that we will use in this course looks like the following:

```
#!/bin/bash

# Slurm job options. Set name and max time.
#SBATCH --job-name=python_job
#SBATCH --time=0:20:0
#SBATCH --ntasks=1

# Run with budget {{site.gid}} on the serial nodes.
#SBATCH --partition=serial
#SBATCH --qos=serial
#SBATCH --account={{site.gid}}

# Define memory required for this jobs. By default, you would 
# get just under 2 GB, but you can ask for up to 125 GB.
#SBATCH --mem=2G

# Set the number of threads to 1
#   This prevents any threaded system libraries from automatically 
#   using threading.
export OMP_NUM_THREADS=1

# Make the Cray Python installation available.
module load cray-python

# Make available user-installed Python packages installed in work.
export PYTHONUSERBASE=${HOME/home/work}/.local
export PATH=$PYTHONUSERBASE/bin:$PATH
export PYTHONPATH=$PYTHONUSERBASE/lib/python3.8/site-packages:$PYTHONPATH

# Run my_analysis_script.py through Python.
python my_analysis_script.py
```
{: .language-bash}

There's quite a lot to take in here, so we'll go through it piece-by-piece.

The first line is a shebang. Just as Python scripts often start with
`#!/usr/bin/python`, `#!/usr/bin/env python3` or another similar command that
means the script is to be run through the Python interpreter, the shebang in
this case says that the script is to be run through bash.

Several more lines beginning with `#` follow. Some are comments, just there to
help us understand what's happening. Very important though are the lines
beginning with `#SBATCH`. These are Slurm directives which provide information
about the resources we would like for this job.

- `#SBATCH --job-name=python_job` sets the name of the job that we'll see if we
  run `squeue`. You can change this to whatever you think will help you identify
  your job.
- `#SBATCH --time=0:20:0` says that the job can run for, at most, 20 minutes. If
  the script finishes before then, the job will end cleanly. If it's still going
  at 20 minutes, Slurm will shut it down and the resources will be released for
  other jobs.
- `#SBATCH --ntasks=1` says that we want to run a single task on one processor
  core. For us just now, this doesn't matter too much, but ARCHER2 is capable of
  running massively parallel jobs with hundreds of thousands of tasks working in
  tandem.
- `#SBATCH --partition=serial` and `#SBATCH --qos=serial` are two commands we
  use together to specify that we want this job to run on one of the data
  analysis nodes. These are a bit different from the standard ARCHER2 compute
  nodes. They have twice as much memory and are intended to allow users to run
  many different serial (meaning not parallel) jobs on them at the same time.
- `#SBATCH --account={{site.gid}}` tells Slurm which budget to charge the
  resources used by the job to. You can think of the word 'account' here meaning
  something more like a bank account rather than a user account. For us, the
  value is the project name. As we'll be running on the data analysis nodes, the
  `{{site.gid}}` budget won't actually be charged.
- `#SBATCH --mem=2G` tells Slurm that of the 512 GB memory on an individual data
  analysis node, we would like to reserve 2 GB for our job. You can request up
  to 125 GB if necessary.

Because these lines begin with the bash comment character `#`, bash will ignore
them. But, as we'll see, we run the script through a special command which will
pick these lines up and use them when entering the job into the scheduler.

So far, everything we've seen has been about setting up the job and requesting
the resources we would like to use for it. But what about the actual work? We
want a Python script to run and do something useful for us. So, after all the
`#SBATCH` lines are some uncommented lines of commands to actually be run via
bash on the data analysis node when the job begins. Let's go over those now.

Firstly, there's an `export` command setting the variable `OMP_NUM_THREADS` to
have the value of `1`. This isn't too important for us here, but its purpose is
to make sure that anything in the background that might use OpenMP threading for
parallelisation restricts itself to one thread only. This makes sense, as we've
only got one CPU core to use from the `ntasks` option we set.

Next, we load a module and set some environment variables. These should be
familiar to you from the previous lesson: we're loading Cray Python and making
sure that it will be able to see and use the Python packages we installed into
our work directory. These commands make an important point: the environment
within the job is new and separate from the environment we're using in our
interactive shell session. If there are any steps you need to go through to
enable your code to start and run correctly, you should put them into your job
script. If we hadn't run `module load cray-python`, the system Python
installation would be used instead; if we hadn't set these three environment
variables, Python would have no idea where to find matplotlib or plotnine.

At this point there is only one thing left to do, and that's to run our script,
so we start Python and tell it to run `my_analysis_script.py`.

## Running and controlling a job

The next step is to actually run a job. Let's take our imaginary job script
above and save it in a file somewhere in our work directories. Its name doesn't
really matter: the most important thing is that it makes sense to you.
