# ESP32 test port of ZMK

### Init ZMK
````
cd zmk
west init -l app/
west update
west zephyr-export
pip3 install --user -r zephyr/scripts/requirements-base.txt

source zephyr/zephyr-env.sh
````

### Test ARM board
```
cd app
export ZEPHYR_TOOLCHAIN_VARIANT=gnuarmemb
export GNUARMEMB_TOOLCHAIN_PATH=/usr/local/
rm -r build
west build -p -b nice60 
```

Build should be successful

### ESP32

Install toolchain

```
cd ../zephyr
west espressif install
west espressif update

export ZEPHYR_TOOLCHAIN_VARIANT=espressif
export ESPRESSIF_TOOLCHAIN_PATH="${HOME}/.espressif/tools/zephyr"
```

Test if toolchain works

```
west build -p -b esp32 samples/bluetooth/peripheral_hids
west flash --esp-baud-rate 460800
```

Build ZMK for TTGO T1 / ESP32

```
cd ../app
rm -r build
west build -p -b ttgo_t1
west flash --esp-baud-rate 460800
```

### CAVEAT

For now, the linking phase fails because the lines in app/include/linker/zmk-events.ld are not injected into the linker scripts. 
Adding them manually to the generated build/zephyr/linker_zephyr_pre0.cmd, build/zephyr/linker_zephyr_pre1.cmd and build/zephyr/linker.cmd files allows for temporary building and linking of firmware.

Change 
```
. = ALIGN(4);
*(.rodata)
*(.rodata.*)
*(.gnu.linkonce.r.*)
*(.rodata1)
__XT_EXCEPTION_TABLE__ = ABSOLUTE(.);
```
to 
```
. = ALIGN(4);
*(.rodata)
*(.rodata.*)
*(.gnu.linkonce.r.*)
        __event_type_start = .; KEEP(*(".event_type")); __event_type_end = .;
        __event_subscriptions_start = .; KEEP(*(".event_subscription")); __event_subscriptions_end = .;
*(.rodata1)
__XT_EXCEPTION_TABLE__ = ABSOLUTE(.);
```

You will need to edit the linker_zephyr_pre0.cmd and linker_zephyr_pre1.cmd first, then rebuild without the -p flag, then edit the linker.cmd file and finally build without -p.