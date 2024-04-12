# Tutorial: Registration

*Currently tested for running on NIH Biowulf HPC only.*

## Setting up HPC environment

Connect to NIH Biowulf HPC with PuTTy (Windows) or Terminal (Mac OS X).

### Mounting drives

On the local desktop, setup directory mounting to the HPC drives to facilitate file transfer and directories creation.

You will need to mount 2 folders to your local desktop: one pointing to your own (user) `data/$USER` directory, and another one to the shared `data/GermainLab` directory where the image files for registration will be (temporarily) stored.

Follow the instructions [here](https://hpc.nih.gov/docs/hpcdrive.html) for mounting drives in the NIH Biowulf cluster on the local desktop:

```
Windows:
\\hpcdrive.nih.gov\data\<user>
\\hpcdrive.nih.gov\data\GermainLab

Mac OS X:
smb://hpcdrive.nih.gov/data/<user>
smb://hpcdrive.nih.gov/data/GermainLab
```

### Conda installation (first time)

1. Download the latest Miniconda installer [here](https://docs.conda.io/en/latest/miniconda.html#linux-installers) for Python >=3.9.

2. Copy the installer file to your data drive `/data/$USER`

3. Since NIH Biowulf cluster does not permit complex tasks on the login node, to begin installation, first connect to an interactive node:
```
sinteractive --cpus-per-task=56 --mem=20g
```

Wait for resource allocation, and once granted access, the compute node e.g. `cn1234` will be displayed next to your username in the input prompt.

4. Follow the instructions [here](https://conda.io/projects/conda/en/stable/user-guide/install/linux.html) for installation:
```
cd /data/$USER 
bash Miniconda3-<version>-Linux-x86_64.sh
```

5. Once finished, restart terminal, re-connect to Biowulf server.

6. Check if Miniconda has been installed properly.
```
conda
```

### Cloning Conda environment (first time)

After login, we first need to setup a Conda environment where all the required Python packages and modules are installed. The file containing the environment installation `environment.yml` is located at the master copy of 3D IBEX Registration module: `/data/GermainLab/IBEX`

1. Connect to an interactive node first. **Do not perform the following steps in the login node. The operations will fail.**
```
sinteractive --cpus-per-task=56 --mem=100g --exclusive
```

2. Navigate to the folder containing the `environment_new.yml` file:

```
cd /data/GermainLab/IBEX/
```

3. Clone the Conda environment, using `IBEX` as the name of the Conda environment:
```
conda env create --name IBEX --file=environment_new.yml
```

The Conda environment creation and package installation will proceed. This may take a long time.

4. Check whether the environment has been installed properly, activate then check if the Python binary path is correctly pointed:
```
conda activate IBEX
which python
```
Make sure the Python binary path points to the Python *inside* the custom environment (e.g. `data/<user>/miniconda3/envs/myenv/bin/python`)

## Setting up Project environment

1. Check for available nodes.
```
freen
```

Look for `FreeNds` and the `Features`.

2. If from login node i.e. `[user@biowulf]` in the command prompt, request for an interactive node:
```
sinteractive --cpus-per-task=56 --mem=240g --constraint='x2695'
```

Once the interactive node is allocated, you should see `[user@cn1234]` in your input prompt.

3. Navigate to IBEX directory.
```
cd /data/GermainLab/IBEX
```

4. Activate IBEX Conda environment
```
conda activate IBEX
```
Check and make sure your python binary is pointed to `IBEX`:
```
which python
```
Should display `/data/user/miniconda3/envs/IBEX/bin/python`

If not, deactivate and re-activate conda environment:
```
conda deactivate
conda activate IBEX
```
Check again and make sure the binary is correctly pointed.


5. Run the configuration setup script with your `project_name` as the argument:
```
python -m setup_config "project_name"
```
A new folder with `project_name` should be generated inside `IBEX` folder.

6. Use the mounted drive on your local desktop to create new folders and file transfers.

Your project folder is now: `/data/GermainLab/IBEX/my_project`. This will be the **root folder** of the project.

Within the project folder, there will be four sub-folders:
```
/data/GermainLab/IBEX/my_project/
                                |--input/
                                |--output/
                                |--models/
                                |--temp/

```

Copy Imaris files (both fixed and moving) to the `input` sub-directory.

Next, you will need to setup your configuration files, located in your project root folder:  
```
register_myproject.yaml -- this is the registration configuration file
transform_myproject.yaml -- this is the transform configuration file
```

## Setting up Configuration file

3D IBEX Registration pipeline runs based on a project configuration file in which input/output paths and various registration parameters are defined. The registration pipeline is designed to run on local workstation or via HPC cluster setup (SLURM is currently the only supported scheduler). In this example section, we will configure the registration profile to be deployed on the HPC.

The configuration template is split into several sections, and for the most part, only a subset of the parameters need to be changed for every new project.

**For a quickstart guide on configuring the registration pipeline, follow the configuration example [here](./config_example.md).**  
For full documentation on configuration parameters, check out the detailed guide [here](./config_doc.md).

---


## Execute registration pipeline

Once the configuration file has been properly setup, and input files double-checked to be in the correct locations, we can begin executing the registration pipeline.

Step 1: Make sure local drive has enough space

Step 2: Run PuTTy - `biowulf.nih.gov`

Step 3: Login, then activate Conda environment with the command `conda activate IBEX`

Step 4: Check HPC disk space using `checkquota`

Step 5: If need to remove folders/files **(Make sure you have already backed up the files)**, use  
`rm -rf` command e.g. `rm -rf output` to delete the entire `output` folder.  
**This step is irreversible. Double check everything before deleting!**

Step 6: Setup new project   
`python -m setup_config "project_name"`

Step 7: Navigate on Windows Explorer to new project folder e.g. `project_name`  
Go to `input` sub-folder  
Copy your ims files into the sub-folder  

Step 8: While waiting for files to be copied, you need to configure your registration
Navigate to project folder e.g. `project_name`  
You should see `register_project_name.yaml` as the configuration file  
Use the command `nano register_project_name.yaml` to edit directly

Fill in `FixPath` with imaris file name for reference image
Fill in `MovPath` with imaris file name for image to be aligned
Enter the fiducial channel number in `FixChannel` and `MovChannel`, starting from 0.

Press Ctrl + X to exit, when prompted to save, enter `y`   

Step 9: Ready to run SLURM job
Use `sbatch script_register_project_name.sh` to start job

Check your job running by using the command `sjobs`





### Checking job status

To check available nodes on NIH Biowulf, use the command `freen` to determine which CPU and GPU nodes to use.

By default, we use `constraint=x2695` as our CPU nodes (can be set in the Configuration file), and `gpu_type: p100` or `k80` for GPU nodes, depending on availability.

To check running job status, start another terminal logged into the Biowulf cluster, use the command `sjobs` to see if the allocated nodes are running.


## Execute transformation pipeline

Running all channel transformation, after your registration is finished

Step 1: Copy your irf file from `output` folder to `input` folder **Very important**

Step 2: Setup your configuration file

Use `nano transform_project_name.yaml` to edit the configuration file

Fill in `FixPath` and `MovPath` with the correct imaris file names
If `LoadRegistration` is `false`, change to `true`
If `RegistrationPath` is empty, set to the irf file name copied to your `input` folder e.g. `project_name.irf`

**Double check all file names are correct and in place**

Example:
```
INPUT:
    InputPath: input
    FixPath: 'Exp35-SI_P1.ims'
    MovPath: 'Exp35-SI_P3.ims'
    LoadRegistration: true
    RegistrationPath: Exp35_P3.irf
```

Exit with Ctrl + X, `y` and Enter.


Step 3: Ready to run SLURM job
Use `sbatch script_transform_project_name.sh` to start job

Check your job running by using the command `sjobs`

Step 4: Copy transformed images back to local workstation and convert to Imaris format

The Linux system is unable to convert the transformed dataset into Imaris `.ims` format, so instead `.tif` image for individual channels will be generated in the `output` folder.

Copy the `.tif` files back to local workstation and use the `imaris_multichannel_converter` script to generate `.ims` file.



