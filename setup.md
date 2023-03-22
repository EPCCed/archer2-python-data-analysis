---
title: Setup
---

Please try to complete the following setup tasks ahead of the lesson. Doing so
will make sure you can immediately connect to ARCHER2 and get working on the
day of the course. We go over:

* Installing an SSH client, if you don't already have one.
* Creating an SSH keypair to authenticate a connection to ARCHER2.
* Getting a SAFE account.
* Getting an ARCHER2 account.
* Connecting to your new ARCHER2 account.

## Install an SSH client

The Secure Shell protocol allows users to connect to the ARCHER2 supercomputer.
If you've ever logged into a remote workstation or cluster, you are probably
used to using SSH.

If not, you will need to make sure you are ready for the course. If you are
using a Linux distribution or macOS, you can use the `ssh` command line program
to log in. If you are on Windows, you have several options. The simplest way to
get going is probably to download the free home edition of
[MobaXterm](https://mobaxterm.mobatek.net/download.html) which provides (among
many other tools) an SSH client. You may prefer to use another client if you
are used to it, but we only support MobaXterm and may not be able to help you
if you run into problems using another client.

## Create an SSH keypair

You may already have an SSH keypair ready to use, but we recommend that you
create a new one ready to use with ARCHER2 simply as this is more secure than
reusing the same key.

An SSH keypair is made up of two keys stored as separate files: a public key and
a private key. The public key needs to placed on the remote system you want to
connect to, in this case ARCHER2. The private key should never leave the machine
you created it on, so your laptop or workstation. Then, later on, when
connecting to ARCHER2, the two key files will be used along with your password
to authenticate your access to the system.

How to create a keypair will depend on your operating system.

### Create an SSH keypair on Linux and macOS

On Linux and macOS you can use the `ssh-keygen` tool to create a new key. Start
a terminal and run the command

```
user@machine:~> ssh-keygen -t rsa -C "myname@email.com"
```
{: .language-bash}

```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa): [Enter]
Enter passphrase (empty for no passphrase): [Passphrase]
Enter same passphrase again: [Passphrase]
Your identification has been saved in /Home/user/.ssh/id_rsa.
Your public key has been saved in /Home/user/.ssh/id_rsa.pub.
The key fingerprint is:
03:d4:c4:6d:58:0a:e2:4a:f8:73:9a:e8:e3:07:16:c8 your@email.com
The key's randomart image is:
+--[ RSA 2048]----+
|    . ...+o++++. |
| . . . =o..      |
|+ . . .......o o |
|oE .   .         |
|o =     .   S    |
|.    +.+     .   |
|.  oo            |
|.  .             |
| ..              |
+-----------------+
```
{: .output}

The text following the `-C` option is a comment that can help identify which key
is which. You might want to add to this comment to mention also which local
machine you are using to log in to ARCHER2, in case you end up logging into the
system from multiple local machines.

If you've never used these keys before, you can safely press the enter key to
use the default key names of `id_rsa` and `id_rsa.pub`, placing them in your
`$HOME/.ssh` directory. If you have used the default keys in that directory
before, you'll probably want to create new ones: either with different names, or
perhaps in a different location. In that case, enter the absolute path and name
of the private key at the first prompt. That private key will be generated, and
the public key will be generated just the same but with a `.pub` extension.

> ## Non-standard key names or locations
> If your key is called anything other than `id_rsa`, or if is located somewhere
> other than `$HOME/.ssh`, the SSH client may have trouble finding it. We will
> cover telling SSH how to find the key in the section below on connecting to
> ARCHER2.
{: .callout}

You should also enter a key passphrase when prompted, rather than leaving it
empty.

Once this has been done, you are ready to add this public key to your ARCHER2
account.

### Create an SSH keypair on Windows with MobaXterm

As mentioned above, we strongly recommend using MobaXterm to connect to ARCHER2
from Windows. You may choose to use a different SSH client, but we may not be
able to provide support if you run into difficulties.

Start MobaXterm. Click on the 'Tools' item in the menu along the top of the
window, and then on 'MobaKeyGen' from the drop-down menu. A new window will
open.

