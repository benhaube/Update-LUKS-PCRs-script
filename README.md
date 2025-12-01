# Update-LUKS-PCRs-script
This Bash script automates the process of clearing TPM PCRs from the LUKS header, registering new PCRs, and regenerating the initramfs. As a Fedora user it became a pain to manually complete this process every time the kernel gets updated, so I wrote this script to make the process easier. This is a full-featured script with a help message, error logging, and checks for proper command syntax. Unfortunately, for the moment, this script will only work on distributions that use `dracut` for regenerating the initramfs. If people find this script useful and I have enough time, I might add the ability to use `mkinitramfs` to regenerate the initramfs, but I have not gotten that far yet. 

## Dependencies
In oder to run this script you need to have the following:
+ A volume encrypted with **LUKS2** format. LUKS(1) volumes will not work.
    + You can check this with the command: `cryptsetup luksDump /dev/your_device`
+ An active **TPM2** chip.
+ Packages:
    + `systemd-cryptenroll`
    + `tpm2-tss` or `sd-encrypt`
    + `dracut`
+ You will need to configure `/etc/crypttab` to tell the boot process to use the TPM2 chip.
    + If your `/root` volume is encrypted you will need to edit the `/etc/default/grub` config file.
    + Instructions for this process can be found [here](https://github.com/benhaube/Linux-Configuration-Tutorials/blob/main/Security/Unlock_LUKS_TPM2.md).

## Install
1. Clone the Github repo:

    ```
    git clone https://github.com/benhaube/Update-LUKS-PCRs-script.git
    ```

2. Enter the directory:

    ```
    cd Update-LUKS-PCRs-script/
    ```

3. Copy the file `update-pcrs` to the `/usr/local/bin/` directory:

    ```
    sudo cp update-pcrs /usr/local/bin/
    ```

4. Set the execution permission for the `update-pcrs` script:

    ```
    sudo chmod +x /usr/local/bin/update-pcrs
    ```

5. You can now run the script (with `sudo`) from any directory:

    ```
    sudo update-pcrs [options] /dev/your_luks_device [optional_pcrs_list]
    ```
    
## Screenshots
![Update PCRs Script output](/screenshots/update-pcrs_output.png "Output")
![Update PCRs Script help message](/screenshots/update-pcrs_help.png "Help")
![Update PCRs Script no path](/screenshots/update-pcrs_no_path.png "No Path")
![Update PCRs Script no sudo](/screenshots/update-pcrs_no_sudo.png "No sudo")
