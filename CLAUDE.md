# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a ZMK firmware configuration repository for split keyboards (Corne and Ferris Sweep/Cradio) using Nice!Nano v2 microcontrollers. Firmware is built via GitHub Actions—there is no local build process.

## Build Process

Firmware builds are triggered automatically on push/PR via GitHub Actions. The workflow (`.github/workflows/build.yml`) uses ZMK's official `build-user-config.yml` reusable workflow. Build artifacts (`.uf2` files) are available in the GitHub Actions run artifacts.

The build matrix is defined in `build.yaml` and specifies:
- Board/shield combinations for each keyboard half
- ZMK Studio support for left halves (enables real-time keymap editing)
- Nice!View display support for Corne

## Repository Structure

- `config/` - Keyboard configurations
  - `*.conf` - ZMK settings (display, sleep, Bluetooth power)
  - `*.keymap` - Key bindings and layers
  - `west.yml` - ZMK dependency manifest (pinned to v0.3)
- `build.yaml` - GitHub Actions build matrix
- `zephyr/module.yml` - Zephyr module definition

## Keyboard Configurations

**Corne** (`corne.conf`, `corne.keymap`):
- Has Nice!View display (requires `cs-gpios` pin 8 configuration in keymap)
- RGB disabled
- 3 layers with conditional layer support

**Ferris Sweep** (`cradio.conf`, `cradio.keymap`):
- No display
- Uses home row mods (HRML/HRMR macros)
- Custom hold-tap behavior configured

## Key Technical Details

- ZMK Studio is enabled on left halves only (via `snippet: studio-rpc-usb-uart` and cmake args)
- Sleep timeouts: 15min idle, 30min deep sleep
- Bluetooth TX power set to +8dBm for better range
- Nice!View SPI configuration must be in the keymap file, not the conf file
