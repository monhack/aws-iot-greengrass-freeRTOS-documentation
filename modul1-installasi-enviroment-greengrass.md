## Install Enviroment IoT Greengrass pada Raspberry Pi

Instalasi dalam CLI
1. Pertama masuk ke ssh raspi dengan perintah:

	```
	$ ssh pi@{nomor IP raspi} 
	```

2. Tulis perintah seperti berikut:

	```
	$ sudo adduser --system ggc_user
	$ sudo addgroup --system ggc_group 
	```

3. Masuk ke folder ```/etc/sysctl.d``` dan edit file ```98-rpi.conf``` dengan perintah:

	```
	$ cd /etc/sysctl.d
	$ sudo nano 98-rpi.conf 
	```

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
	
11. To make sure that you have all required dependencies, download and run the Greengrass dependency checker from the <link>https://github.com/aws-samples/aws-greengrass-samples</link>. These commands unzip and run the dependency checker script in the ```Downloads``` directory

	```
	$ cd /home/pi/Downloads
	$ mkdir greengrass-dependency-checker-GGCv1.10.x
	$ cd greengrass-dependency-checker-GGCv1.10.x
	$ wget https://github.com/aws-samples/	aws-greengrass-samples/raw/master/greengrass-dependency-checker-GGCv1.10.x.zip
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