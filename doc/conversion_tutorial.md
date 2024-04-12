**ZARR to IMS converter**

---

**Single channel converter**

Use this to convert single fiducial channel after registration is finished.

This can be used for checking global affine (`z_global.zarr`), local affine (`z_local.zarr`) and deform registration (`z_deform.zarr`) before running the full transformation.

**Step 0: Copy registered ZARR file to a local drive folder**

You only need the single channel fiducial registered ZARR file: `z_global.zarr`, `z_local.zarr`, or `z_deform.zarr`.

These files can be found in the `output` folder on the Biowulf project folder.

**Step 1: Setup `project_configuration_single.py`**

Simply change the `input_path` to the folder where the ZARR container is located.

Set the `single_deform` parameter to the ZARR file you want to convert to IMS: `z_global.zarr`, `z_local.zarr`, or `z_deform.zarr`.

If image is not in 12-bit or 16-bit, but in 8-bit, change `bit_info` value to `'uint8'`

```
function_params = { 'input_path' : 'F:/Biowulf/Exp35_P3',
                    'single_deform' : 'z_deform.zarr',
                    'bit_info' : 'uint16'}
```

**Step 2: Run the converter script**

Activate conda environment first, then navigate to the folder where the scripts are located.

Use the command `python converter_script_single.py` to begin conversion.

**Step 3: Crop image**

**IMPORTANT: The output IMS file will be larger than the original image. You have to manually crop the new IMS file before it can be added to the original dataset.**

First, check the original size of the image (open reference dataset in Imaris, go to `Image Properties` and write down the X, Y, Z values.

Then, open the new converted IMS file, go to `Crop 3D`, set the start to 0 for X, Y and Z, then the end with the X, Y, Z values copied from the original image.

Save and overwrite the IMS file.

**Step 4: Add channel to original dataset**

You can now use `Add Channel` in Imaris to add the converted IMS to your original dataset to compare registration.

---

**Multi channel converter**

Use this to convert full transformed image after transformation step is finished.

Usually the file is `full_deform.zarr`

**Step 0: Copy IRF file and `full_deform.zarr` to a local drive folder**

The files you need are the `*.irf` file and `full_deform.zarr`

These files can be found in the `output` folder on the Biowulf project folder.


**Step 1: Setup `project_configuration_multi.py`**


Simply change the `input_path` to the folder where the ZARR container is located.

Set the `irf_file` to the IRF file name.

Set the `full_deform` parameter to the ZARR file you want to convert to IMS, usually `full_deform.zarr`.

`channels_to_process` is defaulted at `all` unless only want to convert specific channels, in which case use a Python list such as `[0, 1, 3, 5]` to convert the specified channels.

If image is not in 12-bit or 16-bit, but in 8-bit, change `bit_info` value to `'uint8'`

```
function_params = { 'input_path' : 'C:/imreg/Exp35_P1_P5',
                    'irf_file' : 'Exp35_P5.irf',
                    'full_deform' : 'full_deform.zarr',
                    'channels_to_process' : 'all',
                    'bit_info' : 'uint16'}
```

**Step 2: Run the converter script**

Activate conda environment first, then navigate to the folder where the scripts are located.

Use the command `python converter_script_multi.py` to begin conversion.

**Step 3: Combine all channels together**

Open the first channel on Imaris, then use use `Add Channel` function in Imaris to add all other channels together.

**There is no need to crop the Imaris dataset because the dataset are already cropped during the transformation steps**

---

