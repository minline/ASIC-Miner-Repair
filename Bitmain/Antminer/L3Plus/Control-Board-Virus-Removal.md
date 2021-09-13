# Control Board Virus Removal

### Download the [201904231425-L3++-SD-recover-NAND.zip](./201904231425-L3++-SD-recover-NAND.zip) file, and extract the zip.

### Use [balenaEtcher](https://www.balena.io/etcher) to flash a MicroSD card with the `201904231425-L3++-SD-recover-NAND.img` image file.

### Short circuit the two pins on the control board as shown in the image

<img src="./Control-Board-Virus-Removal-Short-Circuit.png" width="600px">

### Insert the flashed MicroSD card, turn on the power, wait for about 30 seconds, once the indicator lights start blinking, the flash was successful, turn off the control board, remove the short circuit wire, now flash the board with your desired firmware.
