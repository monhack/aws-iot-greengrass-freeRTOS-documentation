## Cmake program amazon freertos ESP32 WROOM32
<b>Note:</b>
- <b>harus setting permission terlebih dahulu di [IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/) yaitu ```AmazonFreeRTOSFullAccess``` dan ```AWSIoTFullAccess```, lalu harus melakukan langkah langkah pada modul 1-5</b>
- <b>Device yang digunakan yaitu ESP32-DevKitC</b>
- <b>Semua step pada modul 6 dilakukan pada PC/laptop</b>

1. Masuk ke folder freertos terlebih dahulu, lalu setting program mana yang akan dicmake (sesuaikan dengan folder masing masing)

	```
	$ cd ~/Downloads/freertos/vendors/espressif/boards/esp32/aws_demos/config_files/
	$ nano aws_demo_config.h -> bebas pake text editor apa aja
	```

2. Pilih program mana yang mau dicmake ke esp32 nya (baris 26-45). Pada riset ini kita menggunakan ```greengrass_connectity```, berarti kita define saja bagian ```CONFIG_GREENGRASS_DISCOVERY_DEMO_ENABLED``` dan comment bagian ```CONFIG_MQTT_DEMO_ENABLED```

  	```
	#ifndef _AWS_DEMO_CONFIG_H_
	#define _AWS_DEMO_CONFIG_H_
	/* To run a particular demo you need to define one of these.
 	* Only one demo can be configured at a time
 	*
 	*          CONFIG_MQTT_DEMO_ENABLED
 	*          CONFIG_SHADOW_DEMO_ENABLED
 	*          CONFIG_GREENGRASS_DISCOVERY_DEMO_ENABLED
 	*          CONFIG_TCP_ECHO_CLIENT_DEMO_ENABLED
 	*          CONFIG_DEFENDER_DEMO_ENABLED
 	*          CONFIG_OTA_UPDATE_DEMO_ENABLED
 	*          CONFIG_BLE_GATT_SERVER_DEMO_ENABLED
 	*          CONFIG_HTTPS_SYNC_DOWNLOAD_DEMO_ENABLED
 	*          CONFIG_HTTPS_ASYNC_DOWNLOAD_DEMO_ENABLED
 	*          CONFIG_HTTPS_SYNC_UPLOAD_DEMO_ENABLED
 	*          CONFIG_HTTPS_ASYNC_UPLOAD_DEMO_ENABLED
 	*
 	*  These defines are used in iot_demo_runner.h for demo selection */
	#define CONFIG_GREENGRASS_DISCOVERY_DEMO_ENABLED
	/* #define CONFIG_MQTT_DEMO_ENABLED */
	```

3. Setelah itu baru ```cmake``` programnya
	<b>Note: [Cmake buat windows](https://docs.aws.amazon.com/freertos/latest/userguide/building-cmake.html)</b>

	```
	$ cd ~/Downloads/freertos/
	$ sudo cmake -DVENDOR=espressif -DBOARD=esp32_devkitc -DCOMPILER=xtensa-esp32 -DCMAKE_TOOLCHAIN_FILE='~/Downloads/FreeRTOS/tools/cmake/toolchains/xtensa-esp32.cmake' -DCMAKE_C_COMPILER='~/esp/xtensa-esp32-elf/bin/xtensa-esp32-elf-gcc' -DCMAKE_CXX_COMPILER='~/esp/xtensa-esp32-elf/bin/xtensa-esp32-elf-g++' -DCMAKE_ASM_COMPILER='~/esp/xtensa-esp32-elf/bin/xtensa-esp32-elf-gcc' -DAFR_COMPILER_CC='~/esp/xtensa-esp32-elf/bin/xtensa-esp32-elf-gcc' -DAFR_COMPILER_CXX='~/esp/xtensa-esp32-elf/bin/xtensa-esp32-elf-g++' -DCMAKE_MAKE_PROGRAM='/home/{username}/.espressif/tools/ninja' -DAFR_BOARD="espressif.esp32_devkitc" -S . -B esp32_gg_1 -> (nama-directorynya-yang-mau-dibuat)
	```

4. Setelah dicmake, kita setting terlebih dahulu path untuk ```CC, CXX dan ASM``` (harus masuk ke superuser karena ada beberapa file yang diubah dan harus menjadi root)

	```
	$ sudo su
	$ export CC="$PATH:/home/monhack/esp/xtensa-esp32-elf/bin/xtensa-esp32-elf-gcc"
	$ export CXX="$PATH:/home/monhack/esp/xtensa-esp32-elf/bin/xtensa-esp32-elf-g++"
	$ export ASM="$PATH:/home/monhack/esp/xtensa-esp32-elf/bin/xtensa-esp32-elf-gcc" 
	```

6. Setelah setting path, selanjutnya ```make``` program:

	```
	$ cd esp32_gg_1
	$ make all -j4
	```

7. Sambungkan ESP32 dengan PC/laptop, lalu lakukan hal berikut
	<b>Note: Jalankan program trafficLight.py pada raspi agar freertos bisa ngirim data tanpa internet. Perintah ada pada modul4 langkah 11</b>

	```
	$ cd ../
	$ sudo python ~/Documents/Magang/FreeRTOS/vendors/espressif/esp-idf/tools/idf.py erase_flash -p /dev/ttyUSB0 -B esp32_gg_1/
	$ sudo python ~/Documents/Magang/FreeRTOS/vendors/espressif/esp-idf/tools/idf.py flash -p /dev/ttyUSB0 -B esp32_gg_1/
	$ sudo python ~/Documents/Magang/FreeRTOS/vendors/espressif/esp-idf/tools/idf.py monitor -p /dev/ttyUSB0 -B esp32_gg_1/ 
	```

8. Jika programnya berhasil jalan, data akan masuk ke raspi atau IoT Core