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
export ESPRESSIF_TOOLCHAIN_PATH="/Users/melka/.espressif/tools/zephyr"

export PATH="${HOME}/.espressif/tools/xtensa-esp32-elf/esp-2020r3-8.4.0/xtensa-esp32-elf/bin/:$PATH" 

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

I (26) boot: ESP-IDF release/v4.3-116-gbd2e2d8dc 2nd stage bootloader
I (26) boot: compile time 23:08:52
I (27) boot: chip revision: 1
I (31) boot_comm: chip revision: 1, min. bootloader chip revision: 0
I (47) boot.esp32: SPI Speed      : 40MHz
I (47) boot.esp32: SPI Mode       : DIO
I (47) boot.esp32: SPI Flash Size : 4MB
I (52) boot: Enabling RNG early entropy source...
I (57) boot: Partition Table:
I (61) boot: ## Label            Usage          Type ST Offset   Length
I (68) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (75) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (83) boot:  2 factory          factory app      00 00 00010000 00100000
I (90) boot: End of partition table
I (95) boot_comm: chip revision: 1, min. application chip revision: 0
I (102) esp_image: segment 0: paddr=00010020 vaddr=3f400020 size=05064h ( 20580) map
I (118) esp_image: segment 1: paddr=0001508c vaddr=3ffbdb5c size=00340h (   832) load
I (119) esp_image: segment 2: paddr=000153d4 vaddr=3ffbdea0 size=01118h (  4376) load
I (129) esp_image: segment 3: paddr=000164f4 vaddr=3ffbefb8 size=00178h (   376) load
I (136) esp_image: segment 4: paddr=00016674 vaddr=40080000 size=00400h (  1024) load
I (145) esp_image: segment 5: paddr=00016a7c vaddr=40080400 size=0959ch ( 38300) load
I (169) esp_image: segment 6: paddr=00020020 vaddr=400d0020 size=287e4h (165860) map
I (232) esp_image: segment 7: paddr=0004880c vaddr=4008999c size=077b8h ( 30648) load
I (254) boot: Loaded app from partition at offset 0x10000
I (254) boot: Disabling RNG early entropy source...
*** Booting Zephyr OS build cd0742651723  ***
Bluetooth initialized
Advertising successfully started
[00:00:00.542,000] <inf> fs_nvs: 6 Sectors of 4096 bytes
[00:00:00.543,000] <inf> fs_nvs: alloc wra: 5, fd0
[00:00:00.543,000] <inf> fs_nvs: data wra: 5, 1c
[00:00:00.543,000] <inf> esp32_bt_adapter: BT controller compile version [1342a48]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .data initialise [0x3ffae6e0] <== [0x4000d890]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffb0000] - [0x3ffb09a8]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffb09a8] - [0x3ffb1ddc]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffb1ddc] - [0x3ffb2730]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffb2730] - [0x3ffb6388]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffb8000] - [0x3ffb9a20]
[00:00:00.543,000] <dbg> esp32_bt_adapter.btdm_controller_mem_init: .bss initialise [0x3ffbdb28] - [0x3ffbdb5c]
[00:00:01.007,000] <inf> bt_hci_core: No ID address. App must call settings_load()
[00:00:01.008,000] <inf> bt_hci_core: Identity: A4:CF:12:24:CD:52 (public)
[00:00:01.008,000] <inf> bt_hci_core: HCI: version 4.2 (0x08) revision 0x030e, manufacturer 0x0060
[00:00:01.008,000] <inf> bt_hci_core: LMP: version 4.2 (0x08) subver 0x030e