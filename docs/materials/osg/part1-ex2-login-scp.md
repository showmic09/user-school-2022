---
status: testing
---

# OSG Exercise 1.2: Log In to the OS Pool Access Point

The main goal of this exercise is to log in to an Open Science Pool Access Point
so that you can start submitting jobs into the OS Pool instead of the local cluster at UW–Madison.
But before doing that, you will first prepare a file on `learn` to copy to the OS Pool Access Point.
Then you will learn how to efficiently copy files between the CHTC and OS Pool Access Points.

If you have trouble getting `ssh` access to the OS Pool Access Point, ask the instructors right away!
Gaining access is critical for all remaining exercises.

## Part 1: On the CHTC Access Point

The first few sections below are to be completed on `learn.chtc.wisc.edu`, the UW–Madison CHTC Access Point.
This is still the same Access Point you have been using since yesterday.

## Preparing files for transfer

When transferring files between computers, it’s best to limit the number of files as well as their size.
Smaller files transfer more quickly and, if your network connection fails,
restarting the transfer is less painful than it would be if you were transferring large files.

Archiving tools (WinZip, 7zip, Archive Utility, etc.) can compress the size of your files
and place them into a single, smaller archive file.
The Unix `tar` command is a one-stop shop for creating, extracting, and viewing the contents of `tar` archives
(called *tarballs*).  Its usage is as follows:

-   To **create** a tarball named `<archive filename>` containing `<archive contents>`, use the following command:

        :::console
        user@learn $ tar -czvf <archive filename> <archive contents>

    Where `<archive filename>` should end in `.tar.gz` and `<archive contents>` can be a list of any number of files
    and/or folders, separated by spaces.

-   To **extract** the files from a tarball into the current directory:

        :::console
        user@learn $ tar -xzvf <archive filename>

-   To **list** the files within a tarball:

        :::console
        user@learn $ tar -tzvf <archive filename>

Using the guidance above, log into `learn.chtc.wisc.edu`,
create a tarball that contains the OSG exercise 1.1 directory,
and verify that it contains all the proper files.

### Comparing compressed sizes

You can adjust the level of compression of `tar` by prepending your command with `GZIP=--<COMPRESSION>`, where
`<COMPRESSION>` can be either `fast` for the least compression, or `best` for the most compression (the default
compression is between `best` and `fast`).

While still logged in to `learn.chtc.wisc.edu`:

1.  Create and change into a new folder for this exercise, for example `osg-ex12`
1.  Use `wget` to download the following files from our web server:
    1.  Text file: <http://proxy.chtc.wisc.edu/SQUID/osgschool21/random_text>
    1.  Archive: <http://proxy.chtc.wisc.edu/SQUID/osgschool21/pdbaa.tar.gz>
    1.  Image: <http://proxy.chtc.wisc.edu/SQUID/osgschool21/obligatory_cat.jpg>
1.  Use `tar` on each file and use `ls -l` to compare the sizes of the original file and the compressed version.

Which files were compressed the least?  Why?

## Part 2: On the Open Science Pool Access Point

For many of the remaining exercises, you will be using an OSG Connect Access Point,
which submits jobs into the Open Science Pool.
For the School, the default server is named `login05.osgconnect.net`;
however, if you had an OSG Connect account from before the School (you know who you are),
you *may* be using `login05.osgconnect.net`, so just change the examples as needed.

To log in to the OSG Connect Access Point,
use the username and SSH key that you made when you set up OSG Connect.
If you have any issues logging in to `login05` (or `login04`, if that’s you),
please ask for help right away!

So please `ssh` in to the server and take a look around:

1.  Log in using `ssh login05.osgconnect.net` (or `login04`, if that’s you)
1.  Try some Linux and HTCondor commands; for example:
    *   Linux commands: `hostname`, `pwd`, `ls`, and so on
    *   What is the operating system? `uname` and (in this case) `cat /etc/redhat-release`
    *   HTCondor commands: `condor_version`, `condor_q`, `condor_status -total`

## Transferring files

In the next exercise, you will submit the same kind of job as in the previous exercise.
Wouldn’t it be nice to copy the files instead of starting from scratch?
And in general, being able to copy files between servers is helpful, so let’s explore a way to do that.

