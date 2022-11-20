# asus-rog-flow-x13-tablet-mode

## Overview

This kernel patch fixes the problem, that the linux kernel does not detect "SW_TABLET_MODE" events (see: [https://github.com/CO-1/asus-flow-x13-linux#tablet](https://github.com/CO-1/asus-flow-x13-linux#tablet))
This "is required for Gnome to automatically rotate display and enable virtual keyboard" (- [source](https://github.com/CO-1/asus-flow-x13-linux#tablet).

## Installation

### NixOS

Add the following to your `configuration.nix`:

```nix
# tablet-mode patch
  boot.kernelPatches = [
    { name = "asus-rog-flow-x13-tablet-mode";
      patch = builtins.fetchurl {
        url = "https://raw.githubusercontent.com/IvanDovgal/asus-rog-flow-x13-tablet-mode/main/support_sw_tablet_mode.patch";
        sha256 = "sha256:1qk63h1fvcqs6hyrz0djw9gay7ixcfh4rdqvza1x62j0wkrmrkky";
      };
    }
  ];
```

Then perform a `sudo nixos-rebuild switch` and reboot.

## Debugging

Once installed, check if tablet-mode gets detected using libinput:

`libinput debug-events`

Then put your laptop into tablet-mode and back into laptop-mode by rotating the lid. You should see the following output in your terminal:

```
 event17  SWITCH_TOGGLE           +0.000s       switch tablet-mode state 1
 event17  SWITCH_TOGGLE           +4.199s       switch tablet-mode state 0
```
