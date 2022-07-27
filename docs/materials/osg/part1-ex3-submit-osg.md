---
status: testing
---

# OSG Exercise 1.3: Running Jobs in OSG

The goal of this exercise is for you to run jobs in OSG,
specifically the Open Science Pool,
and map their geographical locations.

## Where in the world are my jobs? (Part 2)

In this version of the geolocating exercise,
you will submit jobs to the OS Pool from `login05.osgconnect.net`
and hopefully get back interesting results!
You will use the same job (software) as you did in [OSG exercise 1.1](part1-ex1-submit-refresher.md).

### Gathering network information from OS Pool jobs

This may be your first OSG job, ever!

1.  If not already logged in, `ssh` into `login05.osgconnect.net`

1.  Set the default project associated with your account by running the following command:

        :::console
        connect project

    If you are given a choice and do not know which project to select, reach out to OSG staff.
    The default project will stay in effect until changed at a later time.

1.  Make a new directory for this exercise, `osg-ex13` and change into it

1.  If you did not copy the tarball for [OSG exercise 1.1](part1-ex1-submit-refresher.md)
    from `learn` to `login05` in the previous exercise,
    return to [OSG exercise 1.2](part1-ex2-login-scp.md) and do that now.

1.  Unpack the tarball containing the OSG exercise 1.1 files

    Not sure how to do this step?  Revisit [OSG exercise 1.2](part1-ex2-login-scp.md)!

1.  Copy the submit file, wrapper script, and input file (`wn-geoip.tar.gz`)
    from the OSG exercise 1.1 directory into the new OSG exercise 1.3 directory (created above)

1.  Check the submit file&nbsp;— does anything need to change?

1.  For your final run, you will want to run a lot of jobs.
    But remember the idea of testing a small number of jobs first and then scaling up?
    Pick a number of jobs to run as a test and try that.
    Fix any issues and repeat until solid.

1.  When you think the submit file is ready, scale up to 500 jobs and run!

1.  When your “final” run is done (or nearly done),
    use the command from [OSG exercise 1.1](part1-ex1-submit-refresher.md)
    to get the unique set of location coordinates:

        :::console
        user@login05 $ cat location-*.out | sort | uniq

## Mapping your jobs

As before, you will be using <https://www.mapcustomizer.com/> to visualize where your jobs have landed in the OSG.
Copy and paste the collated results from your job output into the Bulk Entry area.
You can omit the `0, 0` line, because it will not be mapped.
Where did your jobs end up?
This exercise gives you a small sense of the scope of the OSG’s Open Science Pool!

## Next exercise

Once completed, move onto the next exercise: [Hardware Differences in the OSG](part1-ex4-hardware-diffs.md)

## Extra Challenge: Cleaning up your submit directory

If you run `ls` in the directory from which you submitted your job, you may see that you now have thousands of files!
Proper data management starts to become a requirement as you start to develop true HTC workflows;
it may be helpful to separate your submit files, code, and input data from your output data.

1.  Try editing your submit file so that all your output and error files are saved to separate directories within your
    submit directory.

    !!! note "Tip"
        Experiment with fewer job submissions until you’re confident you have it right,
        then go back to submitting 500 jobs.
        Remember: Test small and scale up!

1.  Submit your file and track the status of your jobs.

Did your jobs complete successfully with output and error files saved in separate directories?
If not, can you find any useful information in the job logs or hold messages?
If you get stuck, review the [slides from Tuesday](../index.md).
