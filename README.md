# Pico W sensor

### Installation

 * Install pico-sdk from [the official github repo](https://github.com/raspberrypi/pico-sdki)
 * Switch to the project dir: `cd picow_sensor`
 * Generate the project using CMake: `cmake -DPICO_BOARD=pico_w -DPICO_SDK_PATH=/home/francisco/Documents/pico-sdk/ -DTEST_TCP_SERVER_IP=192.168.0.10 .`
 * Edit the WIFI credentials to match your network: `vim src/wifi_credentials.h`
 * Compile the project: `make`
 * Copy the resulting uf2 executable to the Raspberry Pico W mass storage device: `cp picow_tcpip_client_poll.uf2 /run/media/${USER}/RPI-RP2/`
 * The Pico should reboot and run the executable