You can leave the options along the bottom of the new windows as-is. Click on
the 'Generate' button. At this point, you'll see a message in the large blank
area asking you to move the mouse cursor around in order to generate randomness.
Do so until the bar fills up to generate the random key.

Once this has been done, you'll see the text of the public key appear in a new
text box. There are three text boxes below for you to fill out. The first is a
comment you can add to identify the key. Enter your email address. You might
want to add to this comment to mention also which local machine you are using to
log in to ARCHER2, in case you end up logging into the system from multiple
local machines. In the next two boxes enter the passphrase you want to use to
unlock the key for use.

Finally, save the public and private keys by pressing these two buttons. The
private key will be saved with a `.ppk` (PuTTY Private Key) extension, while the
public key won't have an extension. You can place these wherever you like with
whatever name you like, as long as it makes sense to you. Perhaps you could
create a directory calls `sshkeys` in your home directory and inside it place
the two keys, `archer2.ppk` and `archer2`.

You should keep this window open just for now. The format MobaXterm saves its
keys in can't be used with other SSH clients, so the best way to add the key to
your ARCHER2 account is to copy and paste it directly from this window.

## Create a SAFE account

If you don't already have a SAFE account, you will need one in order to create
an account on ARCHER2.

The SAFE is located online at
[https://safe.epcc.ed.ac.uk](https://safe.epcc.ed.ac.uk). Go there and click on
the 'Create an account' link to set up a new account. You will be required to
enter some personal information in order to register, including details of your
institution.

## Create an ARCHER2 account

Getting a new account for this course on ARCHER2 is managed via the SAFE. If
you've used ARCHER2 or another EPCC system before, this process will be familiar
to you. Go to [https://safe.epcc.ed.ac.uk](https://safe.epcc.ed.ac.uk) and log
in with your SAFE account details.

Once you are logged in, mouse over 'Login accounts' on the web page's top menu
bar and then, once the menu has popped up, click on 'Request login account'. The
next page will request the project under which the account should be created.
Make sure that you enter the code `{{site.gid}}` -- double check it before you
click the 'Next' button. After this the project managers (in this case the
people running the course) will get an email that allows them to approve your
account. If you enter the incorrect code, the request will be created under the
wrong project, the wrong people will be asked to approve the account, and it
will likely be rejected.

On the next page you will be asked which machine the account should be created
on. There should be only one option, ARCHER2, and it should already be selected.
You can click 'Next' to move on.

On the final page you will need to choose the username you will have on ARCHER2,
accept the terms and conditions of the system's use, and provide an optional SSH
public key. If you have already created a key, as described above, you can
either upload the public key file here or else copy and paste the text in the
file into the text box on this page. If you don't do this now, you will need to
add one to your account once it is active.

If you are using Windows, now is when you can highlight the text of the public
key from MobaXterm, copy it, and paste it into the SAFE. Then, you can close
MobaKeyGen. If you're on Linux or macOS, you can copy the text of the public key
or else upload it directly.

After clicking on 'Request' the project managers will be asked to approve your
account. Assuming you are enrolled on this course they will do so, and you will
then receive another email to inform you of the outcome.

## Add an SSH key and checking your initial password

Once your ARCHER2 machine account has been created, you can go to the SAFE
website again. Once logged in, mouse over 'Login accounts' on the menu bar along
the top of the page. You should now see 'user@archer2', where 'user' is the
username you chose.

Clicking on that will take you to the main page you can use to manage your
account. There is information on this page about your resources and storage on
ARCHER2. You'll see a set of buttons near the bottom of the page. We'll cover
the three which are most important for everyone here.

Firstly, if you didn't add an SSH key to your account you can do so now. Click
on 'Add credential' and then click through to add your public key. As noted
above, the best way to do this from Windows is to keep your MobeKeyGen window
open when you generate the key, then copy and paste the public key's text into
the SAFE. On Linux and macOS you can copy and paste the key or else upload the
file directly.

> ## Logging in from multiple machines
> Remember that an SSH private key should never leave the machine it was generated on.
> That means that if you want to be able to log on to ARCHER2 from any other machines,
> you will need to generate a new SSH keypair. You will then need to add the new
> public key to your ARCHER2 account on the SAFE just as described here. The SAFE
> page for your account will list all SSH keys on the account. You can remove any old
> ones with the 'Delete Credential' button.
{: .callout}

Secondly, the 'View Login Account Password' button will take you to a page where
you can see the password you will use _the first time_ that you log in to
ARCHER2. You will be prompted then to change the password to one of your
choosing. The password shown in the SAFE will not change to reflect that; only
you will know your password.

Thirdly, if you forget or lose your login password, you can come here again and
click on 'Request Password Reset'. You will receive an email shortly afterwards
to tell you that this the password on the machine has been changed, and you can
come back to the SAFE to 'View Login Account Password' again, which is just as
though you were logging in for the first time again.

## Test logging in to ARCHER2

Everything is now in place for you to attempt logging in to ARCHER2. The methods
will be different depending on your operating system.

> ## Passwords and passphrases
> There's potential to get confused between 'passwords' and 'passphrases'. These
> always refer to two specific, different methods of authentication. Your
> 'password' is always the sequence of characters that you obtain from the SAFE
> for your first login and then set in the terminal for subsequent logins. The
> 'passphrase' on the other hand is always the sequence of characters that you
> set when you created your SSH key to keep it locked. You will need to remember
> which is which and provide the correct one when prompted.
{: .callout}

### Log in from Linux and macOS

From Linux and macOS you will log in using the `ssh` program. In its simplest
form, this is:

```
user@machine:~> ssh user@login.archer2.ac.uk
```
{: .language-bash}

If it works, you will need to provide your key's passphrase and the initial
account password that you can retrieve from the SAFE. You will then be told that
you need to change your password to a new one of your choosing. You'll need to
re-enter the initial password from the SAFE, and then enter the new one twice.

If the connection is rejected with a message `Permission denied (publickey)` you
will probably need to tell SSH to use the specific SSH key you set up for your
account. If you created your new private key at `/home/user/.ssh/id_rsa_archer2`
and your new public key at `/home/user/.ssh/id_rsa_archer2.pub`, then you should
run instead:

```
user@machine:~> ssh user@login.archer2.ac.uk -i /home/user/.ssh/id_rsa_archer2
```
{: .language-bash}

You could also create or edit the file `$HOME/.ssh/config` to have a set of
lines reading:

```
Host archer2
  HostName login.archer2.ac.uk
  User user
  IdentityFile /home/user/.ssh/id_rsa_archer2
```

You should change `user` to be your ARCHER2 username. If you do this, you can
then log in using:

```
user@machine:~> ssh archer2
```
{: .language-bash}

where `archer2` is being matched with the `Host` entry in `$HOME/.ssh/config`.

Once you have successfully logged in to ARCHER2, you are ready to follow along
with the lessons in the course.

### Log in from Windows with MobaXterm

Using MobaXterm, we'll set up a reusable ARCHER2 SSH session.

Once MobaXterm is open, click on the large 'Session' icon at the top left. A new
window will open with a row of icons. The leftmost shows a key with 'SSH' --
click on this to define a new SSH session.

In the 'Remote host' text box enter `login.archer2.ac.uk`. Make sure that the
'Specify username' box is ticked, and enter your ARCHER2 username into the text
box next to it. The port should remain `22`.

Now click on the 'Advanced SSH settings' tab below. This will open a new set of
options. Tick the 'Use private key' box. You'll see in the text box next to it
that there is a little file icon. Click on this, then navigate to where you saved
your SSH key files. Choose the `.ppk` file you saved earlier, and then on OK. You
can then click on the Session window's OK button to create the session.

You should now have a `login.archer2.ac.uk` session listed in the left-hand pane
of MobaXterm. If you double-click on it, it will try to connect to ARCHER2. You
will need to provide your key's passphrase and the initial account password that
you can retrieve from the SAFE. You will then be told that you need to change
your password to a new one of your choosing. You'll need to re-enter the initial
password from the SAFE, and then enter the new one twice.

Once you have successfully logged in to ARCHER2, you are ready to follow along
with the lessons in the course.

{% include links.md %}
