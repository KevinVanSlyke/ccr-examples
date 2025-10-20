# Example R Container  

This example demonstrates how to configure and run a [Rocker Project](https://rocker-project.org/) container to utilize recent versions of R with [CRAN](https://cran.r-project.org/) packages on the CCR software environment using Apptainer. Note that external documentation linked to here is for supplementary reference and will not work without modification on the CCR software environment. The files in this example have implemented such required modifications and serve as a basic example which you can modify to suit your needs.

## Building the container

1. Pull and modify a Rocker image using a definition file. An example definition file is provided in [R.def](R.def). The following outline steps through how to modify your container in more detail.

```bash
FROM rocker/r-ver:4.3.0 # Or the desired R version

# Install additional packages
RUN R -e "install.packages(c('ggplot2', 'dplyr'))"

# You can add more packages here as needed

# Set working directory (optional)
WORKDIR /app

# Copy any local scripts or files (optional)
# COPY . .

# Example command to run R
CMD ["R", "-e", "cat('R environment ready\\n')"]
```

2. Start an interactive job

Apptainer is not available on the CCR login nodes and the compile nodes may not provide enough resources for you to build a container.  We recommend requesting an interactive job on a compute node to conduct this build process.  This will allow you to test your build after completion as well.  Please refer to our documentation on [running jobs](https://docs.ccr.buffalo.edu/en/latest/hpc/jobs/#interactive-job-submission) for more information.  This is provide as an example only and not all users will have access to the resources in this example:  

```bash
$ salloc --cluster=ub-hpc --partition=general-compute --qos=general-compute --mem=32GB --time=02:00:00
salloc: Pending job allocation 19319338
salloc: job 19319338 queued and waiting for resources
salloc: job 19319338 has been allocated resources
salloc: Granted job allocation 19319338
salloc: Nodes cpn-h23-04 are ready for job
CCRusername@cpn-h23-04:~$
```



## Running the container

For our example, we have no additional conda packages installed.  However, we can test this container by running Python:

```bash
CCRusername@cpn-h23-04:~$ apptainer exec conda-$(arch).sif python
Python 3.10.16 | packaged by conda-forge | (main, Apr  8 2025, 20:53:32) [GCC 13.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Alternatively, you can first get shell access into the container and then run Python, as shown here:

```bash
CCRusername@cpn-h23-04:~$ apptainer shell conda-$(arch).sif
Apptainer> python
Python 3.10.16 | packaged by conda-forge | (main, Apr  8 2025, 20:53:32) [GCC 13.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Please refer to the CCR [container documentation](https://docs.ccr.buffalo.edu/en/latest/howto/containerization/) for more info on accessing project directories, using GPUs, and other important topics.
