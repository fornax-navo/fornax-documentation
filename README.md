Fornax Science Console
================

## What is the Fornax Science Console?

The Fornax Science Console is a compute system in the cloud near to NASA data in the cloud which provides a place where astronomers can do data intensive research with reduced barriers.  The first barrier we remove is the need to install software and deal with packages on your personal computer.  This greatly simplifies the setup and startup for any astronomer who now no longer needs to worry about setting up a python working environment.  Kernels with pre-installed astronomy software are provided upon login to Jupyterlab for all users of Fornax.  The second barrier we remove is the need for every astronomer to buy compute/memory commensurate to data science level projects.  You will no longer need to buy the fastest/ most memory  personal computer.  When you need those capabilities, we will have it ready for you.  When you need to just write code, we have smaller compute ready for that too.  These two things (increased compute/memory and ease of use) should lower the barrier of entry to data science projects for all NASA astronomers.  You no longer need to have an expensive computer, you no longer need to be an expert at installing software, you just need to have an idea!  Lastly, by lowering the barrier to entry, we also increase the potential for reproducibility of large data results in astronomy.  Before, if you were wanting to reproduce some data intensive work done in the literature, it would have been challenging to to have the right compute and setup, now you just need a login....  

* What does it do? (Basic Capabilities)
  * Increased compute,
  * Increased memory,
  * Increased reproducibility,
  * Increased inclusion,
  * Increased ease of use
* Who is it for?
  * US based astronomers??  is this true??  all astronomers??
