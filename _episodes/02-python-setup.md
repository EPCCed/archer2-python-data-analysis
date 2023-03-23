---
title: "Preparing Python for data analysis on ARCHER2"
teaching: 30
exercises: 0
questions:
- "How do I access Python on ARCHER2?"
- "What extra setup do I need to do to use Python for data analysis?"
objectives:
- "Understand how to enable and customise Python on ARCHER2."
- "Add the extra packages we need for data analysis."
keypoints:
- "The `cray-python` module provides an enhanced Python installation on ARCHER2."
- "Many numerical packages are already provided in this installation."
- "Further packages can be added to it using `pip`."
---

## Accessing and using the Cray Python installation

When you log in to ARCHER2 in a new SSH session, Python will be available
immediately on the command line. However, this is really just a fairly
bare-bones installation that the operating system provides, and we would not
recommend making use of it.

HPE, the ARCHER2 vendor, provide a separate installation of 'Cray Python' which
includes Python 3 and several useful numerical and scientific packages. If
you've used them before, you might think this is like having access to an
Anaconda Python installation, which also comes with lots of useful packages. The
Cray Python installation is actually a bit different: it includes more packages
than Miniconda, but not as many as Anaconda. The most important point to note,
though, is that the software bundled into Cray Python has been optimised for the
ARCHER2 software and hardware, meaning it is able to use the high performance
numerical and communication libraries on the system.

Cray Python is accessible via the modules system. After logging in, run the
following commmand:

```
auser@ln01:~> module load cray-python
```
{: .language-bash}

If you choose to start the Python interpreter, you will see that it is indeed a
Cray build of Python 3 which runs:

```
auser@ln01:~> python
```
{: .language-bash}