### Using secure copy

[Secure copy](https://en.wikipedia.org/wiki/Secure_copy) (`scp`) is a command based on SSH
that lets you securely copy files between two different servers.
It takes similar arguments to the Unix `cp` command but also takes additional information about servers.
Its general form is like this:

```console
scp <source 1> <source 2>...<source N> [username@]<remote server>:<remote path>
```

`<remote path>` may be omitted if you want to copy your sources to your remote home directory
and `[username@]` may be omitted if your usernames are the same across both servers.
For example, if you are logged in to `login05.osgconnect.net`
and wanted to copy the file `foo` from your current directory
to your home directory on `learn.chtc.wisc.edu`,
and if your usernames are the same on both servers,
the command would look like this:

```console
user@login05 $ scp foo learn.chtc.wisc.edu:
```

Additionally, you could *pull* files from `learn.chtc.wisc.edu` to `login05.osgconnect.net`.
The following command copies `bar` from your home directory on `learn.chtc.wisc.edu`
to your current directory on `login05.osgconnect.net`;
and in this case, the username (Net ID) for `learn` is specified:

``` console
user@login05 $ scp net_id@learn.chtc.wisc.edu:bar .
```

Also, you can copy folders between servers using the `-r` option.
If you kept all your files from the HTCondor exercise 1.3 in a folder named `htc-1.3` on `learn.chtc.wisc.edu`,
you could use the following command to copy them to your home directory on `login05.osgconnect.net`:

``` console
user@login05 $ scp -r net_id@learn.chtc.wisc.edu:htc-1.3 .
```

Using this information, try this:
From `login05.osgconnect.net`,
try copying the tarball you created earlier in this exercise on `learn.chtc.wisc.edu`
to `login05.osgconnect.net`.

### Secure copy to your laptop

During your research, you may need to transfer output files
from your submit server to inspect them on your personal computer,
which can also be done with `scp`!
To use `scp` on your laptop, follow the instructions relevant to your computer‘s operating system:

#### Mac and Linux users

`scp` should be included by default and available via the terminal on both Mac and Linux operating systems.
Open a terminal window on your laptop and
try copying the tarball containing the OSG exercise 1.1 from `login05.osgconnect.net` to your laptop.

#### Windows users

WinSCP is an `scp` client for Windows operating systems.

1.  Install WinSCP from <https://winscp.net/eng/index.php>
1.  Start WinSCP and enter your SSH credentials for `login05.osgconnect.net`
1.  Copy the tarball containing OSG exercise 1.1 to your laptop

### Extra challenge: Using rsync

(This last section is about a more advanced tool; you may skip this section if you want.)

`scp` is a common and useful tool for file transfers between computers,
but there are better tools if you find yourself transferring the same set of files to the same location repeatedly.
Another common tool available on many Linux servers is `rsync`,
which has more features than `scp` and is correspondingly more complex.
The invocation is similar to `scp`:
You can transfer files and/or folders,
but the options are different
and, when transferring folders, pay close attention to a trailing slash (`/`),
because it means different things to include or omit that single character!

Here is the general format of an `rsync` command:

``` console
rsync -Pavz <source 1> <source 2>...<source N> [username@]<remote server>:<remote path>
```

`rsync` has many benefits over `scp`,
but two of its biggest features are built-in compression (so you don't have to create a tarball)
and the ability to only transfer files that have changed.
Both of these features are helpful when you have network issues
so that you do not need to restart the transfer from scratch every time your connection fails.

1.  Log in to `login05.osgconnect.net`
1.  Use `rsync` to transfer the folder containing OSG exercise 1.1 on `learn.chtc.wisc.edu` to `login05.osgconnect.net`
1.  In a separate terminal window, log in to `learn.chtc.wisc.edu`
1.  Create a new file in your OSG exercise 1.1 folder on `learn.chtc.wisc.edu` with the `touch` command:

        :::console
        user@learn $ touch <filename>

1.  From `login05.osgconnect.net`,
    use the same `rsync` command to transfer the folder with the new file you just created.
    How many files were transferred the first time?
    How many files were transferred if you run the same rsync command again?

# Next exercise

Once completed, move onto the next exercise: [Running jobs in the OSG](part1-ex3-submit-osg.md)