* Limits: What does it not do?
  * How many cores/RAM do I get?
    * In JK's understanding (which needs to be updated) you get what the numbers say when you choose a server upon login, but for a limited time if you need slightly more, the code will not crash but will have access to slightly more. 
    * Raen has seen some evidence of this "bursting" behavior, but doesn't have direct knowledge of the actual configuration. It would be good to know.
  * How much disk space do I have access to?
    * Current default is 10GB (Feb 2024).
  * What restrictions are there to prevent egress charges?
    * Raen's understanding is:
        * Users ought to be able to access any data they both want and have permissions/credentials for, regardless of where it is (AWS S3, Google's GCS, NASA archive, personal computer, etc.).
        * Any applicable egress charges are generally outside the purview of Fornax (more in the "aside" below). The exception would be egress on data that the user exports from the platform (download to local machine, send to a bucket, etc.). I don't know the Fornax configuration to know specifics, or whether it's something that developers and users need to be aware of.
            * Aside:  Historically, the data holder (e.g., IRSA) typically covers the cost (e.g., egress) of delivering the data to the user, but cloud storage buckets are starting to change both the workflows and the cost models. Buckets support large-scale data access in ways that an archive like IRSA cannot support from on-premise. This is great, but also means more data requests and larger (and less predictable) costs. Data holders can often get the costs covered through grants, arrangements with the cloud provider (e.g., AWS), etc. But, in some cases they will decide that the best option is to make the data available in a bucket under what I would call the "cloud-pricing model" and sum up as "everyone pays for what *they* use". In particular, this means the data holder will pay the *storage* costs (which they can predict and easily control), and the requestor/end user will pay the *access* costs including egress (which the data holder cannot easily predict or control, but the requestor can). Individual charges are generally small and reasonable when spread around in this way, and the the cloud provider often offers free access up to some small but reasonable limit. However, egress is a particular fee that often does not apply at all, but in other cases can balloon to $$$ very quickly. So it is absolutely something to be considered, planned for, and controlled. Lastly, who pays the access/egress costs is determined by a setting on the bucket. If the bucket is "requestor pays", the user/requestor will need (e.g.,) AWS credentials to access it -- charges are then billed to the AWS account that owns the credentials.
        * As far as I can tell, the best (AWS) option for actual controls -- beyond just "monitoring" support -- is [AWS Budgets Actions](https://aws.amazon.com/blogs/aws-cloud-financial-management/get-started-with-aws-budgets-actions/).
        * AWS may charge *ingress* fees to bring data into an SMCE pod or user instance. This would be completely separate from any egress fees. There Someone working more directly on Fornax Daskhub would need to answer whether/how ingress applies.

## Getting started
* How do I get an account?
* How to Log in?
  * Log in at  https://daskhub.fornaxdev.mysmce.com/
* How to end a session?
  *  go to `File` Menu: `hub control panel` and `stop my server`.  This insures the server is not still running when you logout which would be a waste of resources
  *  then logout in the upper right
* How to choose which size server to open upon login?
  * Make sure to use `mini` or `standard` size for writing/debugging/testing before switching to larger sizes for full runs of code at scale
  * `On demand` means an AWS server that starts when the user asks for it, and runs as long as you continue to use and pay for it.  This is in contrast to the `spot` servers at AWS which are used for short runs and are cheaper, but can be revoked at any time (per AWS needs)
  * 128 core: do not use unless given permission
* Navigating jupyter lab
  * How do I start a new notebook?
    * The blue `+` in the upper left brings you to the launcher where you can start a new, empty notebook or open a terminal window
  * How do I get a terminal window?
    * The blue `+` in the upper left brings you to the launcher where you can start a new notebook or open a terminal window
  * How to upload data into the Fornax Science Console?
    * The uparrow in the upper left allows you to upload data
    * if it is a large amount of data, consider creating a zip or tar archive first
    * From within Jupyter Lab, you can also use the shell to 'scp' or 'git pull' from external sites that are publicly visible.
    * What are our storage limits for uploaded data?
    * How will I know if I run into a storage limit?
  *  How to download data from the plaltform to my local machine?
     * If it is a small file, you can right click on the file name in the file browser and scroll to `Download`
     * if it is a large amount of data, consider creating a zip or tar archive first
  * Home Directory
    * When you log into the science console, the active directory is your $HOME directory.  This directory is unique to you: edits and uploads are not visible to other users.  
        * Raen thinks there are directories in $HOME that are shared (e.g., `efs` and `s3`), and perhaps just mirrored or symlinked into $HOME. It would be nice to get clarification. Specifically:
            * Which directories are/aren't shared?
            * Which directories does the user have write access to, and are there any restrictions/considerations for putting stuff there (other than disk size)?
            * Which directories can/should the user look in to discover data they have access to? (e.g., `s3` has various archive data, `efs` has some data shared by users, anything else?)
  * Does my work persist between sessions?
    * Files in your home directory will persist between sessions.
    * pip installs will persist across kernel restarts, but not across logging out and back in.
      * If you want software installs to be persistent, consider setting up an environment: See below under "Making a conda environment that persists across sessions"
  * What is the info at the bottom of the jupyterlab window
    * The github branch is listed as well as the name of the kernel in use
    * the kernel is listed as either 'idle' or 'busy' which is useful to know if your kernel is working or has crashed.
* How do I share large amounts of data from inside the SP with (international) collaborators?
  * Download them to favorite storage place (university Box account) or put in AWS cloud (put $$ in your proposals to cover this)
* Is there a way to go directly from Fornax to my University's Box account?
* Is there a way to go directly from Fornax into a different AWS bucket that my project may pay for?
* How do I know what computing resources are available to me?
  * in jupyter hub - open a terminal window by going to the file folder in the upper left, clicking on the plus sign
    * `nproc` will give you the number of processors
    * `cat /proc/cpuinfo` will give you more detailed info on the processors
    * `free` will give the amount of RAM available/used
    * `cat /proc/meminfo` will give the amount of RAM available/used
    * ??is this even true for the AWS instances?
* How do I save my notebook as a python script?
  * `jupyter nbconvert --to script notebookname.ipynb`
* Save your work!
   * the Fornax Science Console will cull servers after a user is inactive for a certain amount of time - what is that time limit??
* How do I run a notebook non-interactively?
* How to open a plot (png, pdf, etc.) either generated by a notebook or uploaded ?
  * double clicking on them in the file browser will open them in a new tab
* What is a kernel and how do I choose one?
  * In Jupyter, kernels are the background processes that execute cells and return results for display.
  * To select the kernel on which you want to run your Notebook, go to the Kernel menu and choose Change Kernel. You can also click directly on the name of the active kernel to switch to another.
* Will notebooks that I run on the SP also work on my laptop?
  * To run the demo notebooks on your laptop, you will also need to have the container that we use which includes software used by the notebooks.
    * see below under "Can I run the container from the SP on my own personal computer/laptop?"
* Do I need to worry about costs when working in the SP?
  * NASA will pay for the work that you do, but please be mindful of those costs.
* How do I know what costs I am incurring?
* Can I do code development in emacs or vi or some other IDE?
  * Emacs or vi is possible from the terminal
* Is there a limit to the number of packages a user can install?
  * There is a limit on the space a user has access to, but not the number of packages, and packages are usually small.


## Data Access
* How do I add my own data?
  * small files can be added with the upload button, which is an `uparrow` in the upper right
  * large file transfers??
* Where should I store my data in the SP?
  * In your home directory??? (Would be nice to be more specific. See "Home Directory" comments above.)
* How to access images in the cloud?
  * [Tutorial](https://github.com/spacetelescope/tike_content/blob/main/content/notebooks/data-access/data-access.ipynb) notebook on STScI data
  * Where is Abdu's similar notebook with pyvo tools that was used for the July2023 HQ demo?
  * placeholder for Brigitta's SIA access notebook
* How to access catalogs in the cloud?
  * placeholder for Troy's parquet access notebook

## Managing Software
* Making a conda environment that persists across sessions
  * If the pre-installed environments don't have the software you need, you can create your own persistent environment available across multiple sessions.
  * follow [conda documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)
  * specifically [managing environments](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html#managing-envs)
* Can I get a list of what software is already pre-installed on the Fornax Science Console?
* How do I install my own software?
  * Persistent User-Installed Software
    *  See "making a conda environment that persists across sessions" above 
  * Non-persistent User-Installed Software 
    * you can !pip install your favorite software from inside a notebook.  This installed software will stay through kernel restarts, but will not be persistent if you stop your server and restart it (logging out and back in).
    * For the tutorial notebooks we tend to have a requirements.txt file in the repo which lists all the software dependencies.  Then the first line in the notebook is `!pip install -r requirements.txt`  That way other people can run the notebook and will know which software is involved.
* What is the terminal command to list package version info using pip?
  * `pip show packagname`
* Can I launch apps from icons? Like MOPEX or SPICE
  * no
* Can I run licensed software (IDL) in the SP?
  * no
* Can I bring my own docker image?
* Can I run the container from the SP on my own personal computer/laptop?

## Examples and Tutorials
* Cloud ([STScI](https://github.com/spacetelescope/tike_content/blob/main/content/notebooks/data-access/data-access.ipynb))
* [IRSA Cloud Access Introduction](https://irsa.ipac.caltech.edu/docs/notebooks/cloud-access-intro.html)
* [Parquet](https://irsa.ipac.caltech.edu/docs/notebooks/wise-allwise-catalog-demo.html)
* Image cutouts
* Optimizing code for fast processing
  * By looking inside the code for slow pieces (CPU profiling)
    * profiliing within the SP is possible, however vizualizing the profile is not yet possible
    * profiling needs to be done on a .py script, and not a jupyter notebook
    * sample command on the SP command line: `python -m cProfile -o output_profile_name.prof  code_name.py`
    * Then download the .prof file
    * On your local computer command line: `python -m snakeviz output_profile_name.prof`
    * documentation for snakeviz: https://jiffyclub.github.io/snakeviz/
    * This really only looks at CPU usage
  * By looking inside the code for memory troubles [memory profiling](https://towardsdatascience.com/profile-memory-consumption-of-python-functions-in-a-single-line-of-code-6403101db419)
    * inside the notebook:
      * `pip install -U memory_profiler`
      * `from memory_profiler import profile`
      * above the function you want to check add this line: @profile
      * run the script: python -m memory_profiler <filename>.py > mem_prof.txt
  * By parallelization
    * Python built in [multiprocessing](https://irsa.ipac.caltech.edu/docs/notebooks/Parallelize_Convolution.html)
    * Daskhub gateway
* [MAST](https://github.com/spacetelescope/tike_content/blob/main/markdown/science-examples.md)
* HEASARC [sciserver_cookbooks](https://github.com/HEASARC/sciserver_cookbooks/blob/main/Introduction.md)
* [Cross matching two large catalogs](https://github.com/IPAC-SW/ipac-sp-notebooks/blob/main/gaia_cross_SEIP/gaia_cross_SEIP.ipynb)
* [Work with theoretical catalogs](https://irsa.ipac.caltech.edu/data/theory/Cosmosims/gator_docs/CosmoDC2_Mock_V1_Catalog.html)
* Fully worked science use cases
  * [Forced photometry](https://github.com/fornax-navo/fornax-demo-notebooks/blob/f9306846777541feeddb53572c0469fc09869a9a/forced_photometry/)
  * [Light curves](https://github.com/fornax-navo/fornax-demo-notebooks/blob/f9306846777541feeddb53572c0469fc09869a9a/light_curves/)
* User contributions: open issue or PR on Github (which Github repo?)

## Troubleshooting
* If my internet connection goes away or is intermittent - what happens to the running notebook?
* Restart kernel will solve some problems, especially if the cells were run out of order
  * Under the `kernel` menu: `restart kernel...`
* Restart SP - will solve version issues if packages with different versions were loaded since these won’t persist across SP restarts
* helpdesk

## Getting Help
* We need to setup a helpdesk!!


## Additional Resources
* New to Python? -
  * [numpy tutorials](?)
  * [Scipy lecture notes](https://scipy-lectures.org/)
  * [Data science handbook](https://jakevdp.github.io/PythonDataScienceHandbook/)
* [Guide to documenting python functions](https://developer.lsst.io/python/numpydoc.html#numpydoc-sections-in-docstrings)
* [Github](https://docs.github.com/en/get-started/quickstart)
* [Debugging](https://jakevdp.github.io/PythonDataScienceHandbook/01.06-errors-and-debugging.html#Debugging:-When-Reading-Tracebacks-Is-Not-Enough)
* Three NASA archives are involved in this work
  * [IRSA](https://irsa.ipac.caltech.edu/frontpage/)
  * [HEASARC](https://heasarc.gsfc.nasa.gov)
  * [MAST](https://archive.stsci.edu)
* [Jupyterlab](https://jupyter.org)