```
Python 3.8.5 (default, Aug 24 2020, 19:11:09)
[GCC 9.3.0 20200312 (Cray Inc.)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
{: .output}

Once your cursor is flashing at the `>>>` prompt, remember you can type `Ctrl-D`
or `quit()` to exit. The version of Python shown above is correct the time of
writing, but with software upgrades to ARCHER2 this may change.

> ## Python 2 or 3?
> Python 2 is no longer supported on ARCHER2. Once the `cray-python` module has
> been loaded, both the `python` and `python3` commands will start Python 3.
{: .callout}

At this point you can go ahead and use Python. If you were to use NumPy or mpi4py,
you would be using the versions of those packages which have been optimised for
ARCHER2. However, we need to take things a bit further, because we specifically
want to use pandas, matplotlib and plotnine, which are not installed--so we have
to do this ourselves.

## What is pip?

Modern Python installations are designed to be flexible, allowing you to add
extra pacakges as you require them. For that reason, several tools are available
which make managing packages very simple, automating their download and
installation from the Internet, installing any necessary dependencies to use
them, upgrading to new versions, and, if necessary, their uninstallation. If you
use a Linux distribution with a package manager like `apt` or `pacman`, or if
you use `brew` on macOS, this will probably look quite familiar to you. Almost
all modern Python distributions come with the 'pip' package manager.

Pip is run on the command line with `pip`. Let's run the `pip list` command on
ARCHER2 once the `cray-python` module has been loaded in order to list the
packages that are currently installed.

```
auser@ln01:~> pip list
```
{: .language-bash}

```
Package            Version
------------------ -------
atomicwrites       1.3.0
attrs              19.3.0
Cython             0.29.15
dask               2.13.0
fsspec             0.7.4
importlib-metadata 1.5.0
locket             0.2.0
more-itertools     8.2.0
mpi4py             3.0.3
nose               1.3.7
numpy              1.18.2
packaging          20.1
pandas             1.0.5
partd              1.1.0
pip                20.1.1
pluggy             0.13.1
py                 1.8.1
pyparsing          2.4.6
pytest             5.3.5
python-dateutil    2.8.1
pytz               2020.1
scipy              1.4.1
setuptools         47.1.0
setuptools-scm     3.4.3
six                1.14.0
toolz              0.10.0
wcwidth            0.1.8
zipp               2.2.0
```
{: .output}

There are a few other commands worth remembering. Some of these need an extra
bit of flavour to use on ARCHER2, so we'll look at them in the next section.
However, because these are worth bearing in mind, they are:

- `pip install <package>` which is used to install `<package>`, for example `pip
  install pandas`. This will also install any other packages which are needed in
  order for `<package>` to run correctly.
- `pip uninstall <package>` which will, as you've probably guessed, uninstall
  `<package>`, for example `pip uninstall pandas`.
- `pip install --upgrade <package>` which will update `package` to any available
  newer versions. You should generally run `pip update` first and then `pip
  upgrade`.
- `pip show <package>` to get some brief information on `<package>`, including
  its dependencies, for example `pip info pandas`.
- `pip help` for a list of commands available. You can get more information on a
  particular command by adding the command after `help`, for example `pip help
  install`.

The packages installed by pip are downloaded from the Python Package Index
(PyPI). You can go to the website [here](https://pypi.org/) to search for more
information on packages you might be interested in.

## Installing extra packages in user-space

If using pip on your own machine, you can do things like update pip itself. When
using it on ARCHER2, you might even see it produce a message letting you know it
can be updated. However, any updates to ARCHER2 or the packages provided in the
base installation, like `numpy` and `mpi4py`, cannot be upgraded in-place as
these are what we call 'central installations', installations which are
available for use by everyone on the system. As such, only users with elevated
privileges can modify Cray Python and the packages it includes, and we would
only expect them to be updated as part of a new software release from HPE. This
makes sense on a multi-user system like ARCHER2; imagine if you were working
with it, and then another user upgraded a package and caused your script to
break!

> ## Using a newer version of Python
> Software releases from HPE come in the form of a new version of the CPE (Cray
> Programming Environment). These are actually available on the system now, and
> you can see them if you run
>
> ```
> module avail cpe
> ```
> {: .language-bash}
>
> Among the output you should see that `cpe/21.04`, `cpe/21.09` and `cpe/22.04`
> are available. The first of these is the default CPE version loaded at login,
> so when you run `module load cray-python` you are getting the version of Cray
> Python that was packaged in this release. You can load newer versions of
> Python by loading the module for a newer CPE release. For example, after
> running `module load cpe/22.04`, Python is version 3.9.7.
{: .callout}

At this point it might look like the Python installation is, to use a Pythonic
term, immutable, but this isn't entirely the case. The files making up the
central installation can't be changed, but we can *add* to the installation with
up extra packages which will go into our own user directories where we do have
sufficient permissions. This is called a user-space installation.

This just needs a small change to the `pip install` command. To install a package
we would now do:

```
auser@ln01:~> pip install --user <package>
```
{: .language-bash}

The new `--user` option tells pip to download and install `<package>` not into
the directories where Python is installed but into our own directories. By
default, this means into a directory called `.local` inside our home directory,
*i.e.* `$HOME/.local`. Remember that our ARCHER2 project on this course is
called `{{site.gid}}`, so if your username is `auser` you would find that that
this pip user-space installation would place the new files and directories in
`/home/{{site.gid}}/{{site.gid}}/auser/.local`.

This is all good, but there's a final and critical consideration when using
ARCHER2 in this way. As we saw in the previous episode, the home file system,
where `/home` resides, is not mounted on the compute nodes. On the other hand,
the high-performance work file system is. That means that if we want to use any
custom-installed Python packages when running on the compute nodes, we will need
to change the installation location to somewhere on `/work` instead.

An obvious thing to do is simply to install our custom packages into our own
work directories. We can do that with the three commands below, setting the
environment variables that pip uses to see where it should install to and also
making sure that Python and the shell will be able to find those packages once
installed. When doing this, you should of course change `auser` to your own
ARCHER2 username.

```
auser@ln01:~> export PYTHONUSERBASE=/work/{{site.gid}}/{{site.gid}}/auser/.local
auser@ln01:~> export PATH=$PYTHONUSERBASE/bin:$PATH
auser@ln01:~> export PYTHONPATH=$PYTHONUSERBASE/lib/python3.8/site-packages:$PYTHONPATH
```
{: .language-bash}

At this point you can go ahead and use `pip install --user <package>` to install
to the non-default location. In the next section we'll go through the commands
we need to perform the necessary setup.

> ## Setting the environment up again
> If you log out and in again after doing this set up, you'll find that you need
> to run these three `export` commands again. Asking pip to install to the
> non-default location doesn't automatically carry over between shells. To make
> things a bit easier, you can open up the `.bashrc` file in your home directory
> and add the commands. This script runs in new shells when they start up,
> including when you log in. If you're ever unsure of the value of an
> environment variable or whether it's been set or not, you can simply print it
> to screen with `echo`, for example, `echo $PYTHONPATH`.
{: .callout}

> ## Python virtual environments
> What we've done here is about the simplest and fastest way to get the extra
> packages we need installed. If you want to, though, you may be interested in
> instead using virtual environments to manage different sets of installed
> packages depending on your requirements. You can read about how to do this
> [in the ARCHER2 documentation](https://docs.archer2.ac.uk/user-guide/python/#setting-up-virtual-environments).
{: .callout}

## Adding the extra packages for this course

Finally, let's put all of above together and actually install the packages we
need to run the exercises from earlier on ARCHER2.

Starting from logging in, so we have a clean environment, we'll firstly load
Cray Python into the environment.

```
auser@ln01:~> module load cray-python
```
{: .language-bash}

Then we'll set the environment variables to install into the work file system,
just as above.

```
auser@ln01:~> export PYTHONUSERBASE=/work/{{site.gid}}/{{site.gid}}/auser/.local
auser@ln01:~> export PATH=$PYTHONUSERBASE/bin:$PATH
auser@ln01:~> export PYTHONPATH=$PYTHONUSERBASE/lib/python3.8/site-packages:$PYTHONPATH
```
{: .language-bash}

Finally, we'll install the necessary packages for the exercises. You can check
what's already been installed with `pip list`. You'll see that pandas is already
there, but we might need both matplotlib and plotnine, so we'll install those.

```
auser@ln01:~> pip install --user matplotlib plotnine
```
{: .language-bash}

You'll see pip doing some work as it downloads the packages and probably a good
few dependencies from the Internet to install them in your work directory. Once
done, pip should inform you that it has successfully installed these two
packages and any dependencies.

At this point we're almost ready to run our exercises on ARCHER2. In the next episode,
we'll look at how to submit a job to the ARCHER2 backend to do so.

{% include links.md %}
