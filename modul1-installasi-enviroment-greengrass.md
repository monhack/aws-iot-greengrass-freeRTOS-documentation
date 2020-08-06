## Install enviroment iot greengrass pada raspberry pi

Instalasi dalam CLI
1. Pertama masuk ke ssh raspi

2. Masukin inputan seperti berikut:

	```
	$ sudo adduser --system ggc_user \
	$ sudo addgroup --system ggc_group 
	```

3. Masuk ke folder ```/etc/sysctl.d``` dan edit file ```98-rpi.conf``` dengan perintah:

	```
	$ cd /etc/sysctl.d
	$ sudo nano 98-rpi.conf 
	```

4. Masukin 2 baris perintah diakhir file

	```
	fs.protected_hardlinks = 1
	fs.protected_symlinks = 1 
	```

5. Setelah itu Re-boot raspi dengan perintah:

	```
	$ sudo reboot 
	```

6. Setelah beberapa menit, masuk ke ssh raspi dan masukan perintah berikut:

	```
	$ sudo sysctl -a 2> /dev/null | grep fs.protected 
	```

7. Selanjutnya masuk ke folder ```/boot``` dengan perintah:

	```
	$ cd /boot/ 
	```

8. Buka file ```cmdline.txt```, tambahkan perintah di kalimat terakhir, tidak di baris baru

	```
	cgroup_enable=memory cgroup_memory=1 
	```

9. Setelah itu Re-boot raspi dengan perintah:

	```
	$ sudo reboot 
	```

10. Install Java 8 runtime untuk kebutuhan ```stream manager``` dengan perintah:

	```
	$ sudo apt install openjdk-8-jdk 
	```
	
11. To make sure that you have all required dependencies, download and run the Greengrass dependency checker from the <link>https://github.com/aws-samples/aws-greengrass-samples</link>. These commands unzip and run the dependency checker script in the ```Downloads``` directory

	```
	cd /home/pi/Downloads
	mkdir greengrass-dependency-checker-GGCv1.10.x
	cd greengrass-dependency-checker-GGCv1.10.x
	wget https://github.com/aws-samples/	aws-greengrass-samples/raw/master/greengrass-dependency-checker-GGCv1.10.x.zip
	unzip greengrass-dependency-checker-GGCv1.10.x.zip
	cd greengrass-dependency-checker-GGCv1.10.x
	sudo modprobe configs
	sudo ./check_ggc_dependencies | more 
	```