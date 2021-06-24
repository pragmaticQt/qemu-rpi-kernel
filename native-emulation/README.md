## Native emulation of Rpi2/3 using Qemu's Raspi2/3 machine

This section of repo contains a set of instructions and relevant files in order to emulate 
raspberry pi using Qemu's raspi2/raspi3 machine.

### Pre requisites

- Qemu 5.2
- Raspberry pi img file
- Raspberry pi kernel image
- Relevant dtb files

### Command to emulate

sudo qemu-system-arm \
    -M raspi2 \
    -drive file=2019-09-26-raspbian-buster.img,format=raw,if=sd \
    -dtb bcm2709-rpi-2-b.dtb \
    -kernel kernel7.img \
    -append 'rw earlycon=pl011,0x3f201000 console=ttyAMA0 \
        loglevel=8 root=/dev/mmcblk0p2 fsck.repair=yes \
        net.ifnames=0 rootwait memtest=1 \
        dwc_otg.fiq_fsm_enable=0' \
    -serial stdio -no-reboot \
    -usb -device usb-kbd -device usb-tablet \
    -netdev user,id=net0,hostfwd=tcp::5022-:22 \
    -device usb-net,netdev=net0



sudo qemu-system-aarch64 \
  -M raspi3 \
  -append "dwc_otg.lpm_enable=0 rw earlyprintk loglevel=8 console=ttyAMA0,115200 root=/dev/mmcblk0p2 rootdelay=1" \
  -dtb bcm2710-rpi-3-b-plus.dtb \
  -sd 2020-08-20-raspios-buster-armhf.img \
  -kernel kernel8.img \
  -m 1G \
  -smp 4 \
  -serial stdio \
  -usb \
  -device usb-mouse \
  -device usb-tablet \
  -device usb-kbd \
  -netdev user,id=net0,hostfwd=tcp::5022-:22 \
  -device usb-net,netdev=net0
