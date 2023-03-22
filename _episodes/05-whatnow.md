---
title: "What now?"
teaching: 5
exercises: 90
questions:
- "How can I use ARCHER2 for my own work?"
objectives:
- "Run an exercise or Python script of your own on ARCHER2."
keypoints:
- "Perform large tasks on ARCHER2 by running jobs on the back end."
- "Any files you generate should be transferred to your own local storage and backed up."
---

Now you know enough about ARCHER2 to explore how to use it for your work or to
understand what its potential benefits are you. You may also have ideas around
where the barriers and difficulties may lie and have further questions on how
you can start using and/or trying ARCHER2 for your work.

This session is designed to give you the opportunity to explore these questions
and issues. We recommend that you try to run one of the exercises from the [first
part of the course](https://datacarpentry.org/python-ecology-lesson/) on ARCHER2,
or possibly even try to use a data analysis script of your own on the system.

The instructors and helpers on the course will be on hand to help and to answer
any questions you might have.

Outside this course, you can always seek help on using ARCHER2 by getting in touch
with the service desk at [support@archer2.ac.uk](mailto:support@archer2.ac.uk).

## Running an exercise on ARCHER2

At this point you should be able to log in to ARCHER2, install up your custom
Python packages, and run jobs.

Remember that you will need to use a job script similar to the one we looked at
in the previous lesson. Feel free to copy that script and save it into a
directory you create in your ARCHER2 work directories, along with a Python
script to do some useful work and any input data required. Make any changes you
feel are necessary to that job script before submitting it to Slurm with
`sbatch`. Keep track of it in the scheduler using `squeue` and by checking the
`slurm-<jobid>.out` file, and if necessary cancel it with `scancel`.

Once a job has completed successfully, you can transfer any output to your
machine using a tool like `scp` or `rsync`. If you are connecting with MobaXterm
from Windows, you should be able to use the client's built-in SFTP file transfer
using the pane on the left hand side of the GUI.

If you'd prefer to check text output on ARCHER2 itself, remember that you can
use the command line tools `cat` or `more` or `less`, or else open the files
directly with an editor like `vim`. If you log in to ARCHER2 with X11 forwarding
enabled (using either the `-X` or `-Y` options to `ssh`, or enabling it as an
option in MobaXterm), you can also use ImageMagick's `display` tool to view
images after you've loaded the module:

```
auser@ln01:~> module load imagemagick
auser@ln01:~> display image.jpg
```
{: .language-bash}

Bear in mind that none of ARCHER2's file systems are intended for long term
storage of data and you should consider anything kept in `/work` as being at
risk. To keep any data you generate safe, you should transfer it off-site and
keep backups.

With everything now covered, you can try using this time to try logging in to
the ARCHER2 supercomputer, setting up Python, and running a job!

{% include links.md %}
