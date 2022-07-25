---
status: reviewed
---

Data Exercise 2.2: Using Stash for Large Shared Data
=============================================================

This exercise will use a [BLAST](http://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastHome) workflow to
demonstrate the functionality of StashCache for transferring input files to jobs on OSG.

Because our individual blast jobs from [Exercise 2.1](../part2-ex1-blast-proxy) would take a bit longer
with a larger database (too long for an workable exercise), we'll imagine for this exercise that our
`pdbaa_files.tar.gz` file is too large for a web proxy (larger than ~1 GB).
For this exercise, we will use the input from Exercise 2.1, but instead of using the web proxy for the `pdbaa` database,
we will place it in Stash via the OSG Connect server.

Stash is connected to a distributed set of caches spread across the U.S.
They are connected with high bandwidth connections to each other, and to the data origin servers, where your data is
originally placed.

![StashCache Map](../files/osgus19-day4-part2-CacheLocations.png)

We will be using the command `stashcp` to copy files from Stash to the execute hosts.  It has a `cp` like syntax.

Setup
-----

-   Make sure you're logged in to `login04.osgconnect.net`
-   Copy the following files from the previous [Exercise 2.1](../part2-ex1-blast-proxy) to a new directory in `/home/<username>` called `stash-shared`:
    - `blast_wrapper.sh`
    - `blastx`
    - `mouse_rna.fa.1`
    - `mouse_rna.fa.2`
    - `mouse_rna.fa.3`
    - Your most recent submit file (probably named `blast_split.sub`)

Place the Database in Stash
--------------------------------

### Copy to your `public` space on OSG Connect

StashCache provides a public space for you to store data which can be accessed through the caching servers.
First, you need to move your BLAST database into this public directory.
You have already placed files in your `/public` directory in the previous exercise in order for it to be
accessible to the HTTP proxies.
The same directory is accessible to StashCache.

As the `public` directory name indicates, your files placed in the `public` directory will be accessible to anyone's
jobs if they know how to use `stashcp`, though no one else will be able to edit the files, since only you can *place* or
*change* files in your `public` space.
For your own work in the future, make sure that you never put any sensitive data in such locations.

### Check the file on OSG Connect

Next, you can check for the file and test the command that we'll use in jobs on the OSG Connect login node:

``` console
user@login04 $ ls /public/<USERNAME>
```

Now, test a copy of the file from StashCache into your `/home` directory on `login04.osgconnect.net`:

``` console
user@login04 $ stashcp /osgconnect/public/<USERNAME>/pdbaa_files.tar.gz ./
```

Replacing all instances of `<USERNAME>` with your username on `login04.osgconnect.net`.
You should now see the `pdbaa_files.tar.gz` file in your current directory.
Notice that we had to include the **`/osgconnect/public`** and your username in the file path for `stashcp`, which make sure you're
copying from **your** `public` space.

Modify the Submit File and Wrapper
----------------------------------

You will have to modify the wrapper and submit file to use Stash:

1. Delete the `wget` and `export http_proxy` lines from the job wrapper script `blast_wrapper.sh`

2. HTCondor knows how to do stash transfers, so you just have to provide the correct URL in 
   `transfer_input_files`. Note there is no servername (3 slashes in :///) and we instead
   have added an `osgconnect` for the namespace.

        ::file
        transfer_input_files = blastx, $(inputfile), stash:///osgconnect/public/<USERNAME>/pdbaa_files.tar.gz

6. Confirm that your queue statement is correct for the current directory. It should be something like:

        ::file
        queue inputfile matching mouse_rna.fa.*

And that `mouse_rna.fa.*` files exist in the current directory (you should have copied a few them from the previous exercise
directory).

Submit the Job
--------------

Now submit and monitor the job! If your 100 jobs from the previous exercise haven't started running yet, this job will
not yet start.
However, after it has been running for ~2 minutes, you're safe to continue to the next exercise!

Note: Keeping Stash 'Clean'
--------------------------------

Just as for data on a web proxy, it is VERY important to remove old files from Stash when you no longer need them,
especially so that you'll have plenty of space for such files in the future.
For example, you would delete (`rm`) files from `public` on `login04.osgconnect.net` when you don't need them there
anymore, but only after all jobs have finished.
The next time you use Stash after the school, remember to first check for old files that you can delete.

Next exercise
-------------

Once completed, move onto the next exercise: [Using Stash for unique large input](../part2-ex3-stash-unique)

