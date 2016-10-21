# Jenkins builder

This bundle creates a Jenkins environment to build projects from source as 
part of a build pipeline. The goal is to have Jenkins build the binaries and
pass it off to another step in the process.

## Overview

This bundle deploys the jenkins charm and uses a subordinate charm named 
jenkins-workspace to configure a custom workspace. 

**WARNING**: This particular bundle uses Juju constraints to request a very 
large VM which can be very expensive. Read the [constraints](#constraints) 
section for more information.

## Usage

To deploy this bundle:

```
juju deploy ./bundle.yaml
juju config jenkins password=YourSuperSecretPassword.
```

You should now be able to go to the public address of the jenkins charm and 
log in as admin with the password you entered.

Create jobs and build away!

## Constraints

The amount of RAM required to build some projects s greater than 11GB so this 
bundle uses Juju constraints to request a system large enough (mem=12G) to run
Jenkins and execute a build without consuming all the memory causing build 
failures.

We also saw build failures when the system ran out of disk space. Which is 
also solved by using Juju constraints where we request more attached storage
(root-disk=50G) to keep the logs and other build artifacts on the system.

In an effort to build faster, Juju constraints was used to request more virtual
CPUs (cpu-cores=4). This gives the builder the ability to use more threads for
compilation of the project.

**WARNING**: The combination of these additional constraints makes Juju request
a very large system from your cloud provider. It is relatively expensive to 
run this bundle for long periods of time. Remember to destroy the machines when
they are not in use or no longer needed.

## Contact information

If you have any problems using the bundle please open a 
[github issue](https://github.com/juju-solutions/bundle-jenkins-builder/issues)
with the bundle.
