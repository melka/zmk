# Init ZMK
````
cd zmk
west init -l app/
west update
west zephyr-export
pip3 install --user -r zephyr/scripts/requirements-base.txt

source zephyr/zephyr-env.sh
````

# Test ARM board
cd app
export ZEPHYR_TOOLCHAIN_VARIANT=gnuarmemb
export GNUARMEMB_TOOLCHAIN_PATH=/usr/local/
rm -r build
west build -p -b nice60 

# ESP32
cd zephyr
west espressif install
west espressif update

export ZEPHYR_TOOLCHAIN_VARIANT=espressif
export ESPRESSIF_TOOLCHAIN_PATH="${HOME}/.espressif/tools/zephyr"

west build -p -b esp32 samples/bluetooth/peripheral_hids
west flash --esp-baud-rate 460800

cd ../app
rm -r build
west build -b samsung60

mkdir build_backup
cp build/zephyr/zmk.* build_backup

Add this to linked scripts
         __event_type_start = .; KEEP(*(".event_type")); __event_type_end = .;
         __event_subscriptions_start = .; KEEP(*(".event_subscription")); __event_subscriptions_end = .;

west flash --esp-baud-rate 460800

// NO LONGER NEEDED ! IT LINKS !
esptool.py --chip esp32 elf2image -ff 80m -fm dio -fs 4MB -o build/zephyr/zmk.bin build/zephyr/zmk.elf
BT MAC : A4:CF:12:24:CD:52


SUCCESFUL BOOT ON SAMPLE

*** Booting Zephyr OS build cd0742651723  ***
Bluetooth initialized
Advertising successfully started
[00:00:00.542,000] <inf> fs_nvs: 6 Sectors of 4096 bytes
[00:00:00.542,000] <inf> fs_nvs: alloc wra: 5, f88
[00:00:00.542,000] <inf> fs_nvs: data wra: 5, c0
[00:00:00.543,000] <inf> esp32_bt_adapter: BT controller compile version [1342a48]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .data initialise [0x3ffae6e0] <== [0x4000d890]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffb0000] - [0x3ffb09a8]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffb09a8] - [0x3ffb1ddc]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffb1ddc] - [0x3ffb2730]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffb2730] - [0x3ffb6388]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffb8000] - [0x3ffb9a20]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffbdb28] - [0x3ffbdb5c]
[00:00:01.040,000] <inf> bt_hci_core: No ID address. App must call settings_load()
[00:00:01.042,000] <inf> bt_hci_core: Identity: A4:CF:12:24:CD:52 (public)
[00:00:01.042,000] <inf> bt_hci_core: HCI: version 4.2 (0x08) revision 0x030e, manufacturer 0x0060
[00:00:01.042,000] <inf> bt_hci_core: LMP: version 4.2 (0x08) subver 0x030e
[00:00:01.042,000] <wrn> bt_id: Set privacy mode command is not supported
Connected E8:6D:CB:56:A4:85 (public)
Security changed: E8:6D:CB:56:A4:85 (public) level 4
[00:00:23.815,000] <wrn> bt_hci_driver_esp32: Controller not ready to receive packets
[00:00:23.897,000] <wrn> bt_att: Unhandled ATT code 0x1d
Disconnected from E8:6D:CB:56:A4:85 (public) (reason 0x13)