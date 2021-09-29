# NVFBC plugin for OBS Studio (Flatpak)

## Requirements

* Relatively modern NVIDIA GPU
* Patched Flatpak NVIDIA drivers (**optional** if you have a Quadro/Tesla model)
* [OBS Studio](https://flathub.org/apps/details/com.obsproject.Studio) (Flatpak)
* This plugin for OBS Studio! :)

## Step-by-step

### 1 - Patching the drivers

**Note**: Skip this step if you have a professional-grade GPU model, such as Quadro or Tesla.

1. Clone the [nvidia-patch](https://github.com/keylase/nvidia-patch) project:

    ```bash
    git clone https://github.com/keylase/nvidia-patch.git
    ```

2. Patch the NVIDIA drivers (host drivers first):

    ```bash
    cd nvidia-patch/
    sudo ./patch.sh
    sudo ./patch-fbc.sh
    ```

3. Now patch the drivers for Flatpak:

    ```bash
    sudo ./patch.sh -f
    sudo ./patch-fbc.sh -f
    ```

4. Reboot your computer.

### 2 - Installing OBS Studio (Flatpak)

1. This should be straightforward:

    ```bash
    flatpak install com.obsproject.Studio
    ```

### 3 - Installing this plugin

1. Download the latest release from [here](https://github.com/guihkx/com.obsproject.Studio.Plugin.NVFBC/releases/latest).
2. Assuming you downloaded the `.flatpak` file to your `~/Downloads` folder:

    ```
    cd ~/Downloads
    flatpak install NvFBC_OBS_Plugin_*.flatpak
    ```

### 4 - Adding the NvFBC Source plugin on OBS

1. Open OBS Studio.
2. Right-click the `Sources` panel, go to `Add` and then select `NvFBC Source`.
3. (Optional) rename your source, hit `OK`.
4. (Optional) tweak the settings (source screen, FPS, etc.), hit `OK`.

    The whole process looks like this:
    ![GIF](../master/images/obs-add-source.gif?raw=true)

## Reverting

If, for whatever reason, you want to revert this whole process, you can restore the original driver files by running:

```bash
# Restores the original driver files for the host
sudo ./patch.sh -r
sudo ./patch-fbc.sh -r
# Restores the original driver files for Flatpak
sudo ./patch.sh -f -r
sudo ./patch-fbc.sh -f -r
```

Uninstall this plugin:

```bash
flatpak uninstall com.obsproject.Studio.Plugin.NVFBC
```

Uninstall OBS Studio **(optional)**:

```bash
flatpak uninstall com.obsproject.Studio
```

Then reboot your computer.

## Manually building this plugin (advanced)

1. Install `flatpak-builder` from your distro repositories.

2. Clone this repository:

    ```bash
    git clone https://github.com/guihkx/com.obsproject.Studio.Plugin.NVFBC.git
    ```

3. Build using `flatpak-builder`:

    ```
    flatpak-builder --user --arch=x86_64 --force-clean --install-deps-from=flathub --repo=repo/ --sandbox builddir/ com.obsproject.Studio.Plugin.NVFBC.yaml
    ```

4. If your build succeeds, you can create your own Flatpak [single-file bundle](https://docs.flatpak.org/en/latest/single-file-bundles.html) (where `NvFBC_OBS_Plugin.flatpak` is the output file):

    ```
    flatpak build-bundle repo/ NvFBC_OBS_Plugin.flatpak com.obsproject.Studio.Plugin.NVFBC stable --runtime
    ```
