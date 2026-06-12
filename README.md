# Dactyl Manuform with ZMK

My Dactyl Manuform, 5x7, 3D printed, switches, diodes, enamel wire, nice!nano v2 (or a clone). Maybe I'll write about the HW build someday.

## Building

### GitHub Actions

On push to main, the build workflow runs and builds both sides. Go to the actions page, pick the last run, and from the artifacts section download `firmware.zip`.

### Building Locally

If you want to develop locally, you can follow the instructions in [the ZMK docs](https://zmk.dev/docs/development/local-toolchain/setup), or follow this tl;dr with Docker:

Clone this repo somewhere, and also [ZMK](https://github.com/zmkfirmware/zmk) (tested at v0.3).

Start a shell in the dev container with:

```bash
docker run \
  -v /path/to/zmk-rbw-dactyl:/workspaces/zmk-modules/zmk-rbw-dactyl \
  -v /path/to/zmk:/workspaces/zmk \
  -e WORKSPACE_DIR=/workspaces/zmk \
  -w /workspaces/zmk \
  -it \
  zmkfirmware/zmk-build-arm:3.5
```

In the container shell:

```bash
west init -l app/
west update
cd app
west build -p -d build/left -b nice_nano -- -DSHIELD=rbw_dactyl_left -DZMK_EXTRA_MODULES=/workspaces/zmk-modules/zmk-rbw-dactyl/
west build -p -d build/right -b nice_nano -- -DSHIELD=rbw_dactyl_right -DZMK_EXTRA_MODULES=/workspaces/zmk-modules/zmk-rbw-dactyl/
```

Now you have built the firmware for both halves. Find it in `zmk/app/build/${side}/zephyr/zmk.uf2`.

## Flashing

Now flash the UF2 file into each nice!nano. Start by connecting via USB then putting it in UF2 bootloader mode, pressing reset twice within 500ms or so. Then drag the corresponding firmware UF2 file into the storage device that showed up. It'll auto-reboot when it's done.
