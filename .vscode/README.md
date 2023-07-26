# VS Code Configuration

Configuration for debugging programs in-editor with VS Code.  
This directory contains the configuration for the following platform:

 - `STM32L4S5x` via OpenOCD

## Required Extensions

If you have the `code` command in your path, you can run the following commands to install the necessary extensions.

```sh
code --install-extension rust-lang.rust-analyzer
code --install-extension marus25.cortex-debug
```

Otherwise, you can use the Extensions view to search for and install them, or go directly to their marketplace pages and click the "Install" button.

- [Rust Language Server (rust-analyzer)](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)
- [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug)

## Use

The repository comes with one debug configuration.
It is configured to build the project, using the default settings from `.cargo/config`, prior to starting a debug session.

1. OpenOCD: Starts a debug session for a `STM32L4S5 Discovery kit` (or any `STM32L4S5x` running at 120MHz).
   - Follow the instructions above for configuring the build with `.cargo/config` and the `memory.x` linker script.

## Customizing for other targets

For full documentation, see the [Cortex-Debug][cortex-debug] repository.

### Device

Some configurations use this to automatically find the SVD file.  
Replace this with the part number for your device.

```json
"device": "STM32L4S5VIT6",
```

### OpenOCD Config Files

The `configFiles` property specifies a list of files to pass to OpenOCD.

```json
"configFiles": [
    "interface/stlink.cfg",
    "target/stm32L4x.cfg"
],
```

See the [OpenOCD config docs][openocd-config] for more information and the [OpenOCD repository for available configuration files][openocd-repo].

### SVD

The SVD file is a standard way of describing all registers and peripherals of an ARM Cortex-M mCU.  
Cortex-Debug needs this file to display the current register values for the peripherals on the device.  

You can probably find the SVD for your device on the vendor's website.  

For example, the STM32L4S5 Discovery kit uses an mcu from the `STM32L4+` line of processors.  
All the SVD files for the STM32L4+ series are available on [ST's Website][stm32l4+].  
Download the corresponding [SVD file][stm32l4s5-svd], and copy the `STM32L4S5.svd` file into `.vscode/`.  
This line of the config tells the Cortex-Debug plug in where to find the file.

```json
"svdFile": "${workspaceRoot}/.vscode/STM32L4S5.svd",
```

For other processors, simply copy the correct `*.svd` file into the project and update the config accordingly.

### CPU Frequency

If your device is running at a frequency other than 120 MHz, you'll need to modify this line of `launch.json` for the `ITM` output to work correctly.

```json
"cpuFrequency": 120000000,
```

### Other GDB Servers

For information on setting up GDB servers other than OpenOCD, see the [Cortex-Debug repository][cortex-debug].

[cortex-debug]: https://github.com/Marus/cortex-debug
[stm32l4+]: https://www.st.com/en/microcontrollers-microprocessors/stm32l4-plus-series.html
[stm32l4s5-svd]: https://github.com/posborne/cmsis-svd/blob/master/data/STMicro/STM32L4S5.svd
[openocd-config]: http://openocd.org/doc/html/Config-File-Guidelines.html
[openocd-repo]: https://sourceforge.net/p/openocd/code/ci/master/tree/tcl/
