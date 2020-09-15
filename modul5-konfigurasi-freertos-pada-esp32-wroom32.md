## Kofigurasi amazon freertos pada ESP32 WROOM32
- <b>Note: harus setting permission terlebih dahulu di [IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/) yaitu ```AmazonFreeRTOSFullAccess``` dan ```AWSIoTFullAccess```, lalu harus melakukan langkah langkah pada modul 1-4</b>
- <b>Device yang digunakan yaitu ESP32-DevKitC</b>

1. Konfigurasi Espressif Hardware di [Setting Espressif Hardware](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html#step-1-install-prerequisites), lakukan semua instruksinya

2. Install toolchain terlebih dahulu di [Toolchain Linux](https://docs.espressif.com/projects/esp-idf/en/v3.3/get-started-cmake/linux-setup.html):

3. Buka link [freeRTOS Console](https://console.aws.amazon.com/freertos), lalu download file freertosnya

  	![AddExtService](images/freertos_download.png)

4. Tulis 2 baris perintah diakhir file

	```
	fs.protected_hardlinks = 1
	fs.protected_symlinks = 1 
	```

5. Re-boot raspi dengan perintah:

	```
	$ sudo reboot 
	```

6. Setelah beberapa menit, masuk ke ssh raspi dan tulis perintah berikut:

	```
	$ sudo sysctl -a 2> /dev/null | grep fs.protected 
	```

7. Masuk ke folder ```/boot``` dengan perintah:

	```
	$ cd /boot/ 
	```

8. Buka file ```cmdline.txt```, tambahkan perintah di kalimat terakhir, tidak di baris baru

	```
	cgroup_enable=memory cgroup_memory=1 
	```

9. Re-boot raspi dengan perintah:

	```
	$ sudo reboot 
	```

10. Install Java 8 runtime untuk kebutuhan ```Stream Manager``` dengan perintah:

	```
	$ sudo apt install openjdk-8-jdk 
	```
	
11. Download script checker untuk mengecek dependensi apa saja yang terpenuhi di <link>https://github.com/aws-samples/aws-greengrass-samples</link>. lalu ekstrak di folder ```Downloads``` dengan perintah:

	```
	$ cd /home/pi/Downloads
	$ mkdir greengrass-dependency-checker-GGCv1.10.x
	$ cd greengrass-dependency-checker-GGCv1.10.x
	$ wget https://github.com/aws-samples/aws-greengrass-samples/raw/master/greengrass-dependency-checker-GGCv1.10.x.zip
	$ unzip greengrass-dependency-checker-GGCv1.10.x.zip
	$ cd greengrass-dependency-checker-GGCv1.10.x
	$ sudo modprobe configs
	$ sudo ./check_ggc_dependencies | more 
	```

12. Jika Java 8 atau NodeJS 12 tidak kedeteksi, maka harus merubah nama binaries fungsi ```java``` dan ```node``` dengan perintah:

	```
	NodeJS:
	$ cd /usr/bin
	$ sudo mv node nodejs12.x
	Java:
	$ cd /usr/bin
	$ sudo mv java java8
	```